# Set up remote MCP

The remote MCP server uses an HTTP connection and makes calls to dbt-mcp hosted on the cloud-based dbt platform. This setup requires no local installation and is ideal for data consumption use cases.

## When to use remote MCP[​](#when-to-use-remote-mcp "Direct link to When to use remote MCP")

The remote MCP server is the ideal choice when:

* You don't want to or are restricted from installing additional software (`uvx`, `dbt-mcp`) on your system.
* Your primary use case is *consumption-based*: querying metrics, exploring metadata, viewing lineage.
* You need access to Semantic Layer and Discovery APIs without maintaining a local dbt project.
* You don't need to execute CLI commands. Remote MCP does not support dbt CLI commands (`dbt run`, `dbt build`, `dbt test`, and more). If you need to execute dbt CLI commands, use the [local MCP server](https://docs.getdbt.com/docs/dbt-ai/setup-local-mcp.md) instead.

<!-- -->

info

Only [`text_to_sql`](#sql) consumes dbt Copilot credits. Other MCP tools do not.

When your account runs out of Copilot credits, the remote MCP server blocks all tools that run through it, even tools invoked from a local MCP server and [proxied](https://github.com/dbt-labs/dbt-mcp/blob/main/src/dbt_mcp/tools/toolsets.py#L24) to remote MCP (like SQL and remote Fusion tools).

If you reach your dbt Copilot usage limit, all tools will be blocked until your Copilot credits reset. If you need help, please reach out to your account manager.

## Setup instructions[​](#setup-instructions "Direct link to Setup instructions")

1. Ensure that you have [AI features](https://docs.getdbt.com/docs/cloud/enable-dbt-copilot) turned on.
2. Obtain the following information from dbt platform:

* **dbt Cloud host**: Use this to form the full URL. For example, replace `YOUR_DBT_HOST_URL` here: `https://YOUR_DBT_HOST_URL/api/ai/v1/mcp/`. It may look like: `https://cloud.getdbt.com/api/ai/v1/mcp/`. If you have a multi-cell account, the host URL will be in the `ACCOUNT_PREFIX.us1.dbt.com` format. For more information, refer to [Access, Regions, & IP addresses](https://docs.getdbt.com/docs/cloud/about-cloud/access-regions-ip-addresses.md).
* **Production environment ID**: You can find this on the **Orchestration** page in the dbt platform. Use this to set an `x-dbt-prod-environment-id` header.
* **Token**: Generate either a personal access token or a service token. In terms of permissions, to fully utilize remote MCP, it must be configured with Semantic Layer and Developer permissions. Note: to use functionality that requires the `x-dbt-user-id` header, a personal access token is required.

3. For the remote MCP, you will pass on headers through the JSON blob to configure required fields:

**Configuration for APIs and SQL tools**

| Header                    | Required | Description                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                   |
| ------------------------- | -------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Authorization             | Required | Your [personal access token (PAT)](https://docs.getdbt.com/docs/dbt-cloud-apis/user-tokens.md) or [service token](https://docs.getdbt.com/docs/dbt-cloud-apis/service-tokens.md) from the dbt platform.<br />**Note**: When using the Semantic Layer, we recommended to use a PAT. If you're using a service token, make sure that it has at least `Semantic Layer Only`, `Metadata Only`, and `Developer` permissions.<br /><br />The value must be in the format `Token YOUR_DBT_ACCESS_TOKEN` or `Bearer YOUR_DBT_ACCESS_TOKEN`, replacing `YOUR_DBT_ACCESS_TOKEN` with your actual token. |
| x-dbt-prod-environment-id | Required | Your dbt platform production environment ID                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                   |

Search table...

|                  |   |   |   |   |
| ---------------- | - | - | - | - |
| Loading table... |   |   |   |   |

**Additional configuration for SQL tools**

| Header                   | Required                   | Description                                                                                   |
| ------------------------ | -------------------------- | --------------------------------------------------------------------------------------------- |
| x-dbt-dev-environment-id | Required for `execute_sql` | Your dbt platform development environment ID                                                  |
| x-dbt-user-id            | Required for `execute_sql` | Your dbt platform user ID ([see docs](https://docs.getdbt.com/faqs/Accounts/find-user-id.md)) |

Search table...

|                  |   |   |   |   |
| ---------------- | - | - | - | - |
| Loading table... |   |   |   |   |

**Additional configuration for Fusion tools**

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

**Configuration to disable tools**

| Header                 | Required | Description                                                                                          |
| ---------------------- | -------- | ---------------------------------------------------------------------------------------------------- |
| x-dbt-disable-tools    | Optional | A comma-separated list of tools to disable. For instance: `get_all_models,text_to_sql,list_entities` |
| x-dbt-disable-toolsets | Optional | A comma-separated list of toolsets to disable. For instance: `semantic_layer,sql,discovery`          |

Search table...

|                  |   |   |   |   |
| ---------------- | - | - | - | - |
| Loading table... |   |   |   |   |

4. After establishing which headers you need, you can follow the [examples](https://github.com/dbt-labs/dbt-mcp/tree/main/examples) to create your own agent.

The MCP protocol is programming language and framework agnostic, so use whatever helps you build agents. Alternatively, you can connect the remote dbt MCP server to MCP clients that support header-based authentication. You can use this example Cursor configuration, replacing `YOUR_DBT_HOST_URL`, `YOUR_DBT_ACCESS_TOKEN`, `PROD-ID`, `USER-ID`, and `DEV-ID` with your information:

```text
{
  "mcpServers": {
    "dbt": {
      "url": "https://YOUR_DBT_HOST_URL/api/ai/v1/mcp/",
      "headers": {
       "Authorization": "Token YOUR_DBT_ACCESS_TOKEN",
        "x-dbt-prod-environment-id": "PROD-ID",
        "x-dbt-user-id": "USER-ID",
        "x-dbt-dev-environment-id": "DEV-ID"
      }
    }
  }
}
```

## Was this page helpful?

YesNo

[Privacy policy](https://www.getdbt.com/cloud/privacy-policy)[Create a GitHub issue](https://github.com/dbt-labs/docs.getdbt.com/issues)

This site is protected by reCAPTCHA and the Google [Privacy Policy](https://policies.google.com/privacy) and [Terms of Service](https://policies.google.com/terms) apply.
