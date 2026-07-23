# Connect dbt MCP server to dbt platform

This quickstart uses the self-hosted MCP server: it runs on your machine using `uvx dbt-mcp`, connects to your dbt platform for Semantic Layer, Discovery, and SQL, and optionally runs self-hosted dbt Core or Fusion CLI.

For self-hosted CLI only (with or without a dbt platform account), see [Run self-hosted dbt](https://docs.getdbt.com/docs/dbt-ai/mcp-quickstart-cli.md) or [Run self-hosted dbt Wizard](https://docs.getdbt.com/docs/dbt-ai/wizard-quickstart.md).

To configure or disable specific tools, see the [Environment variables reference](https://docs.getdbt.com/docs/dbt-ai/mcp-environment-variables.md).

## Prerequisites[​](#prerequisites "Direct link to Prerequisites")

* [Install uv](https://docs.astral.sh/uv/getting-started/installation/)

* A [dbt platform account](https://www.getdbt.com/signup)

* For OAuth connections:

  <!-- -->

  * MCP OAuth is available for Starter, Enterprise, and Enterprise+ plans.
  * An account admin has to enable AI features on your dbt platform account. Refer to [Enable AI features](https://docs.getdbt.com/docs/platform/enable-dbt-ai.md) for more info.

## Step 1: Choose your auth method and configure[​](#step-1-choose-your-auth-method-and-configure "Direct link to Step 1: Choose your auth method and configure")

* OAuth
* Tokens

*MCP OAuth is available in public beta for Starter, Enterprise, and Enterprise+ accounts.*

OAuth is the fastest setup for dbt platform accounts, no tokens to copy or manage. A browser window opens to authenticate the first time you connect.

For OAuth *without* a self-hosted installation, use the [remote MCP server](https://docs.getdbt.com/docs/dbt-ai/mcp-quickstart-remote.md). If your client does not support OAuth or you need token-based access, use [token-based authentication](https://docs.getdbt.com/docs/dbt-ai/setup-remote-mcp.md#token-based-authentication).

Static subdomains required

Only accounts with static subdomains (for example, `abc123` in `abc123.us1.dbt.com`) can use OAuth with MCP servers. Follow [these](https://docs.getdbt.com/docs/platform/about-platform/access-regions-ip-addresses.md) instructions to find your account subdomain. If your account does not have a subdomain, contact support for more information.

#### Find your Access URL[​](#find-your-access-url "Direct link to Find your Access URL")

1. Log in to your dbt platform account.
2. Go to **Account settings** and copy your **Access URL** (for example, `abc123.us1.dbt.com`).

Multi-cell and DBT\_HOST format

* The `DBT_HOST` field accepts both `abc123.us1.dbt.com` and `https://abc123.us1.dbt.com`.

* If your Access URL is `abc123.us1.dbt.com`, split it into two variables:

  * `DBT_HOST=us1.dbt.com`
  * `MULTICELL_ACCOUNT_PREFIX=abc123` Don't include the account prefix in `DBT_HOST`. For more details, see [multi-cell configuration examples](https://docs.getdbt.com/docs/dbt-ai/setup-local-mcp.md#api-and-sql-tool-settings).

#### Add the config to your MCP client[​](#add-the-config-to-your-mcp-client "Direct link to Add the config to your MCP client")

* Claude Desktop
* Claude Code
* Cursor
* VS Code

**Option A: Quick install (recommended)**

1. Go to the [latest dbt MCP release](https://github.com/dbt-labs/dbt-mcp/releases/latest) and download `dbt-mcp.mcpb`.
2. Double-click the file to open it in Claude Desktop.
3. Enter your **Access URL** as the dbt platform Host.
4. Enable the server.

**Option B: Manual config**

1. In Claude Desktop, go to **Settings** → **Developer** tab → **Edit Config**.
2. Paste the following configuration, replacing `YOUR-ACCESS-URL` with your Access URL:

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

3. Save and restart Claude Desktop.

Config file location:

* macOS: `~/Library/Application Support/Claude/claude_desktop_config.json`
* Windows: `%APPDATA%\Claude\claude_desktop_config.json`

Run this command, replacing `YOUR-ACCESS-URL` with your Access URL:

```shell
claude mcp add dbt \
-e DBT_HOST=YOUR-ACCESS-URL \
-- uvx dbt-mcp
```

For example, if your Access URL is `abc123.us1.dbt.com`:

```shell
claude mcp add dbt \
-e DBT_HOST=abc123.us1.dbt.com \
-- uvx dbt-mcp
```

Click a link below with Cursor open to auto-configure, then replace the placeholder with your Access URL:

* [dbt platform only (OAuth)](cursor://anysphere.cursor-deeplink/mcp/install?name=dbt\&config=eyJlbnYiOnsiREJUX0hPU1QiOiJZT1VSLUFDQ0VTUy1VUkwiLCJESVNBQkxFX0RCVF9DTEkiOiJ0cnVlIn0sImNvbW1hbmQiOiJ1dngiLCJhcmdzIjpbImRidC1tY3AiXX0%3D) — platform features only, no CLI
* [dbt platform + CLI (OAuth)](cursor://anysphere.cursor-deeplink/mcp/install?name=dbt\&config=eyJlbnYiOnsiREJUX0hPU1QiOiJZT1VSLUFDQ0VTUy1VUkwiLCJEQlRfUFJPSkVDVF9ESVIiOiIvcGF0aC90by9wcm9qZWN0IiwiREJUX1BBVEgiOiJwYXRoL3RvL2RidC9leGVjdXRhYmxlIn0sImNvbW1hbmQiOiJ1dngiLCJhcmdzIjpbImRidC1tY3AiXX0%3D) — platform features + self-hosted dbt CLI commands

After clicking, replace `YOUR-ACCESS-URL` with your actual Access URL (for example, `abc123.us1.dbt.com`) and save.

1. Open **Settings** → **Features** → **Chat** and ensure **MCP** is enabled.
2. Open the Command Palette (`Ctrl/Cmd + Shift + P`) and select **MCP: Open User Configuration**.
3. Add the following configuration to `mcp.json`:

VS Code uses `"servers"`, not `"mcpServers"`

```json
{
  "servers": {
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

Replace `YOUR-ACCESS-URL` with your Access URL (for example, `abc123.us1.dbt.com`) and save.

#### Optional: Add self-hosted dbt CLI commands[​](#optional-add-self-hosted-dbt-cli-commands "Direct link to Optional: Add self-hosted dbt CLI commands")

To also run dbt CLI commands (`dbt run`, `dbt build`, `dbt test`, and more), add these two variables to your `env` block:

```json
"DBT_PROJECT_DIR": "/path/to/your/dbt/project",
"DBT_PATH": "/path/to/your/dbt/executable"
```

Find `DBT_PATH` by running `which dbt` (macOS/Linux) or `where dbt` (Windows). `DBT_PROJECT_DIR` is the folder containing your `dbt_project.yml`.

Token-based auth gives you more control and is better for shared or team setups. You'll need a service token or Personal Access Token (PAT).

Which token should I use?

* **PAT (Personal Access Token):** Required if you want to use `execute_sql`. Tied to your user account.
* **Service token:** Works for all other platform toolsets. Better for shared or team setups.

See [Choosing an auth method](https://docs.getdbt.com/docs/dbt-ai/setup-local-mcp.md#choose-your-auth-method) for full guidance.

### Find your paths and IDs[​](#find-your-paths-and-ids "Direct link to Find your paths and IDs")

You need the following values. See [Finding your IDs](https://docs.getdbt.com/docs/dbt-ai/mcp-find-ids.md) for step-by-step instructions.

| Variable          | Where to find it                                                           |
| ----------------- | -------------------------------------------------------------------------- |
| `DBT_HOST`        | Your dbt platform hostname, found in **Account settings** → **Access URL** |
| `DBT_TOKEN`       | A service token or PAT from **Account settings** → **API tokens**          |
| `DBT_PROD_ENV_ID` | Your production environment ID, found in **Deploy** → **Environments**     |
| `DBT_DEV_ENV_ID`  | Your development environment ID (required for `execute_sql`)               |
| `DBT_USER_ID`     | Your numeric user ID (required for `execute_sql`)                          |
| `DBT_ACCOUNT_ID`  | Your account ID (required for Admin API tools)                             |

Search table...

|                  |   |   |   |   |
| ---------------- | - | - | - | - |
| Loading table... |   |   |   |   |

Use values only, not full URLs

These variables expect hostnames or numeric IDs — not full URLs:

```bash
# ✅ Correct
DBT_HOST=cloud.getdbt.com            # https://cloud.getdbt.com also works
DBT_PROD_ENV_ID=54321
DBT_USER_ID=123

# ❌ Wrong — IDs must be numeric, not full URLs
DBT_PROD_ENV_ID=https://cloud.getdbt.com/deploy/12345/projects/67890/environments/54321
DBT_USER_ID=https://cloud.getdbt.com/settings/profile
```

Multi-cell accounts

If your Access URL is `abc123.us1.dbt.com`, split it into two variables:

* `DBT_HOST=us1.dbt.com`
* `MULTICELL_ACCOUNT_PREFIX=abc123`

Don't include the account prefix in `DBT_HOST`. For more details, see [multi-cell configuration examples](https://docs.getdbt.com/docs/dbt-ai/setup-local-mcp.md#api-and-sql-tool-settings).

### Add the config to your MCP client[​](#add-the-config-to-your-mcp-client-1 "Direct link to Add the config to your MCP client")

Use the configuration below, replacing the placeholder values with your IDs from above. Include only the variables you need:

* Claude Desktop
* Claude Code
* Cursor
* VS Code

1. In Claude Desktop, go to **Settings** → **Developer** tab → **Edit Config**.
2. Paste the following configuration, replacing the placeholder values:

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
        "DBT_DEV_ENV_ID": "67890",
        "DBT_USER_ID": "123",
        "DBT_ACCOUNT_ID": "99999"
      }
    }
  }
}
```

3. Save and restart Claude Desktop.

Config file location:

* macOS: `~/Library/Application Support/Claude/claude_desktop_config.json`
* Windows: `%APPDATA%\Claude\claude_desktop_config.json`

Run this command, replacing the placeholders with your actual values:

```bash
claude mcp add dbt \
-e DBT_HOST=cloud.getdbt.com \
-e DBT_TOKEN=your-token-here \
-e DBT_PROD_ENV_ID=12345 \
-- uvx dbt-mcp
```

Add `-e DBT_DEV_ENV_ID=...` and `-e DBT_USER_ID=...` if you use `execute_sql`; add `-e DBT_ACCOUNT_ID=...` for Admin API.

1. In Cursor, open **Settings** → **MCP** → **Edit config** (or your config file).
2. Paste the following configuration, replacing the placeholder values:

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
        "DBT_DEV_ENV_ID": "67890",
        "DBT_USER_ID": "123",
        "DBT_ACCOUNT_ID": "99999"
      }
    }
  }
}
```

3. Save the configuration.

1) Open **Settings** → **Features** → **Chat** and ensure **MCP** is enabled.
2) Open the Command Palette (`Ctrl/Cmd + Shift + P`) and select **MCP: Open User Configuration**.
3) Add the following configuration to `mcp.json`:

VS Code uses `"servers"`, not `"mcpServers"`

```json
{
  "servers": {
    "dbt": {
      "command": "uvx",
      "args": ["dbt-mcp"],
      "env": {
        "DBT_HOST": "cloud.getdbt.com",
        "DBT_TOKEN": "your-token-here",
        "DBT_PROD_ENV_ID": "12345",
        "DBT_DEV_ENV_ID": "67890",
        "DBT_USER_ID": "123",
        "DBT_ACCOUNT_ID": "99999"
      }
    }
  }
}
```

4. Save `mcp.json` and restart VS Code.

 Optional: add self-hosted dbt CLI commands

To also run dbt commands (`dbt run`, `dbt build`, `dbt test`, and more), add these two variables to your `env` block:

```json
"DBT_PROJECT_DIR": "/path/to/your/dbt/project",
"DBT_PATH": "/path/to/your/dbt/executable"
```

Find `DBT_PATH` by running `which dbt` (macOS/Linux) or `where dbt` (Windows). `DBT_PROJECT_DIR` is the folder containing your `dbt_project.yml`.

## Step 2: Authenticate[​](#step-2-authenticate "Direct link to Step 2: Authenticate")

* OAuth
* Tokens

The first time you connect, dbt MCP opens a browser window to complete OAuth. After signing in, your session is saved and future connections are automatic.

If authentication doesn't start, close your client and run:

* macOS/Linux: `rm -f ~/.dbt/mcp.yml ~/.dbt/mcp.lock`
* Windows: `Remove-Item -Force $env:USERPROFILE\.dbt\mcp.yml, $env:USERPROFILE\.dbt\mcp.lock`

Then restart your client.

No additional authentication step is needed — your token is already in the configuration. The server connects automatically when your MCP client starts.

## Step 3: Test your setup[​](#step-3-test-your-setup "Direct link to Step 3: Test your setup")

Ask your AI assistant a data-related question (for example, *"What models are in my dbt project?"* or *"What metrics are defined in my Semantic Layer?"*). If dbt MCP is working, the response will use your dbt metadata.

## What's available[​](#whats-available "Direct link to What's available")

With the platform setup, your AI assistant can use:

* Semantic Layer queries
* Metadata Discovery (model lineage, test results, source freshness)
* Admin API (trigger jobs, list runs, get artifacts)
* SQL execution and text-to-SQL (requires a [PAT](https://docs.getdbt.com/docs/dbt-apis/user-tokens.md))
* All dbt commands if you added `DBT_PROJECT_DIR` and `DBT_PATH`

For the complete tool list, see [Available tools](https://docs.getdbt.com/docs/dbt-ai/mcp-available-tools.md).

Looking for self-hosted dbt CLI only?

If you only need to run dbt commands locally (with or without a dbt platform account), see [Run self-hosted dbt](https://docs.getdbt.com/docs/dbt-ai/mcp-quickstart-cli.md).

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

 OAuth login not initiating

 OAuth login not initiating

**Symptoms:** The OAuth browser window never opens, or authentication appears to hang.

**Cause:** dbt MCP uses a lock file to avoid repeated authentication. If a previous session left the lock file in place, it can block new authentication attempts.

**Solution:**

1. Close your MCP client (Claude Desktop, Cursor, VS Code, etc.).

2. Delete the self-hosted dbt MCP config files:

   <!-- -->

   * macOS/Linux: `rm -f ~/.dbt/mcp.yml ~/.dbt/mcp.lock`
   * Windows: `Remove-Item -Force $env:USERPROFILE\.dbt\mcp.yml, $env:USERPROFILE\.dbt\mcp.lock`

3. Restart your client and try connecting again.

If these steps don't resolve the issue, confirm that AI features are enabled on your account. An account admin can enable them in **Account settings** → **Edit** → toggle on **Enable account access to dbt Wizard features**. Refer to [Enable dbt Wizard](https://docs.getdbt.com/docs/platform/enable-dbt-ai.md).

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

 execute\_sql tool not working

 execute\_sql tool not working

**Symptoms:** The `execute_sql` tool returns an authentication error or is unavailable.

**Cause:** `execute_sql` requires a Personal Access Token (PAT). Service tokens do not work for this tool.

**Solution:**

1. Generate a [Personal Access Token (PAT)](https://docs.getdbt.com/docs/dbt-apis/user-tokens.md) in **Account settings** → **API tokens** → **Personal tokens**.
2. Use the PAT as your `DBT_TOKEN` value.
3. Also ensure `DBT_DEV_ENV_ID` and `DBT_USER_ID` are set — these are required for `execute_sql`.

 Toolset unavailable or showing as disabled

 Toolset unavailable or showing as disabled

**Symptoms:** A toolset (Semantic Layer, Discovery, Admin API) is not available in your AI client even though you've configured credentials.

**Cause:** Either the required variables are missing, or the toolset has been explicitly disabled.

**Solution:**

1. Check that all required variables for the toolset are set — see [Tool requirements](https://docs.getdbt.com/docs/dbt-ai/setup-local-mcp.md#tool-requirements-at-a-glance).
2. Check whether you have any `DISABLE_*` variables set to `true` that might be turning off the toolset.
3. If you're using enable mode (`DBT_MCP_ENABLE_*`), make sure the toolset you need is listed.
4. Set `DBT_MCP_LOG_LEVEL=DEBUG` to see which toolsets are active at startup.

 Pasting full URLs instead of IDs

 Pasting full URLs instead of IDs

**Symptoms:** Authentication errors, unexpected behavior, or the server failing to connect to the right environment.

**Cause:** Environment variables like `DBT_PROD_ENV_ID`, `DBT_USER_ID`, and `DBT_ACCOUNT_ID` expect numeric integers, not full browser URLs.

**Solution:**

```bash
# ✅ Correct
DBT_HOST=cloud.getdbt.com            # https://cloud.getdbt.com also works
DBT_PROD_ENV_ID=54321
DBT_USER_ID=123

# ❌ Wrong — IDs must be numeric, not full URLs
DBT_PROD_ENV_ID=https://cloud.getdbt.com/deploy/12345/projects/67890/environments/54321
DBT_USER_ID=https://cloud.getdbt.com/settings/profile
```

See [Finding your IDs](https://docs.getdbt.com/docs/dbt-ai/mcp-find-ids.md) for step-by-step instructions.

 Multi-cell account connection issues

 Multi-cell or static subdomain account connection issues

**Symptoms:** Connection errors when your account URL includes a prefix (for example, `abc123.us1.dbt.com`).

**Solution (as of v1.14.0):** Set `DBT_HOST` to the full hostname including the prefix. If you're using PAT-based auth, also set `DBT_ACCOUNT_ID`.

```bash
# ✅ Correct
DBT_HOST=abc123.us1.dbt.com
DBT_ACCOUNT_ID=12345  # required for PAT-based auth
```

You no longer need to set `MULTICELL_ACCOUNT_PREFIX` or `DBT_HOST_PREFIX`. If you have these set from an older configuration, remove them.

For all troubleshooting topics, see [MCP troubleshooting](https://docs.getdbt.com/docs/dbt-ai/mcp-troubleshooting.md).

## Next steps[​](#next-steps "Direct link to Next steps")

* Run dbt commands locally: see [Run self-hosted dbt](https://docs.getdbt.com/docs/dbt-ai/mcp-quickstart-cli.md)
* Configure specific toolsets: see the [Environment variables reference](https://docs.getdbt.com/docs/dbt-ai/mcp-environment-variables.md)
* Understand toolset requirements: see [Set up self-hosted MCP](https://docs.getdbt.com/docs/dbt-ai/setup-local-mcp.md#tool-requirements-at-a-glance)
