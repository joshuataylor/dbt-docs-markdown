# dbt MCP FAQs and troubleshooting

Find answers to common questions about the dbt Model Context Protocol (MCP) server and fixes for common setup issues. For client-specific steps, see the troubleshooting sections in the [Get started](./mcp-quickstart-oauth.md) and [Setup and config](./setup-local-mcp.md) pages.

## FAQs

The `execute_sql` tool in the dbt MCP server lets your AI assistant run SQL in your warehouse. These FAQs cover how it works and what its limits are.

 What does execute\_sql do?

`execute_sql` lets your AI assistant run a SQL query against your data warehouse and read the results. It's part of the SQL toolset and runs on dbt platform, so you don't need dbt installed locally.

Here's what happens when your assistant calls it:

1. The assistant writes a SQL query and calls `execute_sql`.
2. The dbt MCP server forwards the request to dbt platform, along with your token, development environment ID, and user ID.
3. dbt platform compiles the SQL (including any Jinja, such as `ref()`) and runs it in your warehouse using your development credentials.
4. The results come back to your assistant as a table.

![The AI assistant calls execute\_sql. The dbt MCP server forwards the request to dbt platform, which compiles the SQL, runs it in the warehouse with your development credentials, and returns the results to the assistant.](/img/docs/dbt-ai/mcp-execute-sql-flow.svg?v=2 "The AI assistant calls execute_sql. The dbt MCP server forwards the request to dbt platform, which compiles the SQL, runs it in the warehouse with your development credentials, and returns the results to the assistant.")The AI assistant calls execute\_sql. The dbt MCP server forwards the request to dbt platform, which compiles the SQL, runs it in the warehouse with your development credentials, and returns the results to the assistant.

 Is execute\_sql the same as dbt show?

No. `dbt show` runs through the dbt platform CLI tools and previews a model's output while you develop. `execute_sql` sends SQL to dbt platform, which runs it in your warehouse.

 Whose warehouse credentials does execute\_sql use?

Your personal development credentials for the project's development environment. These are the same credentials you use in Studio IDE. It doesn't use your production deployment credentials.

If you don't have development credentials, the query fails with a message that no warehouse credentials are configured. Ask a project admin to set them up.

 Is execute\_sql read-only?

No. `execute_sql` doesn't block write statements. What a query can do depends on the permissions of the warehouse credentials it runs with. If those credentials can create or drop tables, so can your assistant.

To keep things safe:

* **Use least-privilege warehouse roles.** Give each user's development credentials read-only access to production-like data and write access only to their own development schema.
* **Keep tool approval prompts on.** Your assistant then asks before running SQL, so you can review it first.
* **Don't count on disabling the tool as a security control.** You can turn off `execute_sql` with the `x-dbt-disable-tools` header, but whoever configures the MCP client sets it, so admins can't enforce it. Warehouse permissions are the control that holds.

 Who can use execute\_sql?

Anyone with a developer seat or a read-only seat, as long as they connect with OAuth or a [personal access token (PAT)](../dbt-apis/user-tokens.md). Service tokens don't work because development credentials belong to a person.

Read-only seats can run plain SQL. SQL that contains Jinja, like `ref()` or `source()`, needs a developer seat.

For setup details, see [Set up remote MCP](./setup-remote-mcp.md).

 Why doesn't ref() point to my development tables?

Even though the query runs with your development credentials, `ref('model')` resolves to the production relation. It can't point to a table you built in your own development schema.

To query a development table, use its full name instead, for example `my_dev_schema.my_model`. Avoid `source()` too, and use full table names or `ref()` instead.

 Are there limits on query size or run time?

Yes. Long-running queries time out, and dbt caps large result sets. Add filters and `limit` clauses to keep queries small and fast.

 Can I see results as a chart or table?

Yes you can! Your AI tool can ask for results to show as a chart or table in the UI. Otherwise, only your assistant sees the results.

 When should I use something other than execute\_sql?

| If you want to...                                    | Use                                                                                                                         |
| ---------------------------------------------------- | --------------------------------------------------------------------------------------------------------------------------- |
| Ask governed business questions with defined metrics | The [Semantic Layer tools](./mcp-available-tools.md#semantic-layer), like `query_metrics` |
| Preview a model's output while you develop           | dbt platform CLI tools, like `show`, `build`, and `run`                                                                     |
| Turn a question into SQL without running it          | `text_to_sql`                                                                                                               |

## Troubleshooting

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

**Symptoms:** The OAuth browser window never opens, or authentication appears to hang.

**Cause:** dbt MCP uses a lock file to avoid repeated authentication. If a previous session left the lock file in place, it can block new authentication attempts.

**Solution:**

1. Close your MCP client (Claude Desktop, Cursor, VS Code, etc.).

2. Delete the self-hosted dbt MCP config files:

   * macOS/Linux: `rm -f ~/.dbt/mcp.yml ~/.dbt/mcp.lock`
   * Windows: `Remove-Item -Force $env:USERPROFILE\.dbt\mcp.yml, $env:USERPROFILE\.dbt\mcp.lock`

3. Restart your client and try connecting again.

If these steps don't resolve the issue, confirm that AI features are enabled on your account. An account admin can enable them in **Account settings** → **Edit** → toggle on **Enable account access to dbt Wizard features**. Refer to [Enable dbt Wizard](../platform/manage-dbt-ai.md).

Server not starting

**Symptoms:** The MCP server shows as disconnected or unavailable in your client.

**Diagnosis:** Check the server logs:

* **VS Code:** Open the Command Palette (`Ctrl/Cmd + Shift + P`) → `MCP: List Servers` → click the dbt server to see detailed logs.
* **Claude Desktop:** Check `~/Library/Logs/Claude` (macOS) or `%APPDATA%\Claude\logs` (Windows).
* **All clients:** Set `DBT_MCP_LOG_LEVEL=DEBUG` in your environment variables to get more verbose output.

**Common causes:**

* Missing or incorrect (Applies to dbt v1.11 and later) `DBT_ENGINE_PROJECT_DIR` or `DBT_PATH` — verify the paths exist and are absolute paths.
* Invalid or expired authentication tokens — generate a new token and update your config.
* Missing required environment variables for the toolset you're trying to use — see [Tool requirements](./setup-local-mcp.md#tool-requirements-at-a-glance).

Configuration not working in WSL (VS Code)

**Symptoms:** VS Code MCP config works on other machines or native Windows but not in WSL.

**Cause:** Local user settings are not applied in WSL environments.

**Solution:**

Configure MCP in the WSL-specific settings instead of local user settings:

1. Open the Command Palette → **Preferences: Open Remote Settings**.
2. Or select the **Remote** tab in the Settings editor.
3. Add your MCP server configuration there.

execute\_sql tool not working

**Symptoms:** The `execute_sql` tool returns an authentication error or is unavailable.

**Cause:** How you fix this depends on how you connected:

* **Remote MCP with OAuth:** Sign in through your MCP client when prompted. You don't need a personal access token.
* **Remote MCP with token-based auth:** You need a [personal access token (PAT)](../dbt-apis/user-tokens.md). Service tokens won't work. You also need `x-dbt-dev-environment-id` and `x-dbt-user-id` headers. The user ID must match the user who created the PAT.
* **Local MCP:** Set `DBT_TOKEN` to a personal access token (PAT). Service tokens won't work.

**Solution:**

1. If you use OAuth for remote MCP, confirm you completed sign-in and consent in your MCP client.
2. If you use token-based auth or local MCP, create a [personal access token (PAT)](../dbt-apis/user-tokens.md) in **Account settings** → **API tokens** → **Personal tokens**.
3. For token-based remote MCP, add the PAT to the `Authorization` header as `Token YOUR_PAT`. For local MCP, set the PAT as your `DBT_TOKEN` value.
4. Also ensure `DBT_DEV_ENV_ID` and `DBT_USER_ID` are set (local MCP) or `x-dbt-dev-environment-id` and `x-dbt-user-id` headers are set (remote MCP). Refer to [Finding your IDs](./mcp-find-ids.md) for details.

Toolset unavailable or showing as disabled

**Symptoms:** A toolset (Semantic Layer, Discovery, Admin API) is not available in your AI client even though you've configured credentials.

**Cause:** Either the required variables are missing, or the toolset has been explicitly disabled.

**Solution:**

1. Check that all required variables for the toolset are set — see [Tool requirements](./setup-local-mcp.md#tool-requirements-at-a-glance).
2. Check whether you have any `DISABLE_*` variables set to `true` that might be turning off the toolset.
3. If you're using enable mode (`DBT_MCP_ENABLE_*`), make sure the toolset you need is listed.
4. Set `DBT_MCP_LOG_LEVEL=DEBUG` to see which toolsets are active at startup.

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

See [Finding your IDs](./mcp-find-ids.md) for step-by-step instructions.

Multi-cell or static subdomain account connection issues

**Symptoms:** Connection errors when your account URL includes a prefix (for example, `abc123.us1.dbt.com`).

**Solution (as of v1.14.0):** Set `DBT_HOST` to the full hostname including the prefix. If you're using PAT-based auth, also set `DBT_ACCOUNT_ID`.

```bash
# ✅ Correct
DBT_HOST=abc123.us1.dbt.com
DBT_ACCOUNT_ID=12345  # required for PAT-based auth
```

You no longer need to set `MULTICELL_ACCOUNT_PREFIX` or `DBT_HOST_PREFIX`. If you have these set from an older configuration, remove them.

Claude Desktop errors

Claude Desktop may return errors such as `Error: spawn uvx ENOENT`, `Error: spawn npx ENOENT`, `Could not connect to MCP server dbt-mcp`, or `Server disconnected`. For self-hosted installations, replace the `command` with the full path to `uvx`: run `which uvx` on Unix systems or `where uvx` on Windows and paste the full path into your JSON (for example, `"command": "/the/full/path/to/uvx"`). For token-based remote setups with `mcp-remote`, apply the same full-path fix for `npx`.

If Claude reports that entries in `claude_desktop_config.json` are not valid and were skipped, check that you are not using `"type": "http"` in that file. Use the [token-based `mcp-remote` config](./integrate-mcp-claude.md#desktop-remote) instead.

For remote setups, double-check that the MCP URL ends in `/api/ai/v1/mcp/` (including the trailing slash) and that your `Authorization` header value is `Token YOUR_DBT_ACCESS_TOKEN` or `Bearer YOUR_DBT_ACCESS_TOKEN`.

Logs are at `~/Library/Logs/Claude` (macOS) or `%APPDATA%\Claude\logs` (Windows).

Claude Code can't connect to the dbt MCP server

If the dbt MCP server doesn't connect, confirm `.mcp.json` is at the *project root* and that the `dbt` block matches one of the examples on the [Claude setup page](./integrate-mcp-claude.md#claude-code). For self-hosted installations, apply the same full-path fix for `uvx` (and for `--env-file` paths). For remote setups, verify the URL and headers, and try the equivalent `claude mcp add --transport http` command to compare.

The connector won't authorize or the OAuth flow fails

* Confirm your account has a [static subdomain](../platform/about-platform/access-regions-ip-addresses.md). OAuth with MCP requires one.
* Verify the host in `API_ALLOWED_PREFIXES`, `OAUTH_TOKEN_ENDPOINT`, `OAUTH_AUTHORIZATION_ENDPOINT`, and `OAUTH_RESOURCE_URL` all match your MCP URL host exactly, and that the integration `ENABLED = TRUE`.
* AI features are enabled by default and admins can [turn them off or back on anytime](../platform/manage-dbt-ai.md).
* Make sure remote MCP OAuth is available for your account plan.

The agent returns no metrics or empty results

* Confirm the project you authorized has a configured [Semantic Layer](../use-dbt-semantic-layer/dbt-sl.md) with metrics and dimensions.
* Check that the user who connected has at least read-only access to that project — the agent only sees what the user can see.
* If you scoped the OAuth connection to a single project, make sure it's the project that contains your metrics.

Permission errors when creating the integration or MCP server

`CREATE API INTEGRATION` and `CREATE EXTERNAL MCP SERVER` require `ACCOUNTADMIN` (or a role with account-level `CREATE INTEGRATION`). Run steps 1–2 as `ACCOUNTADMIN`, then grant `USAGE` on the MCP server and integration to the role that runs the agent (Step 3).
