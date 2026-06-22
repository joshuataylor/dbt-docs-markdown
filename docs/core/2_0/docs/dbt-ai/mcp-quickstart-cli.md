# Run dbt MCP server locally

This quickstart walks you through connecting dbt MCP server to your local dbt project. This setup gives you dbt command tools (`run`, `build`, `test`, `compile`, and more) inside your AI assistant.

If you'd like to connect to dbt platform with the CLI, see the [OAuth quickstart](https://docs.getdbt.com/docs/dbt-ai/mcp-quickstart-oauth.md).

To connect to dbt Wizard, see the [dbt Wizard quickstart](https://docs.getdbt.com/docs/dbt-ai/wizard-quickstart.md).

No clone required

You don't need to clone the dbt-mcp repository. Install [uv](https://docs.astral.sh/uv/getting-started/installation/) and run `uvx dbt-mcp` — it fetches and runs dbt-mcp for you.

## Prerequisites[​](#prerequisites "Direct link to Prerequisites")

* [Install uv](https://docs.astral.sh/uv/getting-started/installation/)
* A local dbt project (the folder containing your `dbt_project.yml` file)
* dbt installed locally (
  <!-- -->
  , dbt Fusion engine, or dbt CLI)

For the full list of environment variables and how to enable or disable toolsets, see the [Environment variables reference](https://docs.getdbt.com/docs/dbt-ai/mcp-environment-variables.md).

## Step 1: Find your paths[​](#step-1-find-your-paths "Direct link to Step 1: Find your paths")

You need two values before configuring your MCP client:

* `DBT_PROJECT_DIR` — the full path to your dbt project folder (where `dbt_project.yml` lives). For example, if your project name is `jaffle_shop`, the path should be `/Users/yourname/dbt-projects/jaffle_shop`.
* `DBT_PATH` — the full path to your dbt executable.

- macOS/Linux
- Windows

```bash
# Find DBT_PATH
which dbt
# Example output: /opt/homebrew/bin/dbt

# Find DBT_PROJECT_DIR — run from inside your project folder
pwd
# Example output: /Users/yourname/projects/my_dbt_project
```

```bash
# Find DBT_PATH
where dbt
# Example output: C:\Python39\Scripts\dbt.exe

# Find DBT_PROJECT_DIR — run from inside your project folder
cd
# Example output: C:\Users\yourname\projects\my_dbt_project
```

Note: Use forward slashes in your configuration: `C:/Python39/Scripts/dbt.exe`

## Step 2: Add to your MCP client[​](#step-2-add-to-your-mcp-client "Direct link to Step 2: Add to your MCP client")

Replace the paths below with the values from Step 1:

* Claude Desktop
* Claude Code
* Cursor
* VS Code

1. In Claude Desktop, go to **Settings** → **Developer** tab → **Edit Config**.
2. Paste the following configuration, replacing the paths with your actual values:

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

3. Save and restart Claude Desktop.

Config file location:

* macOS: `~/Library/Application Support/Claude/claude_desktop_config.json`
* Windows: `%APPDATA%\Claude\claude_desktop_config.json`

Run this command, replacing the paths with your actual values:

```shell
claude mcp add dbt \
-e DBT_PROJECT_DIR=/path/to/your/dbt/project \
-e DBT_PATH=/path/to/your/dbt/executable \
-- uvx dbt-mcp
```

Click the link below with Cursor open to auto-configure:

[Add <!-- -->or Fusion to Cursor](cursor://anysphere.cursor-deeplink/mcp/install?name=dbt\&config=eyJlbnYiOnsiREJUX1BST0pFQ1RfRElSIjoiL3BhdGgvdG8veW91ci9kYnQvcHJvamVjdCIsIkRCVF9QQVRIIjoiL3BhdGgvdG8veW91ci9kYnQvZXhlY3V0YWJsZSJ9LCJjb21tYW5kIjoidXZ4IiwiYXJncyI6WyJkYnQtbWNwIl19)

After clicking:

1. Update `DBT_PROJECT_DIR` with the full path to your dbt project.
2. Update `DBT_PATH` with the full path to your dbt executable (from Step 1).
3. Save the configuration.

1) Open **Settings** → **Features** → **Chat** and ensure **MCP** is enabled.
2) Open the Command Palette (`Ctrl/Cmd + Shift + P`) and select **MCP: Open User Configuration**.
3) Add the following configuration to `mcp.json`, replacing the paths with your actual values:

VS Code uses `"servers"`, not `"mcpServers"`

```json
{
  "servers": {
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

4. Save the file.

## Step 3: Test your setup[​](#step-3-test-your-setup "Direct link to Step 3: Test your setup")

Ask your AI assistant to run a dbt command (for example, *"Run `dbt compile` on my project"* or *"List all models in my project"*). If dbt MCP is working, the assistant will execute the command against your local project.

 Optional: verify from the command line

```bash
uvx dbt-mcp
```

If there are no errors, your configuration is correct. Press `Ctrl+C` to stop the server.

## What's available[​](#whats-available "Direct link to What's available")

With CLI-only setup, your AI assistant can use:

* `dbt run`, `dbt build`, `dbt test`, `dbt compile`, `dbt list`, `dbt parse`, `dbt show`
* Model lineage and node details from your local project
* Codegen tools (when enabled — see [Environment variables reference](https://docs.getdbt.com/docs/dbt-ai/mcp-environment-variables.md))

Platform features like Semantic Layer, Discovery API, and metadata queries require a dbt platform account. To add them, see [Connect to dbt platform](https://docs.getdbt.com/docs/dbt-ai/mcp-quickstart-oauth.md).

## Troubleshooting[​](#troubleshooting "Direct link to Troubleshooting")

 Can't find the uvx executable

 Can't find the uvx executable

**Symptoms:** Error messages like `Could not connect to MCP server dbt-mcp`, `Error: spawn uvx ENOENT`, or `spawn uvx ENOENT` in your MCP client.

**Cause:** Your MCP client (like Claude desktop) can't find `uvx` in its PATH because it starts with a limited environment.

**Solution:** Use the full path to `uvx` in your configuration.

1. Find the full path:

   * macOS/Linux: Run `which uvx` in Terminal.
   * Windows: Run `where uvx` in Command Prompt or PowerShell.

2. Replace `"command": "uvx"` with the full path:

```json
{
  "mcpServers": {
    "dbt": {
      "command": "/full/path/to/uvx",
      "args": ["dbt-mcp"],
      "env": { }
    }
  }
}
```

Example on macOS with Homebrew: `"command": "/opt/homebrew/bin/uvx"`

For VS Code (`mcp.json`), the same fix applies — replace `uvx` with its full path in the `command` field.

 Server not starting

 Server not starting

**Symptoms:** The MCP server shows as disconnected or unavailable in your client.

**Diagnosis:** Check the server logs:

* **VS Code:** Open the Command Palette (`Ctrl/Cmd + Shift + P`) → `MCP: List Servers` → click the dbt server to see detailed logs.
* **Claude Desktop:** Check `~/Library/Logs/Claude` (macOS) or `%APPDATA%\Claude\logs` (Windows).
* **All clients:** Set `DBT_MCP_LOG_LEVEL=DEBUG` in your environment variables to get more verbose output.

**Common causes:**

* Missing or incorrect `DBT_PROJECT_DIR` or `DBT_PATH` — verify the paths exist and are absolute paths.
* Invalid or expired authentication tokens — generate a new token and update your config.
* Missing required environment variables for the toolset you're trying to use — see [Tool requirements](https://docs.getdbt.com/docs/dbt-ai/setup-local-mcp.md#tool-requirements-at-a-glance).

For all troubleshooting topics, see [MCP troubleshooting](https://docs.getdbt.com/docs/dbt-ai/mcp-troubleshooting.md).

## Next steps[​](#next-steps "Direct link to Next steps")

* Add dbt platform features: see [Connect to dbt platform](https://docs.getdbt.com/docs/dbt-ai/mcp-quickstart-oauth.md)
* Configure toolsets or disable specific tools: see the [Environment variables reference](https://docs.getdbt.com/docs/dbt-ai/mcp-environment-variables.md)

## Was this page helpful?

YesNo

[Privacy policy](https://www.getdbt.com/cloud/privacy-policy)[Create a GitHub issue](https://github.com/dbt-labs/docs.getdbt.com/issues)

This site is protected by reCAPTCHA and the Google [Privacy Policy](https://policies.google.com/privacy) and [Terms of Service](https://policies.google.com/terms) apply.
