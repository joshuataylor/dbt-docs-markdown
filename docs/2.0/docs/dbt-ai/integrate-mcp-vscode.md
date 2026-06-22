# Integrate VS Code with MCP

[Microsoft Visual Studio Code (VS Code)](https://code.visualstudio.com/mcp) is a powerful and popular integrated development environment (IDE).

VS Code can connect to either the **local** dbt MCP server (runs on your machine, supports CLI commands like `dbt run`) or the **remote** dbt MCP server (HTTP, no install, consumption-focused). Before starting, make sure you have:

* VS Code installed with the latest updates.
* For local MCP: completed the [local MCP setup](https://docs.getdbt.com/docs/dbt-ai/setup-local-mcp.md) and configured your dbt project paths.
* For remote MCP: your **MCP URL** from **Account settings** → **Access URLs** → **MCP Endpoint URL** in dbt platform.

## Set up with local dbt MCP server[​](#set-up-with-local-dbt-mcp-server "Direct link to Set up with local dbt MCP server")

To get started, in VS Code:

1. Open the **Settings** menu and select the correct tab atop the page for your use case:

   * **Workspace**: Configures the server in the context of your workspace
   * **User**: Configures the server in the context of your user

   <br />

   **Note for WSL users**: If you're using VS Code with Windows Subsystem for Linux (WSL), you'll need to configure WSL-specific settings. Run the **Preferences: Open Remote Settings** command from the **Command Palette** (F1) or select the **Remote** tab in the **Settings** editor. Local user settings are reused in WSL but can be overridden with WSL-specific settings. Configuring MCP servers in the local user settings will not work properly in a WSL environment.

2. Select **Features** --> **Chat**

3. Ensure that **MCP** is **Enabled**

[![mcp-vscode-settings](/img/mcp/vscode_mcp_enabled_image.png?v=2 "mcp-vscode-settings")](#)mcp-vscode-settings

4. Open the command palette `Control/Command + Shift + P`, and select either:

   * **MCP: Open Workspace Folder MCP Configuration** — if you want to install the MCP server for this workspace
   * **MCP: Open User Configuration** — if you want to install the MCP server for the user

5. Add your server configuration (`dbt`) to the provided `mcp.json` file as one of the servers:

   tip

   You do not need to clone the dbt-mcp repository. Install [uv](https://docs.astral.sh/uv/getting-started/installation/) and run `uvx dbt-mcp` (or use the config below); cloning is only for contributing.

    Local MCP with dbt platform OAuth

   Local MCP with OAuth is for users who want to use the dbt platform features.

   Before you begin, make sure your account admin has enabled AI features on your dbt platform account. Refer to [Enable dbt AI](https://docs.getdbt.com/docs/platform/enable-dbt-ai.md) for more info.

   Choose your configuration based on your use case:

   * dbt platform only
   * dbt platform + CLI

   This option is for users who only want dbt platform features (Discovery API, Semantic Layer, job management) without local CLI commands.

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

   **Note:** Replace `YOUR-ACCESS-URL` with your Access URL hostname (for example, `abc123.us1.dbt.com`). Both `abc123.us1.dbt.com` and `https://abc123.us1.dbt.com` are accepted. This enables OAuth authentication without requiring local dbt installation.

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

    Local MCP (CLI only)

   For users who only want to use dbt commands with dbt Core or Fusion

   ```json
   {
     "servers": {
       "dbt": {
         "command": "uvx",
         "args": ["dbt-mcp"],
         "env": {
           "DBT_ENGINE_PROJECT_DIR": "/path/to/your/dbt/project",
           "DBT_PATH": "/path/to/your/dbt/executable"
         }
       }
     }
   }
   ```

   **Finding your paths:**

   * **DBT\_ENGINE\_PROJECT\_DIR**: Full path to the folder containing your `dbt_project.yml` file

     <!-- -->

     * macOS/Linux: Run `pwd` from your project folder.
     * Windows: Run `cd` from your project folder in Command Prompt.

   * **DBT\_PATH**: Path to dbt executable

     <!-- -->

     * macOS/Linux: Run `which dbt`.
     * Windows: Run `where dbt`.

    Local MCP with .env

   For advanced users who need custom environment variables or service token authentication. Put your `.env` file in your *dbt project root* (same folder as `dbt_project.yml`) and use an absolute path with `--env-file`. Refer to the [Environment variables reference](https://docs.getdbt.com/docs/dbt-ai/mcp-environment-variables.md) for the complete list of available environment variables for the local MCP server.

   Using the `env` field (single-file configuration):

   IDs are integers, not URLs

   `DBT_PROD_ENV_ID`, `DBT_DEV_ENV_ID`, and `DBT_USER_ID` must be numeric IDs (for example, `54321`), not full URLs copied from your browser. `DBT_HOST` accepts both `cloud.getdbt.com` and `https://cloud.getdbt.com`.

   ```json
   {
     "servers": {
       "dbt": {
         "command": "uvx",
         "args": ["dbt-mcp"],
         "env": {
           "DBT_ENGINE_HOST": "cloud.getdbt.com",
           "DBT_TOKEN": "your-token-here",
           "DBT_PROD_ENV_ID": "12345",
           "DBT_ENGINE_PROJECT_DIR": "/path/to/project",
           "DBT_PATH": "/path/to/dbt"
         }
       }
     }
   }
   ```

   Using an `.env` file (alternative - two-file configuration):

   ```json
   {
     "servers": {
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

6. You can start, stop, and configure your MCP servers by:

   * Running the `MCP: List Servers` command from the Command Palette (Control/Command + Shift + P) and selecting the server.
   * Utilizing the keywords inline within the `mcp.json` file.

[![VS Code inline management](/img/mcp/vscode_run_server_keywords_inline.png?v=2 "VS Code inline management")](#)VS Code inline management

Now, you can access the dbt MCP server in VS Code through interfaces like GitHub Copilot.

## Set up with remote dbt MCP server[​](#set-up-with-remote-dbt-mcp-server "Direct link to Set up with remote dbt MCP server")

The remote dbt MCP server runs in dbt platform — no `uvx` or local install needed. VS Code connects to it over HTTP from the same `mcp.json` you use for local servers.

info

Remote MCP OAuth is available for Starter, Enterprise, and Enterprise+ accounts.

1. Open the command palette (`Control/Command + Shift + P`) and select one of:

   * **MCP: Open Workspace Folder MCP Configuration** — for this workspace.
   * **MCP: Open User Configuration** — for your user.

2. Get your MCP URL:

   You can copy your full **MCP URL** from **Account settings** → **Access URLs** → **MCP Endpoint URL** in dbt platform, and paste it directly into your AI tool.

    Build your own MCP URL

   We recommend using the MCP URL from **Account settings** → **Access URLs** → **MCP Endpoint URL** in dbt platform. However, if you want to build your own MCP URL, use your **Access URL** from **Account settings** in dbt platform. The remote MCP endpoint is `https://YOUR_DBT_HOST_URL/api/ai/v1/mcp`. Replace `YOUR_DBT_HOST_URL` with your hostname only (no `https://`).

   For default hosts, multi-cell accounts, and regions, see [Access, Regions, & IP addresses](https://docs.getdbt.com/docs/platform/about-platform/access-regions-ip-addresses.md).

3. Add a `dbt` entry under the top-level `servers` key. (VS Code uses `servers`, not `mcpServers`.) Pick the tab that matches your auth method:

   * OAuth (remote)
   * Token-based

   *OAuth is in private beta for Enterprise and Enterprise+ accounts.*

   **Before you connect**

   * Your MCP client must support OAuth for HTTP-based MCP servers. If it doesn't, use [token-based authentication](https://docs.getdbt.com/docs/dbt-ai/setup-remote-mcp.md#token-based-authentication) instead.
   * On first connect, your client opens a browser for sign-in. dbt then shows a consent screen with the scopes (the specific permissions the client is allowed to use) it's requesting — see [Scopes and consent](https://docs.getdbt.com/docs/platform/manage-access/connect-apps-oauth.md#scopes-and-consent) for what each scope means.
   * Most modern MCP clients self-register on first connect via [dynamic registration (RFC 7591)](https://docs.getdbt.com/docs/platform/manage-access/connect-apps-oauth.md#dynamic-registration). Clients that don't support it need an admin to register them in **Account settings → Integrations → App integrations**. See [Manual registration](https://docs.getdbt.com/docs/platform/manage-access/connect-apps-oauth.md#manual-registration).

   For the full flow, sessions, and limitations, refer to [OAuth (remote MCP)](https://docs.getdbt.com/docs/dbt-ai/setup-remote-mcp.md#oauth-remote-mcp).

   Add the following to `mcp.json`. VS Code opens a browser for sign-in and consent the first time the server connects.

   ```json
   {
     "servers": {
       "dbt": {
         "type": "http",
         "url": "https://YOUR_DBT_HOST_URL/api/ai/v1/mcp/"
       }
     }
   }
   ```

   Replace `YOUR_DBT_HOST_URL` with your hostname (for example, `abc123.us1.dbt.com`). You can find the URL in dbt platform under **Account settings** → **Access URLs** → **MCP Endpoint URL**.

   Use token-based auth when your client doesn't yet support OAuth for HTTP MCP servers, or when you need a shared/CI setup.

   ```json
   {
     "servers": {
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

4. Save the file. Use **MCP: List Servers** from the command palette to start the server, then ask Copilot Chat a data-related question to confirm the connection.

## Troubleshooting[​](#troubleshooting "Direct link to Troubleshooting")

This section contains troubleshooting steps for errors you might encounter when integrating VS Code with MCP.

 Cannot find \`uvx\` executable

If you see errors like `Could not connect to MCP server dbt` or `spawn uvx ENOENT`, VS Code may be unable to find the `uvx` executable.

To resolve, use the full path to `uvx` in your configuration:

1. Find the full path:

   * macOS/Linux: Run `which uvx` in Terminal.
   * Windows: Run `where uvx` in Command Prompt or PowerShell.

2. Update your `mcp.json` to use the full path:

   ```json
   {
     "servers": {
       "dbt": {
         "command": "/full/path/to/uvx",
         "args": ["dbt-mcp"],
         "env": { ... }
       }
     }
   }
   ```

   Example on macOS with Homebrew: `"command": "/opt/homebrew/bin/uvx"`

 Configuration not working in WSL

If you're using VS Code with Windows Subsystem for Linux (WSL), make sure you've configured the MCP server in the WSL-specific settings, not the local user settings. Use the **Remote** tab in the Settings editor or run **Preferences: Open Remote Settings** from the Command Palette.

 Server not starting

Check the MCP server status:

1. Run `MCP: List Servers` from the Command Palette (Control/Command + Shift + P).
2. Look for any error messages next to the dbt server.
3. Click on the server to see detailed logs.

Common issues:

* Missing or incorrect paths for `DBT_ENGINE_PROJECT_DIR` or `DBT_PATH`
* Invalid authentication tokens
* Missing required environment variables

## Resources[​](#resources "Direct link to Resources")

* [Microsoft VS Code MCP documentation](https://code.visualstudio.com/docs/copilot/chat/mcp-servers)

## Was this page helpful?

YesNo

[Privacy policy](https://www.getdbt.com/cloud/privacy-policy)[Create a GitHub issue](https://github.com/dbt-labs/docs.getdbt.com/issues)

This site is protected by reCAPTCHA and the Google [Privacy Policy](https://policies.google.com/privacy) and [Terms of Service](https://policies.google.com/terms) apply.
