# Set up local MCP

[The local dbt MCP server](https://github.com/dbt-labs/dbt-mcp) runs locally on your machine and supports dbt Core, dbt Fusion engine, and dbt CLI. You can use it with or without a dbt platform account.

## Prerequisites[​](#prerequisites "Direct link to Prerequisites")

* [Install uv](https://docs.astral.sh/uv/getting-started/installation/) to be able to run `dbt-mcp` and [related dependencies](https://github.com/dbt-labs/dbt-mcp/blob/main/pyproject.toml) into an isolated virtual environment.
* Have a local dbt project (if you want to use dbt CLI commands).

## Setup options[​](#setup-options "Direct link to Setup options")

Choose the setup method that best fits your workflow:

### OAuth authentication with dbt platform [Enterprise](https://www.getdbt.com/pricing "Go to https://www.getdbt.com/pricing")[Enterprise +](https://www.getdbt.com/pricing "Go to https://www.getdbt.com/pricing")[​](#oauth-authentication-with-dbt-platform- "Direct link to oauth-authentication-with-dbt-platform-")

This method uses OAuth to authenticate with your dbt platform account. It's the simplest setup and doesn't require managing tokens or environment variables manually.

Static subdomains required

Only accounts with static subdomains (for example, `abc123` in `abc123.us1.dbt.com`) can use OAuth with MCP servers. Follow [these](https://docs.getdbt.com/docs/cloud/about-cloud/access-regions-ip-addresses.md) instructions to find your account subdomain. If your account does not have a subdomain, contact support for more information.

#### Configuration options[​](#configuration-options "Direct link to Configuration options")

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

Once configured, your session connects to the dbt platform account, starts the OAuth authentication workflow, and then opens your account where you can select the project you want to reference.

[![Select your dbt platform project](/img/mcp/select-project.png?v=2 "Select your dbt platform project")](#)Select your dbt platform project

After completing OAuth setup, skip to [Test your configuration](#optional-test-your-configuration).

### CLI only (no dbt platform)[​](#cli-only-no-dbt-platform "Direct link to CLI only (no dbt platform)")

This option runs the MCP server locally and connects it to your local dbt project using `DBT_PROJECT_DIR` and `DBT_PATH`.

If you're using the dbt Core or Fusion CLI and don't need access to dbt platform features (Discovery API, Semantic Layer, Administrative API), you can set up local MCP with just your dbt project information.

Add this configuration to your MCP client (refer to the specific [integration guides](#set-up-your-mcp-client) for exact file locations):

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

#### Locating your paths[​](#locating-your-paths "Direct link to Locating your paths")

Follow the appropriate instructions for your OS to locate your path:

 macOS/Linux

* **DBT\_PROJECT\_DIR**: The full path to your dbt project folder

  * Example: `/Users/yourname/dbt-projects/my_project`
  * This is the folder containing your `dbt_project.yml` file.

* **DBT\_PATH**: Find your dbt executable path by running in terminal:

  ```bash
  which dbt
  ```

  * Example output: `/opt/homebrew/bin/dbt`
  * Use this exact path in your configuration.

 Windows

* **DBT\_PROJECT\_DIR**: The full path to your dbt project folder

  * Example: `C:\Users\yourname\dbt-projects\my_project`
  * This is the folder containing your `dbt_project.yml` file.
  * Use forward slashes or escaped backslashes: `C:/Users/yourname/dbt-projects/my_project`

* **DBT\_PATH**: Find your dbt executable path by running in Command Prompt or PowerShell:

  ```bash
  where dbt
  ```

  * Example output: `C:\Python39\Scripts\dbt.exe`
  * Use forward slashes or escaped backslashes: `C:/Python39/Scripts/dbt.exe`

After completing this setup, skip to [Test your configuration](#optional-test-your-configuration).

### Environment variable configuration[​](#environment-variable-configuration "Direct link to Environment variable configuration")

If you need to configure multiple environment variables or prefer to manage them separately, you can use environment variables. If you are only using the dbt CLI commands, you do not need to supply the dbt platform-specific environment variables, and vice versa.

Here is an example of the file:

```code
DBT_HOST=cloud.getdbt.com
DBT_PROD_ENV_ID=your-production-environment-id
DBT_DEV_ENV_ID=your-development-environment-id
DBT_USER_ID=your-user-id
DBT_ACCOUNT_ID=your-account-id
DBT_TOKEN=your-service-token
DBT_PROJECT_DIR=/path/to/your/dbt/project
DBT_PATH=/path/to/your/dbt/executable
MULTICELL_ACCOUNT_PREFIX=your-account-prefix
```

You will need this file for integrating with MCP-compatible tools.

## API and SQL tool settings[​](#api-and-sql-tool-settings "Direct link to API and SQL tool settings")

| Environment Variable       | Required                               | Description                                                                                                                                                                                                                                                                                             |
| -------------------------- | -------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `DBT_HOST`                 | Required                               | Your dbt platform [instance hostname](https://docs.getdbt.com/docs/cloud/about-cloud/access-regions-ip-addresses.md). **Important:** For Multi-cell accounts, exclude the account prefix from the hostname. The default is `cloud.getdbt.com`.                                                          |
| MULTICELL\_ACCOUNT\_PREFIX | Only required for Multi-cell instances | Set your Multi-cell account prefix here (not in DBT\_HOST). If you are not using Multi-cell, don't set this value. You can learn more about regions and hosting [here](https://docs.getdbt.com/docs/cloud/about-cloud/access-regions-ip-addresses.md).                                                  |
| DBT\_TOKEN                 | Required                               | Your personal access token or service token from the dbt platform.<br />**Note**: When using the Semantic Layer, it is recommended to use a personal access token. If you're using a service token, make sure that it has at least `Semantic Layer Only`, `Metadata Only`, and `Developer` permissions. |
| DBT\_ACCOUNT\_ID           | Required for Administrative API tools  | Your [dbt account ID](https://docs.getdbt.com/faqs/Accounts/find-user-id.md)                                                                                                                                                                                                                            |
| DBT\_PROD\_ENV\_ID         | Required                               | Your dbt platform production environment ID                                                                                                                                                                                                                                                             |
| DBT\_DEV\_ENV\_ID          | Optional                               | Your dbt platform development environment ID                                                                                                                                                                                                                                                            |
| DBT\_USER\_ID              | Optional                               | Your dbt platform user ID ([docs](https://docs.getdbt.com/faqs/Accounts/find-user-id.md))                                                                                                                                                                                                               |

Search table...

|                  |   |   |   |   |
| ---------------- | - | - | - | - |
| Loading table... |   |   |   |   |

**Multi-cell configuration examples:**

✅ **Correct configuration:**

```bash
DBT_HOST=us1.dbt.com
MULTICELL_ACCOUNT_PREFIX=abc123
```

❌ **Incorrect configuration (common mistake):**

```bash
DBT_HOST=abc123.us1.dbt.com  # Don't include prefix in host!
# MULTICELL_ACCOUNT_PREFIX not set
```

If your full URL is `abc123.us1.dbt.com`, separate it as:

* `DBT_HOST=us1.dbt.com`
* `MULTICELL_ACCOUNT_PREFIX=abc123`

## dbt CLI settings[​](#dbt-cli-settings "Direct link to dbt CLI settings")

The local dbt-mcp supports all flavors of dbt, including dbt Core and dbt Fusion engine.

| Environment Variable | Required | Description                                                                                                                             | Example                                                                          |
| -------------------- | -------- | --------------------------------------------------------------------------------------------------------------------------------------- | -------------------------------------------------------------------------------- |
| `DBT_PROJECT_DIR`    | Required | The full path to where the repository of your dbt project is hosted locally. This is the folder containing your `dbt_project.yml` file. | macOS/Linux: `/Users/myname/reponame`<br />Windows: `C:/Users/myname/reponame`   |
| DBT\_PATH            | Required | The full path to your dbt executable (dbt Core/Fusion/dbt CLI). See the next section for how to find this.                              | macOS/Linux: `/opt/homebrew/bin/dbt`<br />Windows: `C:/Python39/Scripts/dbt.exe` |
| DBT\_CLI\_TIMEOUT    | Optional | Configure the number of seconds before your agent will timeout dbt CLI commands.                                                        | Defaults to 60 seconds.                                                          |

Search table...

|                  |   |   |   |   |
| ---------------- | - | - | - | - |
| Loading table... |   |   |   |   |

### Locating your `DBT_PATH`[​](#locating-your-dbt_path "Direct link to locating-your-dbt_path")

Follow the instructions for your OS to locate your `DBT_PATH`:

 macOS/Linux

Run this command in your Terminal:

```bash
which dbt
```

Example output: `/opt/homebrew/bin/dbt`

 Windows

Run this command in Command Prompt or PowerShell:

```bash
where dbt
```

Example output: `C:\Python39\Scripts\dbt.exe`

**Note:** Use forward slashes in your configuration: `C:/Python39/Scripts/dbt.exe`

**Additional notes:**

* You can set any environment variable supported by your dbt executable, like [the ones supported in dbt Core](https://docs.getdbt.com/reference/global-configs/about-global-configs.md#available-flags).
* dbt MCP respects the standard environment variables and flags for usage tracking mentioned [here](https://docs.getdbt.com/reference/global-configs/usage-stats.md).
* `DBT_WARN_ERROR_OPTIONS='{"error": ["NoNodesForSelectionCriteria"]}'` is automatically set so that the MCP server knows if no node is selected when running a dbt command. You can overwrite it if needed, but it provides a better experience when calling dbt from the MCP server, ensuring the tool selects valid nodes.

## Disabling tools[​](#disabling-tools "Direct link to Disabling tools")

You can disable the following tool access on the local `dbt-mcp`:

| Name                          | Default | Description                                                                                                                                           |
| ----------------------------- | ------- | ----------------------------------------------------------------------------------------------------------------------------------------------------- |
| `DISABLE_DBT_CLI`             | `false` | Set this to `true` to disable dbt Core, dbt CLI, and dbt Fusion MCP tools.                                                                            |
| `DISABLE_SEMANTIC_LAYER`      | `false` | Set this to `true` to disable dbt Semantic Layer MCP tools.                                                                                           |
| `DISABLE_DISCOVERY`           | `false` | Set this to `true` to disable dbt Discovery API MCP tools.                                                                                            |
| `DISABLE_ADMIN_API`           | `false` | Set this to `true` to disable dbt Administrative API MCP tools.                                                                                       |
| `DISABLE_SQL`                 | `true`  | Set this to `false` to enable SQL MCP tools.                                                                                                          |
| `DISABLE_DBT_CODEGEN`         | `true`  | Set this to `false` to enable [dbt codegen MCP tools](https://docs.getdbt.com/docs/dbt-ai/about-mcp.md#codegen-tools) (requires dbt-codegen package). |
| `DISABLE_LSP`                 | `false` | Set this to `true` to disable dbt LSP/Fusion MCP tools.                                                                                               |
| `DISABLE_MCP_SERVER_METADATA` | `true`  | Set this to `false` to enable MCP server metadata tools (like `get_mcp_server_version`).                                                              |
| `DISABLE_TOOLS`               | ""      | Set this to a list of tool names delimited by a `,` to disable specific tools.                                                                        |

Search table...

|                  |   |   |   |   |
| ---------------- | - | - | - | - |
| Loading table... |   |   |   |   |

#### Using environment variables in your MCP client configuration[​](#using-environment-variables-in-your-mcp-client-configuration "Direct link to Using environment variables in your MCP client configuration")

The recommended way to configure your MCP client is to use the `env` field in your JSON configuration file. This keeps all configuration in one file:

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

#### Using an `.env` file[​](#using-an-env-file "Direct link to using-an-env-file")

If you prefer to manage environment variables in a separate file, you can create an `.env` file and reference it:

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

However, this approach requires managing two files instead of one.

## (Optional) Test your configuration[​](#optional-test-your-configuration "Direct link to (Optional) Test your configuration")

In your command line tool, run the following to test your setup:

**If using the `env` field in JSON:**

```bash
export DBT_PROJECT_DIR=/path/to/project
export DBT_PATH=/path/to/dbt
uvx dbt-mcp
```

**If using an `.env` file:**

```bash
uvx --env-file <path-to-.env-file> dbt-mcp
```

If there are no errors, your configuration is correct.

## Set up your MCP client[​](#set-up-your-mcp-client "Direct link to Set up your MCP client")

After completing your configuration, follow the specific integration guide for your chosen tool:

* [Claude](https://docs.getdbt.com/docs/dbt-ai/integrate-mcp-claude.md)
* [Cursor](https://docs.getdbt.com/docs/dbt-ai/integrate-mcp-cursor.md)
* [VS Code](https://docs.getdbt.com/docs/dbt-ai/integrate-mcp-vscode.md)

## Debug configurations[​](#debug-configurations "Direct link to Debug configurations")

These settings allow you to customize the MCP server’s logging level to help with diagnosing and troubleshooting.

| Name                | Default | Description                                                                                                              |
| ------------------- | ------- | ------------------------------------------------------------------------------------------------------------------------ |
| `DBT_MCP_LOG_LEVEL` | `INFO`  | Environment variable to override the MCP server log level. Options are: `DEBUG`, `INFO`, `WARNING`, `ERROR`, `CRITICAL`. |

Search table...

|                  |   |   |   |   |
| ---------------- | - | - | - | - |
| Loading table... |   |   |   |   |

To see more detail about what’s happening inside the MCP server and help debug issues, you can temporarily set the log level to `DEBUG`. We recommend setting it temporarily to avoid filling up disk space with logs.

## Troubleshooting[​](#troubleshooting "Direct link to Troubleshooting")

#### Can't find `uvx` executable[​](#cant-find-uvx-executable "Direct link to cant-find-uvx-executable")

Some MCP clients may be unable to find `uvx` from the JSON config. This will result in error messages like `Could not connect to MCP server dbt-mcp`, `Error: spawn uvx ENOENT`, or similar.

**Solution:** Locate the full path to `uvx` and use it in your configuration:

* **macOS/Linux:** Run `which uvx` in your Terminal.
* **Windows:** Run `where uvx` in CMD or PowerShell.

Then update your JSON configuration to use the full path:

```json
{
  "mcpServers": {
    "dbt": {
      "command": "/full/path/to/uvx", # For example, on macOS with Homebrew: "command": "/opt/homebrew/bin/uvx"
      "args": ["dbt-mcp"],
      "env": { ... }
    }
  }
}
```

### Oauth login not initiating[​](#oauth-login-not-initiating "Direct link to Oauth login not initiating")

dbt MCP uses a lock file to avoid repeated authentication. In some cases, this can block authentication entirely. If this happens, close your client (for example, Cursor or Claude) and delete the local dbt MCP config files to reset the auth flow.

* On macOS and Linux, run: `rm -f ~/.dbt/mcp.yml ~/.dbt/mcp.lock`
* On [Windows](https://learn.microsoft.com/en-us/powershell/module/microsoft.powershell.management/remove-item?view=powershell-7.5), run: `Remove-Item -Force $env:USERPROFILE\.dbt\mcp.yml, $env:USERPROFILE\.dbt\mcp.lock`

## Was this page helpful?

YesNo

[Privacy policy](https://www.getdbt.com/cloud/privacy-policy)[Create a GitHub issue](https://github.com/dbt-labs/docs.getdbt.com/issues)

This site is protected by reCAPTCHA and the Google [Privacy Policy](https://policies.google.com/privacy) and [Terms of Service](https://policies.google.com/terms) apply.
