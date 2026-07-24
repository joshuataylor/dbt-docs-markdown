# Set up remote MCP

dbt platformⓘ

The remote MCP server uses an HTTP connection and makes calls to dbt-mcp hosted on the cloud-based dbt platform. The self-hosted installation is not required for remote MCP use and is ideal for data consumption use cases.

[![Remote dbt MCP server architecture](/img/mcp/remote-dbt-mcp.jpg?v=2 "Remote dbt MCP server architecture")](#)Remote dbt MCP server architecture

## When to use remote MCP[​](#when-to-use-remote-mcp "Direct link to When to use remote MCP")

The remote MCP server is the ideal choice when:

* You don't want to or are restricted from installing additional software (`uvx`, `dbt-mcp`) on your system.
* Your primary use case is *consumption-based*: querying metrics, exploring metadata, viewing lineage.
* You need access to Semantic Layer, Administrative, and Discovery APIs without maintaining a local dbt project.
* You don't need to execute CLI commands. Remote MCP does not support self-hosted dbt CLI commands (`dbt run`, `dbt build`, `dbt test`, and more). If you need to execute dbt commands, use the [self-hosted MCP server](https://docs.getdbt.com/docs/dbt-ai/setup-local-mcp.md) instead.

info

Only [`text_to_sql`](https://docs.getdbt.com/docs/dbt-ai/mcp-available-tools.md) consumes your dbt Copilot action allotment. Other MCP tools do not.

When your account runs out of dbt Copilot actions, the remote MCP server blocks every tool that runs through it, including tools invoked from a self-hosted MCP server and [proxied](https://github.com/dbt-labs/dbt-mcp/blob/main/src/dbt_mcp/tools/toolsets.py#L24) to remote MCP, such as SQL and remote Fusion tools.

If you reach your dbt Copilot actions limit, remote MCP tools remain unavailable until the limit resets. If you need help, contact your account manager.

## Choose your auth method[​](#choose-your-auth-method "Direct link to Choose your auth method")

| If you need...                                                               | Use...                                                                                             |
| ---------------------------------------------------------------------------- | -------------------------------------------------------------------------------------------------- |
| Fastest first-time setup and your MCP client supports OAuth for HTTP servers | **OAuth (remote)**<br />Available in public beta for Starter, Enterprise, and Enterprise+ accounts |
| `execute_sql` with a PAT, automation, shared setup, or clients without OAuth | **Token-based** (PAT or service token)                                                             |
| Shared or team setup                                                         | **Service token** (token-based)                                                                    |
| CI or automation                                                             | **Service token** (token-based)                                                                    |

Search table...

|                  |   |   |   |   |
| ---------------- | - | - | - | - |
| Loading table... |   |   |   |   |

`execute_sql` requires a PAT

The `execute_sql` tool does **not** work with service tokens. You must use a [Personal Access Token (PAT)](https://docs.getdbt.com/docs/dbt-apis/user-tokens.md) in the `Authorization` header when using this tool.

## OAuth (remote MCP) [Beta](https://docs.getdbt.com/docs/dbt-versions/product-lifecycles "Go to https://docs.getdbt.com/docs/dbt-versions/product-lifecycles")[Starter](https://www.getdbt.com/pricing "Go to https://www.getdbt.com/pricing")[Enterprise](https://www.getdbt.com/pricing "Go to https://www.getdbt.com/pricing")[Enterprise +](https://www.getdbt.com/pricing "Go to https://www.getdbt.com/pricing")[​](#oauth-remote-mcp "Direct link to oauth-remote-mcp")

OAuth lets you connect to the remote MCP server without copying API tokens into your MCP client, when your client supports OAuth for HTTP-based MCP servers.

### Prerequisites[​](#prerequisites "Direct link to Prerequisites")

* [AI features](https://docs.getdbt.com/docs/cloud/enable-dbt-copilot) enabled for your account.
* Starter, Enterprise, or Enterprise+ account
* An MCP client that supports OAuth for remote (HTTP) MCP servers.
* Your **MCP URL** from **Account settings** → **Access URLs** → **MCP Endpoint URL** in dbt platform. Check out the next section [MCP URL](#mcp-url) for more information.

### MCP URL[​](#mcp-url "Direct link to MCP URL")

You can copy your full **MCP URL** from **Account settings** → **Access URLs** → **MCP Endpoint URL** in dbt platform, and paste it directly into your AI tool.

 Build your own MCP URL

We recommend using the MCP URL from **Account settings** → **Access URLs** → **MCP Endpoint URL** in dbt platform. However, if you want to build your own MCP URL, use your **Access URL** from **Account settings** in dbt platform. The remote MCP endpoint is `https://YOUR_DBT_HOST_URL/api/ai/v1/mcp`. Replace `YOUR_DBT_HOST_URL` with your hostname only (no `https://`).

For default hosts, multi-cell accounts, and regions, see [Access, Regions, & IP addresses](https://docs.getdbt.com/docs/platform/about-platform/access-regions-ip-addresses.md).

### How it works[​](#how-it-works "Direct link to How it works")

1. In your MCP client, navigate to the connector or integrations settings and add your MCP URL (see [MCP URL](#mcp-url)). For example, if you used Claude or ChatGPT, you would go to:

   <!-- -->

   * **Claude (web)**: **Customize** → **Connectors** → **+** → **Add custom connector**
   * **ChatGPT**: **Settings** → **Apps** → **Create App**
   * For Claude Desktop and Claude Code, see [Integrate Claude with dbt MCP](https://docs.getdbt.com/docs/dbt-ai/integrate-mcp-claude.md).

2. When prompted in your MCP client, complete sign-in in the browser and approve the requested scopes on the consent screen.

3. Return to your MCP client; subsequent requests use the OAuth session according to your client's behavior.

### Register your MCP client[​](#register-your-mcp-client "Direct link to Register your MCP client")

OAuth requires every client to be registered with dbt platform. There are two paths:

* **Dynamic registration (default)** — Clients that implement [Dynamic Client Registration (RFC 7591)](https://docs.getdbt.com/docs/platform/manage-access/connect-apps-oauth.md#dynamic-registration) self-register the first time a user connects. No admin action required. Most modern MCP clients (such as Claude Desktop, Claude Code, Cursor, and VS Code) support this.
* **Manual registration** — For clients that don't support dynamic registration, an account admin registers the client in **Account settings → Integrations → App integrations**. Manually registered clients use [PKCE (RFC 7636)](https://docs.getdbt.com/docs/platform/manage-access/connect-apps-oauth.md#manual-registration) instead of a client secret. See [Manual registration](https://docs.getdbt.com/docs/platform/manage-access/connect-apps-oauth.md#manual-registration) for the admin walkthrough.

Both types appear in **App integrations** in dbt platform, where admins can review and audit each connected client.

### Scopes you'll consent to[​](#scopes-youll-consent-to "Direct link to Scopes you'll consent to")

The first time you connect, dbt shows a consent screen listing the scopes (the specific permissions the client is allowed to use) the MCP client is requesting. Scopes act as a **filter on your existing permissions** — they don't grant new access. You can also choose whether the client gets access to all projects or only selected projects.

For the full list of scopes and what each one allows, see [Scopes and consent](https://docs.getdbt.com/docs/platform/manage-access/connect-apps-oauth.md#scopes-and-consent). For sessions, refresh tokens, revoking access, and audit logging, see [Connect apps with OAuth](https://docs.getdbt.com/docs/platform/manage-access/connect-apps-oauth.md).

### Limitations[​](#limitations "Direct link to Limitations")

* Remote MCP doesn't support self-hosted dbt CLI commands (like `dbt run`, `dbt build`, `dbt test`, and more) or local project access; use the [self-hosted MCP server](https://docs.getdbt.com/docs/dbt-ai/setup-local-mcp.md) for those workflows.

For client-specific steps, see [Integrate Claude with MCP](https://docs.getdbt.com/docs/dbt-ai/integrate-mcp-claude.md), [Integrate Cursor with MCP](https://docs.getdbt.com/docs/dbt-ai/integrate-mcp-cursor.md), [INtegrate Snowflake Cortex with MCP](https://docs.getdbt.com/docs/dbt-ai/integrate-mcp-snowflake-cortex.md), or [Integrate VS Code with MCP](https://docs.getdbt.com/docs/dbt-ai/integrate-mcp-vscode.md).

## Token-based authentication[​](#token-based-authentication "Direct link to Token-based authentication")

Token-based authentication lets you connect to the remote MCP server without OAuth by passing a PAT or service token in your MCP client config. Use it when your client doesn't support OAuth for HTTP-based MCP servers, when you need a shared or CI setup, or when you need `execute_sql`, which requires a PAT.

### Setup instructions[​](#setup-instructions "Direct link to Setup instructions")

1. Ensure that you have [AI features](https://docs.getdbt.com/docs/platform/enable-dbt-ai) turned on.
2. Obtain the following information from dbt platform:

* **dbt platform host**: Use this to form the full URL. For example, replace `YOUR_DBT_HOST_URL` here: `https://YOUR_DBT_HOST_URL/api/ai/v1/mcp/`. It may look like: `https://cloud.getdbt.com/api/ai/v1/mcp/`. If you have a multi-cell account, the host URL will be in the `ACCOUNT_PREFIX.us1.dbt.com` format. For more information, refer to [Access, Regions, & IP addresses](https://docs.getdbt.com/docs/platform/about-platform/access-regions-ip-addresses.md).
* **Production environment ID**: You can find this on the **Orchestration** page in the dbt platform. Use this to set an `x-dbt-prod-environment-id` header.
* **Token**: Generate either a personal access token or a service token. To fully utilize remote MCP, the token must have Semantic Layer and Developer permissions.
* If you plan to use `execute_sql`, you must use a [Personal Access Token (PAT)](https://docs.getdbt.com/docs/dbt-apis/user-tokens.md). Service tokens *do not* work for this tool. For other tools that require `x-dbt-user-id`, a PAT is also required.

3. For the remote MCP, you will pass on headers through the JSON blob to configure required fields:

#### Configuration for APIs and SQL tools[​](#configuration-for-apis-and-sql-tools "Direct link to Configuration for APIs and SQL tools")

| Header                    | Required | Description                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                       |
| ------------------------- | -------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Authorization             | Required | Your [personal access token (PAT)](https://docs.getdbt.com/docs/dbt-apis/user-tokens.md) or [service token](https://docs.getdbt.com/docs/dbt-apis/service-tokens.md) from the dbt platform.<br />**Note**: When using the Semantic Layer, we recommended to use a PAT. If you're using a service token, make sure that it has at least `Semantic Layer Only`, `Metadata Only`, and `Developer` permissions.<br /><br />The value must be in the format `Token YOUR_DBT_ACCESS_TOKEN` or `Bearer YOUR_DBT_ACCESS_TOKEN`, replacing `YOUR_DBT_ACCESS_TOKEN` with your actual token. |
| x-dbt-prod-environment-id | Required | Your dbt platform production environment ID                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                       |

Search table...

|                  |   |   |   |   |
| ---------------- | - | - | - | - |
| Loading table... |   |   |   |   |

#### Additional configuration for SQL tools[​](#additional-configuration-for-sql-tools "Direct link to Additional configuration for SQL tools")

| Header                   | Required                   | Description                                                                                   |
| ------------------------ | -------------------------- | --------------------------------------------------------------------------------------------- |
| x-dbt-dev-environment-id | Required for `execute_sql` | Your dbt platform development environment ID                                                  |
| x-dbt-user-id            | Required for `execute_sql` | Your dbt platform user ID ([see docs](https://docs.getdbt.com/faqs/Accounts/find-user-id.md)) |

Search table...

|                  |   |   |   |   |
| ---------------- | - | - | - | - |
| Loading table... |   |   |   |   |

#### Additional configuration for Fusion tools[​](#additional-configuration-for-fusion-tools "Direct link to Additional configuration for Fusion tools")

Fusion tools, by default, defer to the environment provided via `x-dbt-prod-environment-id` for model and table metadata.

| Header                     | Required | Description                                                                                                                                                                                                  |
| -------------------------- | -------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| x-dbt-dev-environment-id   | Required | Your dbt platform development environment ID                                                                                                                                                                 |
| x-dbt-user-id              | Required | Your dbt platform user ID ([see docs](https://docs.getdbt.com/faqs/Accounts/find-user-id.md))                                                                                                                |
| x-dbt-fusion-disable-defer | Optional | Default: `false`. When set to `true`, Fusion tools will not defer to the production environment and use the models and table metadata from the development environment (`x-dbt-dev-environment-id`) instead. |

Search table...

|                  |   |   |   |   |
| ---------------- | - | - | - | - |
| Loading table... |   |   |   |   |

#### Configuration to disable tools[​](#configuration-to-disable-tools "Direct link to Configuration to disable tools")

| Header                 | Required | Description                                                                                          |
| ---------------------- | -------- | ---------------------------------------------------------------------------------------------------- |
| x-dbt-disable-tools    | Optional | A comma-separated list of tools to disable. For instance: `get_all_models,text_to_sql,list_entities` |
| x-dbt-disable-toolsets | Optional | A comma-separated list of toolsets to disable. For instance: `semantic_layer,sql,discovery`          |

Search table...

|                  |   |   |   |   |
| ---------------- | - | - | - | - |
| Loading table... |   |   |   |   |

4. After establishing which headers you need, you can follow the [examples](https://github.com/dbt-labs/dbt-mcp/tree/main/examples) to create your own agent.

## Examples[​](#examples "Direct link to Examples")

The MCP protocol is programming language and framework agnostic, so use whatever helps you build agents. Alternatively, you can connect the remote dbt MCP server to MCP clients that support header-based authentication. Configuration varies by client — select your tool in the following tabs and replace the placeholder values with your own:

* Claude Code
* Cursor
* Gemini

```json
{
  "mcpServers": {
    "dbt": {
      "type": "http",
      "url": "https://YOUR_DBT_HOST_URL/api/ai/v1/mcp/",
      "headers": {
        "Authorization": "Token YOUR_DBT_ACCESS_TOKEN",
        "x-dbt-prod-environment-id": "DBT_PROD_ENV_ID",
        "x-dbt-user-id": "DBT_USER_ID",
        "x-dbt-dev-environment-id": "DBT_DEV_ENV_ID"
      }
    }
  }
}
```

```json
{
  "mcpServers": {
    "dbt": {
      "url": "https://YOUR_DBT_HOST_URL/api/ai/v1/mcp/",
      "headers": {
        "Authorization": "Token YOUR_DBT_ACCESS_TOKEN",
        "x-dbt-prod-environment-id": "DBT_PROD_ENV_ID",
        "x-dbt-user-id": "DBT_USER_ID",
        "x-dbt-dev-environment-id": "DBT_DEV_ENV_ID"
      }
    }
  }
}
```

```json
{
  "mcpServers": {
    "dbt": {
      "httpUrl": "https://YOUR_DBT_HOST_URL/api/ai/v1/mcp/",
      "headers": {
        "Authorization": "Token YOUR_DBT_ACCESS_TOKEN",
        "x-dbt-prod-environment-id": "DBT_PROD_ENV_ID",
        "x-dbt-user-id": "DBT_USER_ID",
        "x-dbt-dev-environment-id": "DBT_DEV_ENV_ID"
      }
    }
  }
}
```

Use numeric IDs, not full URLs

Header values like `x-dbt-prod-environment-id` and `x-dbt-user-id` expect numeric IDs, not full URLs. The host in the `url` field should include `https://`, but ID headers must be integers only:

```bash
# ✅ Correct
"url": "https://cloud.getdbt.com/api/ai/v1/mcp"
"x-dbt-prod-environment-id": "54321"
"x-dbt-user-id": "123"

# ❌ Wrong — don't paste full URLs into ID headers
"x-dbt-prod-environment-id": "https://cloud.getdbt.com/deploy/12345/projects/67890/environments/54321"
"x-dbt-user-id": "https://cloud.getdbt.com/settings/profile"
```

For other MCP clients (Codex, Windsurf, and so on), refer to your client's MCP configuration docs for the correct key format.

For self-hosted MCP, configuration is done via environment variables; see the [Environment variables reference](https://docs.getdbt.com/docs/dbt-ai/mcp-environment-variables.md).

## Related docs[​](#related-docs "Direct link to Related docs")

Step-by-step client setup (including Cursor, VS Code, and Claude) is in:

* [Integrate Cursor with MCP](https://docs.getdbt.com/docs/dbt-ai/integrate-mcp-cursor.md)
* [Integrate VS Code with MCP](https://docs.getdbt.com/docs/dbt-ai/integrate-mcp-vscode.md)
* [Integrate Claude with MCP](https://docs.getdbt.com/docs/dbt-ai/integrate-mcp-claude.md)
* [Integrate Snowflake Cortex with MCP](https://docs.getdbt.com/docs/dbt-ai/integrate-mcp-snowflake-cortex.md)
