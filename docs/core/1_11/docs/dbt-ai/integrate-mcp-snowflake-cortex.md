# Integrate Snowflake Cortex agents with dbt MCP

[Snowflake Cortex agents](https://docs.snowflake.com/en/user-guide/snowflake-cortex/cortex-agents) can call external [MCP servers](https://docs.snowflake.com/en/user-guide/snowflake-cortex/cortex-agents-mcp-connectors) as tools. This guide walks you through connecting a Cortex agent to the remote dbt MCP server so it can query your Semantic Layer metrics and dimensions in plain English from Snowflake Intelligence.

The connection uses OAuth: Snowflake registers itself with dbt platform through Dynamic Client Registration (DCR) with PKCE, so you don't store a client secret in Snowflake. Each user completes their own OAuth consent the first time they use the agent, which means the agent respects each user's existing dbt permissions and project access.

## Prerequisites[​](#prerequisites "Direct link to Prerequisites")

Before connecting a Cortex agent to the remote dbt MCP server, make sure you have the following in place.

#### In dbt platform[​](#in- "Direct link to in-")

Make sure the following are set up before connecting from Snowflake:

* **Account setup**

  * Have [AI features](https://docs.getdbt.com/docs/platform/enable-dbt-ai.md) enabled.
  * [Remote MCP OAuth enabled](https://docs.getdbt.com/docs/dbt-ai/setup-remote-mcp.md). The remote MCP server is generally available, but the OAuth connection method is in public beta for Starter and Enterprise-tiered accounts.
  * A [static subdomain](https://docs.getdbt.com/docs/platform/about-platform/access-regions-ip-addresses.md) configured, for example `abc123` in `abc123.us1.dbt.com`. If your account doesn't have a subdomain, contact support.

* **Access and permissions**
  * Read-only or higher access to dbt platform. The agent inherits each connected user's permissions.

* **Semantic Layer setup**
  * A configured [Semantic Layer](https://docs.getdbt.com/docs/use-dbt-semantic-layer/dbt-sl.md) with [metrics and dimensions](https://docs.getdbt.com/docs/build/build-metrics-intro.md).

* **MCP endpoint**

  You can copy your full **MCP URL** from **Account settings** → **Access URLs** → **MCP Endpoint URL** in dbt platform, and paste it directly into your AI tool.

   Build your own MCP URL

  We recommend using the MCP URL from **Account settings** → **Access URLs** → **MCP Endpoint URL** in dbt platform. However, if you want to build your own MCP URL, use your **Access URL** from **Account settings** in dbt platform. The remote MCP endpoint is `https://YOUR_DBT_HOST_URL/api/ai/v1/mcp`. Replace `YOUR_DBT_HOST_URL` with your hostname only (no `https://`).

  For default hosts, multi-cell accounts, and regions, see [Access, Regions, & IP addresses](https://docs.getdbt.com/docs/platform/about-platform/access-regions-ip-addresses.md).

  <!-- -->

  Use the host portion of this URL in the Snowflake SQL, for example `abc123.us1.dbt.com`.

#### In Snowflake[​](#in-snowflake "Direct link to In Snowflake")

* Snowflake Intelligence and Cortex agents enabled in your account and region.
* External MCP connectors available for your account. Confirm availability with your Snowflake account team.
* The `ACCOUNTADMIN` role, or a role with account-level `CREATE INTEGRATION`.
* A database, schema, and role for creating the MCP server and agent.

Optional: native SQL execution

Cortex agents can compile and execute Semantic Layer queries without extra setup. To also allow ad hoc SQL against Snowflake, configure project read credentials in **Settings → Credentials** in dbt platform. Without those credentials, the agent can still compile Semantic Layer SQL and execute it natively on Snowflake.

## Parameters[​](#parameters "Direct link to Parameters")

The SQL on this page uses placeholders. Replace each one with your own value before running:

| Placeholder                     | Description                                                                    |
| ------------------------------- | ------------------------------------------------------------------------------ |
| `YOUR_DBT_HOST_URL`             | Your dbt host, with no `https://` (for example, `abc123.us1.dbt.com`).         |
| `INTEGRATION_NAME`              | A name for the Snowflake API integration (for example, `dbt_mcp_integration`). |
| `TARGET_DATABASE.TARGET_SCHEMA` | The database and schema where the MCP server and agent live.                   |
| `TARGET_ROLE`                   | The Snowflake role that uses the agent and MCP server.                         |
| `MCP_SERVER_NAME`               | A name for the external MCP server object.                                     |
| `AGENT_NAME`                    | A name for the Cortex agent.                                                   |
| `CORTEX_MODEL`                  | The orchestration model for the agent (for example, `claude-4-sonnet`).        |

## Set up the connection[​](#set-up-the-connection "Direct link to Set up the connection")

The following steps register the remote dbt MCP server with Snowflake and connect it to a Cortex agent. Steps 1–3 run as Snowflake SQL. Step 4 starts in Snowflake Intelligence and redirects each user to dbt platform to complete OAuth authorization.

### Step 1: Create the API integration[​](#step-1-create-the-api-integration "Direct link to Step 1: Create the API integration")

In Snowflake, run this as `ACCOUNTADMIN`. The integration tells Snowflake how to reach the remote dbt MCP endpoint and how to complete OAuth using Dynamic Client Registration with PKCE (no client secret).

```sql
-- Create an API integration for the remote dbt MCP server.
-- Uses OAuth Dynamic Client Registration + PKCE (no client secret).
CREATE API INTEGRATION IF NOT EXISTS INTEGRATION_NAME
  API_PROVIDER = EXTERNAL_MCP
  API_ALLOWED_PREFIXES = ('https://YOUR_DBT_HOST_URL/api/ai/v1/mcp')
  API_USER_AUTHENTICATION = (
    TYPE = OAUTH_DYNAMIC_CLIENT
    OAUTH_CLIENT_AUTH_METHOD = NONE
    OAUTH_TOKEN_ENDPOINT = 'https://YOUR_DBT_HOST_URL/oauth/token'
    OAUTH_AUTHORIZATION_ENDPOINT = 'https://YOUR_DBT_HOST_URL/oauth/authorize'
    OAUTH_RESOURCE_URL = 'https://YOUR_DBT_HOST_URL/api/ai/v1/mcp'
    OAUTH_ALLOWED_SCOPES = ('user_access', 'offline_access')
  )
  ENABLED = TRUE;
```

The `OAUTH_TOKEN_ENDPOINT` and `OAUTH_AUTHORIZATION_ENDPOINT` use the same host as your MCP URL. The `user_access` and `offline_access` scopes let the agent act on your behalf and refresh its session without you re-authenticating each time.

### Step 2: Create the external MCP server[​](#step-2-create-the-external-mcp-server "Direct link to Step 2: Create the external MCP server")

In Snowflake, grant your role the ability to create an external MCP server, then create one that references the integration from Step 1.

```sql
GRANT CREATE EXTERNAL MCP SERVER ON SCHEMA TARGET_DATABASE.TARGET_SCHEMA TO ROLE TARGET_ROLE;

-- Create an external MCP server pointing to the remote dbt MCP endpoint.
CREATE EXTERNAL MCP SERVER IF NOT EXISTS TARGET_DATABASE.TARGET_SCHEMA.MCP_SERVER_NAME
  WITH DISPLAY_NAME = 'dbt Semantic Layer MCP'
  URL = 'https://YOUR_DBT_HOST_URL/api/ai/v1/mcp'
  API_INTEGRATION = INTEGRATION_NAME;
```

### Step 3: Create the Cortex agent[​](#step-3-create-the-cortex-agent "Direct link to Step 3: Create the Cortex agent")

In Snowflake, grant your role the privileges to create an agent and to use the MCP server and its integration, then create the agent.

```sql
GRANT CREATE AGENT ON SCHEMA TARGET_DATABASE.TARGET_SCHEMA TO ROLE TARGET_ROLE;

-- Grant the role access to the MCP server and its underlying integration.
GRANT USAGE ON EXTERNAL MCP SERVER TARGET_DATABASE.TARGET_SCHEMA.MCP_SERVER_NAME TO ROLE TARGET_ROLE;
GRANT USAGE ON INTEGRATION INTEGRATION_NAME TO ROLE TARGET_ROLE;

CREATE AGENT IF NOT EXISTS TARGET_DATABASE.TARGET_SCHEMA.AGENT_NAME
  COMMENT = 'Analytics agent powered by the dbt Semantic Layer via MCP'
  PROFILE = '{"display_name": "dbt Semantic Layer Agent"}'
  FROM SPECIFICATION
  $$
  models:
    orchestration: "CORTEX_MODEL"

  instructions:
    response: 'Answer questions about business data using the dbt Semantic Layer. Present results clearly in plain English with context about what the metrics mean. When data is returned, summarize the key insight before showing details.'
    orchestration: 'Always use the dbt MCP tools to query metrics and dimensions rather than writing raw SQL. First explore available metrics and dimensions if you are unsure what is available, then construct and execute the appropriate semantic layer query.'
    sample_questions:
      - question: 'What are the top 10 products by revenue this quarter?'
      - question: 'How has inventory turnover trended over the last 12 months?'
      - question: 'Which regions have the highest order volume?'

  mcp_servers:
    - server_spec:
        name: "TARGET_DATABASE.TARGET_SCHEMA.MCP_SERVER_NAME"
  $$;
```

Update the `instructions` and `sample_questions` to match the metrics and dimensions in your own Semantic Layer. The `orchestration` instruction steers the agent toward the dbt Semantic Layer tools (like `list_metrics`, `get_dimensions`, and `query_metrics`) instead of writing raw SQL.

### Step 4: Complete the OAuth flow[​](#step-4-complete-the-oauth-flow "Direct link to Step 4: Complete the OAuth flow")

The agent can't query dbt until each user authorizes it. Complete the OAuth flow once per user:

1. In the Snowflake Intelligence user interface, open your MCP connectors. Depending on your Snowflake version, this is under **Settings → User → MCP Connectors**, or in the **Connectors** panel of the agent's sources.
2. Find the **dbt Semantic Layer MCP** connector you created and select **Connect**.
3. Snowflake redirects you to dbt platform to sign in and approve the requested [scopes](https://docs.getdbt.com/docs/platform/manage-access/connect-apps-oauth.md#scopes-and-consent) on the consent screen. You can scope the connection to a specific project (recommended) so the agent only sees that project's data.
4. After you approve, the connector shows as **Connected** and you're returned to Snowflake.

Snowflake self-registers with dbt platform through Dynamic Client Registration on first connect, so no admin action is needed to register it.

In dbt platform, admins can review and audit the connected client, and manage sessions and scopes, in **Account settings → Integrations → App integrations**. For the full registration, consent, and session model, see [Connect apps with OAuth](https://docs.getdbt.com/docs/platform/manage-access/connect-apps-oauth.md).

## Verify the connection[​](#verify-the-connection "Direct link to Verify the connection")

Open your agent in Snowflake Intelligence and ask one of its sample questions, such as *"What are the top 10 products by revenue this quarter?"* If the connection is working, the agent calls the dbt Semantic Layer tools and returns an answer grounded in your metrics.

info

Only [`text_to_sql`](https://docs.getdbt.com/docs/dbt-ai/mcp-available-tools.md) consumes your dbt Copilot action allotment. Other MCP tools do not.

When your account runs out of dbt Copilot actions, the remote MCP server blocks every tool that runs through it, including tools invoked from a self-hosted MCP server and [proxied](https://github.com/dbt-labs/dbt-mcp/blob/main/src/dbt_mcp/tools/toolsets.py#L24) to remote MCP, such as SQL and remote Fusion tools.

If you reach your dbt Copilot actions limit, remote MCP tools remain unavailable until the limit resets. If you need help, contact your account manager.

## Troubleshooting[​](#troubleshooting "Direct link to Troubleshooting")

 The connector won't authorize or the OAuth flow fails

* Confirm your account has a [static subdomain](https://docs.getdbt.com/docs/platform/about-platform/access-regions-ip-addresses.md). OAuth with MCP requires one.
* Verify the host in `API_ALLOWED_PREFIXES`, `OAUTH_TOKEN_ENDPOINT`, `OAUTH_AUTHORIZATION_ENDPOINT`, and `OAUTH_RESOURCE_URL` all match your MCP URL host exactly, and that the integration `ENABLED = TRUE`.
* Make sure [AI features](https://docs.getdbt.com/docs/platform/enable-dbt-ai.md) are enabled and that remote MCP OAuth is available for your account tier.

 The agent returns no metrics or empty results

* Confirm the project you authorized has a configured [Semantic Layer](https://docs.getdbt.com/docs/use-dbt-semantic-layer/dbt-sl.md) with metrics and dimensions.
* Check that the user who connected has at least read-only access to that project — the agent only sees what the user can see.
* If you scoped the OAuth connection to a single project, make sure it's the project that contains your metrics.

 Permission errors when creating the integration or MCP server

`CREATE API INTEGRATION` and `CREATE EXTERNAL MCP SERVER` require `ACCOUNTADMIN` (or a role with account-level `CREATE INTEGRATION`). Run Steps 1–2 as `ACCOUNTADMIN`, then grant `USAGE` on the MCP server and integration to the role that runs the agent (Step 3).

## Related docs[​](#related-docs "Direct link to Related docs")

* [Set up the remote MCP server](https://docs.getdbt.com/docs/dbt-ai/setup-remote-mcp.md)
* [Available MCP tools](https://docs.getdbt.com/docs/dbt-ai/mcp-available-tools.md)
* [Connect apps with OAuth](https://docs.getdbt.com/docs/platform/manage-access/connect-apps-oauth.md)
* [Snowflake: MCP Connectors](https://docs.snowflake.com/en/user-guide/snowflake-cortex/cortex-agents-mcp-connectors)
