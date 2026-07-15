# Integrate Claude with dbt MCP

Claude is an AI assistant from Anthropic with two primary interfaces:

* [Claude Desktop](#claude-desktop): A GUI with MCP support for file access and commands, plus basic coding features.
* [Claude Code](#claude-code): A terminal/IDE tool for development.

Both interfaces can connect to either:

* Self-hosted dbt MCP server (runs on your machine, supports CLI commands like `dbt run`)
* Remote dbt MCP server (HTTP, no install, consumption-focused).

## Prerequisites[​](#prerequisites "Direct link to Prerequisites")

* You use Claude for AI or agentic work
* For OAuth (self-hosted or remote), use your [access URL with a static subdomain](https://docs.getdbt.com/docs/platform/about-platform/access-regions-ip-addresses.md).
  <!-- -->
  * Remote MCP OAuth is available for Starter, Enterprise, and Enterprise+ accounts.

Static subdomains required

Only accounts with static subdomains (for example, `abc123` in `abc123.us1.dbt.com`) can use OAuth with MCP servers. Follow [these](https://docs.getdbt.com/docs/platform/about-platform/access-regions-ip-addresses.md) instructions to find your account subdomain. If your account does not have a subdomain, contact support for more information.

## Claude Desktop[​](#claude-desktop "Direct link to Claude Desktop")

[Claude Desktop](https://claude.ai/download) reads MCP servers from `claude_desktop_config.json`. Open it from **Settings → Developer → Edit Config**.

### Set up with self-hosted dbt MCP server[​](#desktop-local "Direct link to Set up with self-hosted dbt MCP server")

For a fast first install, you can download the prebuilt `.mcpb` file; for more control, edit the JSON directly.

tip

You don't need to clone the dbt-mcp repository — for self-hosted setups, install [uv](https://docs.astral.sh/uv/getting-started/installation/) and run `uvx dbt-mcp` (or use the configs later in this page). Only clone the repository if you want to [contribute to dbt MCP](https://github.com/dbt-labs/dbt-mcp/issues).

#### Quick install with the .mcpb file[​](#quick-install-with-the-mcpb-file "Direct link to Quick install with the .mcpb file")

1. Go to the [latest dbt MCP release](https://github.com/dbt-labs/dbt-mcp/releases/latest) and download the `dbt-mcp.mcpb` file.
2. Double-click the downloaded file to open it in Claude Desktop.
3. Configure the **dbt platform Host**. You can find this in your dbt platform account by navigating to **Account settings** and copying the **Access URL**.
4. Enable the server in Claude Desktop.
5. Ask Claude a data-related question and see dbt MCP in action!

#### Advanced config with Claude Desktop[​](#advanced-config-with-claude-desktop "Direct link to Advanced config with Claude Desktop")

Use advanced configuration when you want to define the dbt MCP server yourself in Claude's configuration file — the same JSON where Claude stores every MCP server, under `mcpServers`, with fields like `command`, `args`, and `env`. See the [MCP install pattern](https://modelcontextprotocol.io/quickstart/user#installing-the-filesystem-server) for the underlying convention.

To open the configuration file and add or replace the dbt MCP server entry:

1. From Claude, go to **Settings…**

2. In the Settings window, select the **Developer** tab.

3. Click **Edit Config** and open the file in a text editor.

4. Add your server configuration under `mcpServers`. Choose the option that fits your use case:

    Self-hosted MCP with OAuth[Starter](https://www.getdbt.com/pricing "Go to https://www.getdbt.com/pricing")[Enterprise](https://www.getdbt.com/pricing "Go to https://www.getdbt.com/pricing")[Enterprise +](https://www.getdbt.com/pricing "Go to https://www.getdbt.com/pricing")

   Configuration for users who want seamless OAuth authentication with the dbt platform.

   Before you begin, make sure your account admin enables AI features on your dbt platform account to use OAuth. Refer to [Enable dbt Wizard](https://docs.getdbt.com/docs/platform/enable-dbt-ai.md) for more info.

   * dbt platform only
   * dbt platform + CLI

   This option is for users who only want dbt platform features (Discovery API, Semantic Layer, job management) without self-hosted dbt CLI commands.

   When you use only the dbt platform, the CLI tools are automatically disabled. You can find the `DBT_HOST` field value in your dbt platform account information under **Access URLs**.

   ```json
   {
     "mcpServers": {
       "dbt": {
         "command": "uvx",
         "args": ["dbt-mcp"],
         "env": {
           "DBT_HOST": "YOUR-ACCESS-URL"
         }
       }
     }
   }
   ```

   **Note:** Replace `YOUR-ACCESS-URL` with your Access URL hostname (for example, `abc123.us1.dbt.com`). Both `abc123.us1.dbt.com` and `https://abc123.us1.dbt.com` are accepted. This enables OAuth authentication without requiring self-hosted dbt installation.

   This option is for users who want both dbt CLI commands and dbt platform features (Discovery API, Semantic Layer, job management).

   The `DBT_PROJECT_DIR` and `DBT_PATH` fields are required for CLI access. You can find the `DBT_HOST` field value in your dbt platform account information under **Access URLs**.

   ```json
   {
     "mcpServers": {
       "dbt": {
         "command": "uvx",
         "args": ["dbt-mcp"],
         "env": {
           "DBT_HOST": "YOUR-ACCESS-URL",
           "DBT_PROJECT_DIR": "/path/to/project",
           "DBT_PATH": "/path/to/dbt/executable"
         }
       }
     }
   }
   ```

   **Note:** Replace `YOUR-ACCESS-URL` with your Access URL hostname (for example, `abc123.us1.dbt.com`). Both `abc123.us1.dbt.com` and `https://abc123.us1.dbt.com` are accepted. This enables OAuth authentication.

    Self-hosted MCP (CLI only)

   Self-hosted configuration for users who only want to use dbt commands with dbt Core or Fusion

   ```json
   {
     "mcpServers": {
       "dbt": {
         "command": "uvx",
         "args": ["dbt-mcp"],
         "env": {
           "DBT_PROJECT_DIR": "/path/to/your/dbt/project",
           "DBT_PATH": "/path/to/your/dbt/executable"
         }
       }
     }
   }
   ```

   Finding your paths:

   * **DBT\_PROJECT\_DIR**: Full path to the folder containing your `dbt_project.yml` file
   * **DBT\_PATH**: Find by running `which dbt` in Terminal (macOS/Linux) or `where dbt` (Windows) in Powershell

    Self-hosted MCP with .env

   Advanced configuration for users who need custom [environment variables](https://docs.getdbt.com/docs/dbt-ai/mcp-environment-variables.md). Put your `.env` file in your *dbt project root* (same folder as `dbt_project.yml`) and use an absolute path with `--env-file`.

   Using the `env` field (single-file configuration):

   IDs are integers, not URLs

   `DBT_PROD_ENV_ID`, `DBT_DEV_ENV_ID`, and `DBT_USER_ID` must be numeric IDs (for example, `54321`), not full URLs copied from your browser. `DBT_HOST` accepts both `cloud.getdbt.com` and `https://cloud.getdbt.com`.

   Using an `.env` file (use an absolute path to `.env` in your dbt project root):

   ```json
   {
     "mcpServers": {
       "dbt": {
         "command": "uvx",
         "args": ["dbt-mcp"],
         "env": {
           "DBT_HOST": "cloud.getdbt.com",
           "DBT_TOKEN": "your-token-here",
           "DBT_PROD_ENV_ID": "12345",
           "DBT_PROJECT_DIR": "/path/to/project",
           "DBT_PATH": "/path/to/dbt"
         }
       }
     }
   }
   ```

   Using an .env file (alternative):

   ```json
   {
     "mcpServers": {
       "dbt": {
         "command": "uvx",
         "args": [
           "--env-file",
           "/absolute/path/to/your-dbt-project/.env",
           "dbt-mcp"
         ]
       }
     }
   }
   ```

5. Save the file and restart Claude Desktop. You'll see an MCP server indicator in the bottom-right corner of the conversation input box.

For more configuration options (env vars, service tokens, tool-access controls), refer to [Set up self-hosted MCP](https://docs.getdbt.com/docs/dbt-ai/setup-local-mcp.md).

### Set up with remote dbt MCP server[​](#desktop-remote "Direct link to Set up with remote dbt MCP server")

The remote dbt MCP server runs in dbt platform — no `uvx` or other installations needed. Claude Desktop connects to it over HTTP.

info

Remote MCP OAuth is available in public beta for Starter, Enterprise, and Enterprise+ accounts.

Get your MCP URL first — you'll need it for both auth methods:

You can copy your full **MCP URL** from **Account settings** → **Access URLs** → **MCP Endpoint URL** in dbt platform, and paste it directly into your AI tool.

 Build your own MCP URL

We recommend using the MCP URL from **Account settings** → **Access URLs** → **MCP Endpoint URL** in dbt platform. However, if you want to build your own MCP URL, use your **Access URL** from **Account settings** in dbt platform. The remote MCP endpoint is `https://YOUR_DBT_HOST_URL/api/ai/v1/mcp`. Replace `YOUR_DBT_HOST_URL` with your hostname only (no `https://`).

For default hosts, multi-cell accounts, and regions, see [Access, Regions, & IP addresses](https://docs.getdbt.com/docs/platform/about-platform/access-regions-ip-addresses.md).

Then follow the tab that matches your auth method:

* OAuth (remote)
* Token-based

*Remote MCP OAuth is available in public beta for Starter, Enterprise, and Enterprise+ accounts.*

**Before you connect**

* Your MCP client must support OAuth for HTTP-based MCP servers. If it doesn't, use [token-based authentication](https://docs.getdbt.com/docs/dbt-ai/setup-remote-mcp.md#token-based-authentication) instead.
* On first connect, your client opens a browser for sign-in. dbt then shows a consent screen with the scopes (the specific permissions the client is allowed to use) it's requesting — see [Scopes and consent](https://docs.getdbt.com/docs/platform/manage-access/connect-apps-oauth.md#scopes-and-consent) for what each scope means.
* Most modern MCP clients self-register on first connect via [dynamic registration (RFC 7591)](https://docs.getdbt.com/docs/platform/manage-access/connect-apps-oauth.md#dynamic-registration). Clients that don't support it need an admin to register them in **Account settings → Integrations → App integrations**. See [Manual registration](https://docs.getdbt.com/docs/platform/manage-access/connect-apps-oauth.md#manual-registration).

For the full flow, sessions, and limitations, refer to [OAuth (remote MCP)](https://docs.getdbt.com/docs/dbt-ai/setup-remote-mcp.md#oauth-remote-mcp).

For OAuth, add dbt as a custom connector through Claude Desktop's settings — you don't need to edit `claude_desktop_config.json`.

The following steps show how to connect dbt as a custom connector in Claude Desktop. The exact UI varies by tool, but the flow is the same: add a custom connector with your MCP URL, complete the OAuth consent flow, then connect.

1. In your AI tool, go to its connector settings and choose to add a custom connector (in Claude Desktop, go to **Chat → Customize → Connectors**, then click **Add custom connector**).
2. Enter a name (for example, `dbt`) and paste your dbt platform MCP URL (for example, `https://abc123.us1.dbt.com/api/ai/v1/mcp`), then click **Add**.
   <!-- -->
   [![Custom connector dialog showing the dbt MCP URL](/img/docs/dbt-cloud/oauth-add-custom-connector.png?v=2 "Custom connector dialog showing the dbt MCP URL")](#)Custom connector dialog showing the dbt MCP URL
3. Click **Connect**. The tool redirects you to dbt to complete the OAuth consent flow, where you can approve or deny individual [scopes](https://docs.getdbt.com/docs/platform/manage-access/connect-apps-oauth.md#scopes-and-consent).
   <!-- -->
   [![OAuth consent screen showing requested scopes and project access](/img/docs/dbt-cloud/oauth-consent-screen.png?v=2 "OAuth consent screen showing requested scopes and project access")](#)OAuth consent screen showing requested scopes and project access
4. After you approve, the connector is added to the **Custom connectors** table and shows as connected.
   <!-- -->
   [![Adding a custom dbt connector in an AI tool's connector settings](/img/docs/dbt-cloud/oauth-connectors-page.png?v=2 "Adding a custom dbt connector in an AI tool's connector settings")](#)Adding a custom dbt connector in an AI tool's connector settings
5. That's it 🎉! Ask your tool a data question like *"What is the total revenue for the last 30 days?"* to confirm the connection.

Use token-based auth when your client doesn't yet support OAuth for HTTP MCP servers, or when you need a shared/CI setup.

1. From Claude Desktop, go to **Settings → Developer → Edit Config** to open `claude_desktop_config.json`.

2. Add a `dbt` entry under `mcpServers`:

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

   For token-based remote MCP, set these headers in your client's MCP config:

   * **`Authorization`** *(required)* — `Token YOUR_DBT_ACCESS_TOKEN` or `Bearer YOUR_DBT_ACCESS_TOKEN`. Use a [personal access token (PAT)](https://docs.getdbt.com/docs/dbt-apis/user-tokens.md) or a [service token](https://docs.getdbt.com/docs/dbt-apis/service-tokens.md) with at least Semantic Layer, Metadata, and Developer permissions.
   * **`x-dbt-prod-environment-id`** *(required)* — your dbt platform production environment ID. Find it on the **Orchestration** page.
   * **`x-dbt-dev-environment-id`** — required for `execute_sql` and Fusion tools.
   * **`x-dbt-user-id`** — required for `execute_sql`. Refer to [Find your user ID](https://docs.getdbt.com/faqs/Accounts/find-user-id.md).

   Use numeric IDs, not full URLs

   Headers like `x-dbt-prod-environment-id`, `x-dbt-dev-environment-id`, and `x-dbt-user-id` expect numeric IDs (for example, `54321`), not full URLs copied from your browser. The host in the `url` field, on the other hand, must include `https://`.

   `execute_sql` does **not** work with service tokens — you must use a PAT. For the complete list of headers (including tool-disable options) and the full table, refer to [Set up remote MCP](https://docs.getdbt.com/docs/dbt-ai/setup-remote-mcp.md#token-based-authentication).

3. Save the file and restart Claude Desktop. Ask Claude a data question to confirm the server is connected.

## Claude Code[​](#claude-code "Direct link to Claude Code")

[Claude Code](https://www.anthropic.com/claude-code) reads MCP servers from `.mcp.json` at the root of your project (the repository root for your workspace). If you already configured the dbt MCP server for another client, you can reuse the same JSON shape here — you don't need a second, separate registration.

### Set up with self-hosted dbt MCP server[​](#code-local "Direct link to Set up with self-hosted dbt MCP server")

tip

You don't need to clone the dbt-mcp repository — for self-hosted setups, install [uv](https://docs.astral.sh/uv/getting-started/installation/) and run `uvx dbt-mcp` (or use the configs later in this page). Only clone the repository if you want to [contribute to dbt MCP](https://github.com/dbt-labs/dbt-mcp/issues).

1. Follow [Set up self-hosted MCP](https://docs.getdbt.com/docs/dbt-ai/setup-local-mcp.md) to choose your auth pattern:

   * OAuth with the dbt platform
   * [CLI only](https://docs.getdbt.com/docs/dbt-ai/setup-local-mcp.md#cli-only)
   * [Environment variables](https://docs.getdbt.com/docs/dbt-ai/setup-local-mcp.md#environment-variable-configuration) (including an `.env` file with `--env-file`)

2. Create `.mcp.json` at the project root and add a `dbt` entry under the top-level `mcpServers` key. Pick the option that fits your use case:

    Self-hosted MCP with OAuth[Starter](https://www.getdbt.com/pricing "Go to https://www.getdbt.com/pricing")[Enterprise](https://www.getdbt.com/pricing "Go to https://www.getdbt.com/pricing")[Enterprise +](https://www.getdbt.com/pricing "Go to https://www.getdbt.com/pricing")

   Configuration for users who want seamless OAuth authentication with the dbt platform.

   Before you begin, make sure your account admin enables AI features on your dbt platform account to use OAuth. Refer to [Enable dbt Wizard](https://docs.getdbt.com/docs/platform/enable-dbt-ai.md) for more info.

   * dbt platform only
   * dbt platform + CLI

   This option is for users who only want dbt platform features (Discovery API, Semantic Layer, job management) without self-hosted dbt CLI commands.

   When you use only the dbt platform, the CLI tools are automatically disabled. You can find the `DBT_HOST` field value in your dbt platform account information under **Access URLs**.

   ```json
   {
     "mcpServers": {
       "dbt": {
         "command": "uvx",
         "args": ["dbt-mcp"],
         "env": {
           "DBT_HOST": "YOUR-ACCESS-URL"
         }
       }
     }
   }
   ```

   **Note:** Replace `YOUR-ACCESS-URL` with your Access URL hostname (for example, `abc123.us1.dbt.com`). Both `abc123.us1.dbt.com` and `https://abc123.us1.dbt.com` are accepted. This enables OAuth authentication without requiring self-hosted dbt installation.

   This option is for users who want both dbt CLI commands and dbt platform features (Discovery API, Semantic Layer, job management).

   The `DBT_PROJECT_DIR` and `DBT_PATH` fields are required for CLI access. You can find the `DBT_HOST` field value in your dbt platform account information under **Access URLs**.

   ```json
   {
     "mcpServers": {
       "dbt": {
         "command": "uvx",
         "args": ["dbt-mcp"],
         "env": {
           "DBT_HOST": "YOUR-ACCESS-URL",
           "DBT_PROJECT_DIR": "/path/to/project",
           "DBT_PATH": "/path/to/dbt/executable"
         }
       }
     }
   }
   ```

   **Note:** Replace `YOUR-ACCESS-URL` with your Access URL hostname (for example, `abc123.us1.dbt.com`). Both `abc123.us1.dbt.com` and `https://abc123.us1.dbt.com` are accepted. This enables OAuth authentication.

    Self-hosted MCP (CLI only)

   Self-hosted configuration for users who only want to use dbt commands with dbt Core or Fusion

   ```json
   {
     "mcpServers": {
       "dbt": {
         "command": "uvx",
         "args": ["dbt-mcp"],
         "env": {
           "DBT_PROJECT_DIR": "/path/to/your/dbt/project",
           "DBT_PATH": "/path/to/your/dbt/executable"
         }
       }
     }
   }
   ```

   Finding your paths:

   * **DBT\_PROJECT\_DIR**: Full path to the folder containing your `dbt_project.yml` file
   * **DBT\_PATH**: Find by running `which dbt` in Terminal (macOS/Linux) or `where dbt` (Windows) in Powershell

    Self-hosted MCP with .env

   Advanced configuration for users who need custom [environment variables](https://docs.getdbt.com/docs/dbt-ai/mcp-environment-variables.md). Put your `.env` file in your *dbt project root* (same folder as `dbt_project.yml`) and use an absolute path with `--env-file`.

   Using the `env` field (single-file configuration):

   IDs are integers, not URLs

   `DBT_PROD_ENV_ID`, `DBT_DEV_ENV_ID`, and `DBT_USER_ID` must be numeric IDs (for example, `54321`), not full URLs copied from your browser. `DBT_HOST` accepts both `cloud.getdbt.com` and `https://cloud.getdbt.com`.

   Using an `.env` file (use an absolute path to `.env` in your dbt project root):

   ```json
   {
     "mcpServers": {
       "dbt": {
         "command": "uvx",
         "args": ["dbt-mcp"],
         "env": {
           "DBT_HOST": "cloud.getdbt.com",
           "DBT_TOKEN": "your-token-here",
           "DBT_PROD_ENV_ID": "12345",
           "DBT_PROJECT_DIR": "/path/to/project",
           "DBT_PATH": "/path/to/dbt"
         }
       }
     }
   }
   ```

   Using an .env file (alternative):

   ```json
   {
     "mcpServers": {
       "dbt": {
         "command": "uvx",
         "args": [
           "--env-file",
           "/absolute/path/to/your-dbt-project/.env",
           "dbt-mcp"
         ]
       }
     }
   }
   ```

About `claude mcp add`

The Claude Code CLI can register MCP servers with `claude mcp add`, but it typically writes to the user-level config (`~/.claude.json`) rather than the project's `.mcp.json`. For dbt MCP, we recommend committing `.mcp.json` to your repository so the setup is project-scoped and easier to share.

### Set up with remote dbt MCP server[​](#code-remote "Direct link to Set up with remote dbt MCP server")

Claude Code can connect to the remote dbt MCP server over HTTP — same JSON shape as Claude Desktop, lives in `.mcp.json` instead of `claude_desktop_config.json`.

info

Remote MCP OAuth is available in public beta for Starter, Enterprise, and Enterprise+ accounts.

1. Open `.mcp.json` at the root of your project (create it if it doesn't exist).

2. Get your MCP URL:

   You can copy your full **MCP URL** from **Account settings** → **Access URLs** → **MCP Endpoint URL** in dbt platform, and paste it directly into your AI tool.

    Build your own MCP URL

   We recommend using the MCP URL from **Account settings** → **Access URLs** → **MCP Endpoint URL** in dbt platform. However, if you want to build your own MCP URL, use your **Access URL** from **Account settings** in dbt platform. The remote MCP endpoint is `https://YOUR_DBT_HOST_URL/api/ai/v1/mcp`. Replace `YOUR_DBT_HOST_URL` with your hostname only (no `https://`).

   For default hosts, multi-cell accounts, and regions, see [Access, Regions, & IP addresses](https://docs.getdbt.com/docs/platform/about-platform/access-regions-ip-addresses.md).

3. Add the `dbt` entry to `.mcp.json` using the tab that matches your auth method:

   * OAuth (remote)
   * Token-based

   *Remote MCP OAuth is available in public beta for Starter, Enterprise, and Enterprise+ accounts.*

   **Before you connect**

   * Your MCP client must support OAuth for HTTP-based MCP servers. If it doesn't, use [token-based authentication](https://docs.getdbt.com/docs/dbt-ai/setup-remote-mcp.md#token-based-authentication) instead.
   * On first connect, your client opens a browser for sign-in. dbt then shows a consent screen with the scopes (the specific permissions the client is allowed to use) it's requesting — see [Scopes and consent](https://docs.getdbt.com/docs/platform/manage-access/connect-apps-oauth.md#scopes-and-consent) for what each scope means.
   * Most modern MCP clients self-register on first connect via [dynamic registration (RFC 7591)](https://docs.getdbt.com/docs/platform/manage-access/connect-apps-oauth.md#dynamic-registration). Clients that don't support it need an admin to register them in **Account settings → Integrations → App integrations**. See [Manual registration](https://docs.getdbt.com/docs/platform/manage-access/connect-apps-oauth.md#manual-registration).

   For the full flow, sessions, and limitations, refer to [OAuth (remote MCP)](https://docs.getdbt.com/docs/dbt-ai/setup-remote-mcp.md#oauth-remote-mcp).

   Add the following to `.mcp.json` at your project root. On first connect, Claude Code opens a browser for sign-in and consent.

   ```json
   {
     "mcpServers": {
       "dbt": {
         "type": "http",
         "url": "https://YOUR_DBT_HOST_URL/api/ai/v1/mcp/"
       }
     }
   }
   ```

   Replace `YOUR_DBT_HOST_URL` with your hostname (for example, `abc123.us1.dbt.com`). You can find the URL in dbt platform under **Account settings** → **Access URLs** → **MCP Endpoint URL**.

   You can also register the same server from the CLI:

   ```bash
   claude mcp add --transport http dbt https://YOUR_DBT_HOST_URL/api/ai/v1/mcp/
   ```

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

   For token-based remote MCP, set these headers in your client's MCP config:

   * **`Authorization`** *(required)* — `Token YOUR_DBT_ACCESS_TOKEN` or `Bearer YOUR_DBT_ACCESS_TOKEN`. Use a [personal access token (PAT)](https://docs.getdbt.com/docs/dbt-apis/user-tokens.md) or a [service token](https://docs.getdbt.com/docs/dbt-apis/service-tokens.md) with at least Semantic Layer, Metadata, and Developer permissions.
   * **`x-dbt-prod-environment-id`** *(required)* — your dbt platform production environment ID. Find it on the **Orchestration** page.
   * **`x-dbt-dev-environment-id`** — required for `execute_sql` and Fusion tools.
   * **`x-dbt-user-id`** — required for `execute_sql`. Refer to [Find your user ID](https://docs.getdbt.com/faqs/Accounts/find-user-id.md).

   Use numeric IDs, not full URLs

   Headers like `x-dbt-prod-environment-id`, `x-dbt-dev-environment-id`, and `x-dbt-user-id` expect numeric IDs (for example, `54321`), not full URLs copied from your browser. The host in the `url` field, on the other hand, must include `https://`.

   `execute_sql` does **not** work with service tokens — you must use a PAT. For the complete list of headers (including tool-disable options) and the full table, refer to [Set up remote MCP](https://docs.getdbt.com/docs/dbt-ai/setup-remote-mcp.md#token-based-authentication).

4. Save the file. Claude Code picks up `.mcp.json` on startup — ask it a data question to confirm the connection.

## Troubleshooting[​](#troubleshooting "Direct link to Troubleshooting")

 Claude Desktop errors

Claude Desktop may return errors such as `Error: spawn uvx ENOENT` or `Could not connect to MCP server dbt-mcp`. For self-hosted installations, replace the `command` with the full path to `uvx`: run `which uvx` on Unix systems or `where uvx` on Windows and paste the full path into your JSON (for example, `"command": "/the/full/path/to/uvx"`). For remote setups, double-check that `url` ends in `/api/ai/v1/mcp/` and that your `Authorization` header is `Token YOUR_DBT_ACCESS_TOKEN` or `Bearer YOUR_DBT_ACCESS_TOKEN`.

Logs are at `~/Library/Logs/Claude` (macOS) or `%APPDATA%\Claude\logs` (Windows).

 Claude Code

If the dbt MCP server doesn't connect, confirm `.mcp.json` is at the *project root* and that the `dbt` block matches one of the examples on this page. For self-hosted installations, apply the same full-path fix for `uvx` (and for `--env-file` paths). For remote setups, verify the URL and headers, and try the equivalent `claude mcp add --transport http` command to compare.
