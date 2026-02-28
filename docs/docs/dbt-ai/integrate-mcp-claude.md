# Integrate Claude with dbt MCP

Claude is an AI assistant from Anthropic with two primary interfaces:

* [Claude for desktop](https://claude.ai/download): A GUI with MCP support for file access and commands as well as basic coding features
* [Claude Code](https://www.anthropic.com/claude-code): A terminal/IDE tool for development

## Claude Desktop[​](#claude-desktop "Direct link to Claude Desktop")

Static subdomains required

Only accounts with static subdomains (for example, `abc123` in `abc123.us1.dbt.com`) can use OAuth with MCP servers. Follow [these](https://docs.getdbt.com/docs/cloud/about-cloud/access-regions-ip-addresses.md) instructions to find your account subdomain. If your account does not have a subdomain, contact support for more information.

To configure Claude Desktop to use the dbt MCP server:

1. Go to the [latest dbt MCP release](https://github.com/dbt-labs/dbt-mcp/releases/latest) and download the `dbt-mcp.mcpb` file.
2. Double-click the downloaded file to open it in Claude Desktop.
3. Configure the **dbt Platform Host**. You can find this in your dbt platform account by navigating to **Account settings** and copying the **Access URL**.
4. Enable the server in Claude Desktop.
5. Ask Claude a data-related question and see dbt MCP in action!

### Advanced configuration with Claude Desktop[​](#advanced-configuration-with-claude-desktop "Direct link to Advanced configuration with Claude Desktop")

To add advanced configurations:

1. Go to the Claude settings and select **Settings…**.

2. In the Settings window, navigate to the **Developer** tab in the left sidebar. This section contains options for configuring MCP servers and other developer features.

3. Click the **Edit Config** button and open the configuration file with a text editor.

4. Add your server configuration based on your use case. Choose the [correct JSON structure](https://modelcontextprotocol.io/quickstart/user#installing-the-filesystem-server) from the following options:

    Local MCP with OAuth

   #### Local MCP with dbt platform authentication [Enterprise](https://www.getdbt.com/pricing "Go to https://www.getdbt.com/pricing")[Enterprise +](https://www.getdbt.com/pricing "Go to https://www.getdbt.com/pricing")[​](#local-mcp-with-dbt-platform-authentication- "Direct link to local-mcp-with-dbt-platform-authentication-")

   Configuration for users who want seamless OAuth authentication with the dbt platform

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
           "DBT_HOST": "https://<your-dbt-host-with-custom-subdomain>",
         }
       }
     }
   }
   ```

   **Note:** Replace `<your-dbt-host-with-custom-subdomain>` with your actual host (for example, `abc123.us1.dbt.com`). This enables OAuth authentication without requiring local dbt installation.

   This option is for users who want both dbt CLI commands and dbt platform features (Discovery API, Semantic Layer, job management).

   The `DBT_PROJECT_DIR` and `DBT_PATH` fields are required for CLI access. You can find the `DBT_HOST` field value in your dbt platform account information under **Access URLs**.

   ```json
   {
     "mcpServers": {
       "dbt": {
         "command": "uvx",
         "args": ["dbt-mcp"],
         "env": {
           "DBT_HOST": "https://<your-dbt-host-with-custom-subdomain>",
           "DBT_PROJECT_DIR": "/path/to/project",
           "DBT_PATH": "/path/to/dbt/executable"
         }
       }
     }
   }
   ```

   **Note:** Replace `<your-dbt-host-with-custom-subdomain>` with your actual host (for example, `https://abc123.us1.dbt.com`). This enables OAuth authentication.

    Local MCP (CLI only)

   Local configuration for users who only want to use dbt CLI commands with dbt Core or Fusion

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

    Local MCP with .env

   Advanced configuration for users who need custom environment variables

   Using the `env` field (recommended):

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
         "args": ["--env-file", "/path/to/.env", "dbt-mcp"]
       }
     }
   }
   ```

5. Save the file. Upon a successful restart of Claude Desktop, you'll see an MCP server indicator in the bottom-right corner of the conversation input box.

For debugging, you can find the Claude desktop logs at `~/Library/Logs/Claude` for Mac or `%APPDATA%\Claude\logs` for Windows.

## Claude Code[​](#claude-code "Direct link to Claude Code")

You can set up Claude Code with both the local and remote `dbt-mcp` server. We recommend using the local `dbt-mcp` for more developer-focused workloads. See the [About MCP](https://docs.getdbt.com/docs/dbt-ai/about-mcp.md#server-access) page for more more information about local and remote server features.

### Set up with local dbt MCP server[​](#set-up-with-local-dbt-mcp-server "Direct link to Set up with local dbt MCP server")

Prerequisites:

* Complete the [local MCP setup](https://docs.getdbt.com/docs/dbt-ai/setup-local-mcp.md).
* Know your configuration method (OAuth
  <!-- -->
  or environment variables)

In your Claude Code set up, run one of these commands based on your use case. Be sure to update the commands for your specific needs:

* CLI only
* OAuth with dbt platform

For dbt Core or Fusion only (no dbt platform account):

```shell
claude mcp add dbt \
-e DBT_PROJECT_DIR=/path/to/your/dbt/project \
-e DBT_PATH=/path/to/your/dbt/executable \
-- uvx dbt-mcp
```

For OAuth authentication (requires static subdomain). Find your static subdomain [here](https://docs.getdbt.com/docs/cloud/about-cloud/access-regions-ip-addresses.md):

```shell
claude mcp add dbt \
-e DBT_HOST=your-host-with-subdomain \
-e DBT_PROJECT_DIR=/path/to/your/dbt/project \
-e DBT_PATH=/path/to/your/dbt/executable \
-- uvx dbt-mcp
```

Replacing `your-host-with-subdomain`, `path/to/your/dbt/project`, and `path/to/your/dbt/executable` with your actual static subdomain, project path, and dbt executable path.

For example, if your static subdomain is `abc123.us1.dbt.com`, your command would look like this:

```shell
claude mcp add dbt \
-e DBT_HOST=abc123.us1.dbt.com \ ## this is the static subdomain
-e DBT_PROJECT_DIR=/path/to/your/dbt/project \
-e DBT_PATH=/path/to/your/dbt/executable \
-- uvx dbt-mcp
```

#### Using an `.env` file[​](#using-an-env-file "Direct link to using-an-env-file")

If you prefer to manage environment variables in a separate file, you can use the `--env-file` parameter from `uvx`:

```bash
claude mcp add dbt -- uvx --env-file <path-to-.env-file> dbt-mcp
```

Replace `<path-to-.env-file>` with the full path to your `.env` file.

## Troubleshooting[​](#troubleshooting "Direct link to Troubleshooting")

* Claude desktop may return errors such as `Error: spawn uvx ENOENT` or `Could not connect to MCP server dbt-mcp`. Try replacing the command and environment variables file path with the full path. For `uvx`, find the full path to `uvx` by running `which uvx` on Unix systems and placing this full path in the JSON. For instance: `"command": "/the/full/path/to/uvx"`.

## Was this page helpful?

YesNo

[Privacy policy](https://www.getdbt.com/cloud/privacy-policy)[Create a GitHub issue](https://github.com/dbt-labs/docs.getdbt.com/issues)

This site is protected by reCAPTCHA and the Google [Privacy Policy](https://policies.google.com/privacy) and [Terms of Service](https://policies.google.com/terms) apply.
