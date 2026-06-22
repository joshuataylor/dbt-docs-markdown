# Use MCP servers with the dbt Wizard CLI [Beta](https://docs.getdbt.com/docs/dbt-versions/product-lifecycles "Go to https://docs.getdbt.com/docs/dbt-versions/product-lifecycles")

The Model Context Protocol (MCP) connects dbt Wizard to external tools and context. Add an MCP server and dbt Wizard can call its tools mid-session — query the dbt MCP server for governed project metadata, open a pull request through the GitHub MCP server, or pull in any other MCP-compatible service.

See it in action and share your feedback

Want to see dbt Wizard in action? Check out the [demo video](https://www.youtube.com/watch?v=-lIzh1xQWMA).

We'd love to hear how dbt Wizard is working for you. Share your feedback by either running the `/feedback` slash command in your interactive terminal session or by going to the [#dbt-wizard](https://getdbt.slack.com/archives/C0B6KLW6T26) channel in the [dbt Community Slack](https://docs.getdbt.com/community/join?version=2.0).

Thanks so much for your help in improving dbt Wizard and dbt data development!

For background on MCP itself, refer to the [Model Context Protocol introduction](https://modelcontextprotocol.io/introduction). For the dbt-maintained server specifically, refer to the [dbt MCP server](https://docs.getdbt.com/docs/dbt-ai/about-mcp.md).

## Locations and precedence[​](#locations-and-precedence "Direct link to Locations and precedence")

MCP servers are configured under `[mcp_servers.NAME]` in `config.toml`. Use user-level config for servers you want in every local project, and project-level config for servers that should travel with a trusted repo.

The following table summarizes where dbt Wizard looks for MCP server configuration. Use the intended locations for new MCP servers, and keep compatibility locations only when migrating existing setup. Project-level MCP config loads only for trusted projects. Within project-level locations, the current working directory wins over parent directories. Project-level servers win over user-level servers with the same name.

| Location                    | Level   | Use for new MCP servers?                                                                                    | Precedence                                                                                                 |
| --------------------------- | ------- | ----------------------------------------------------------------------------------------------------------- | ---------------------------------------------------------------------------------------------------------- |
| `.dbt/wizard/config.toml`   | Project | Yes. Use `[mcp_servers.NAME]` here for repo-shared MCP servers.                                             | Highest project MCP config location; closer to the current working directory wins over parent directories. |
| `~/.dbt/wizard/config.toml` | User    | Yes. Use this for MCP servers you want across all local projects. The `wizard mcp add` command writes here. | Below project config, above compatibility imports.                                                         |
| `.mcp.json`                 | Project | No. Compatibility import for existing MCP setup.                                                            | Imported with the project config for the same directory.                                                   |
| `~/.dbt/wizard/.mcp.json`   | User    | No. Compatibility import for existing MCP setup.                                                            | Imported with user config; takes precedence over `~/.mcp.json` if both exist.                              |
| `~/.mcp.json`               | User    | No. Compatibility import for existing MCP setup.                                                            | User-level fallback.                                                                                       |

Avoid defining the same MCP server name in more than one location unless you intentionally want the higher-precedence location to override it. If a compatibility `.mcp.json` file and `config.toml` in the same directory define the same server name, remove the duplicate and keep the intended `config.toml` entry.

## Why use an MCP server[​](#why-use-an-mcp-server "Direct link to Why use an MCP server")

dbt Wizard natively understands your dbt project. An MCP server extends that reach to the other tools and systems your work depends on, so you can do more without leaving your [session](https://docs.getdbt.com/docs/dbt-ai/wizard-how-it-works.md#sessions). Each server you add gives dbt Wizard a new set of tools it can call on your behalf. For example:

* dbt MCP server for governed access to your models, metrics, and lineage.
* GitHub server to read and review pull requests.
* Data warehouse server, or another server, to pull in context that lives outside dbt.

The dbt Wizard CLI lets you add, remove, authenticate, and customize MCP servers, including per-tool approvals, through the `config.toml` file.

MCP servers are a CLI feature

You can configure MCP servers only in the dbt Wizard CLI. You can't add your own MCP servers in the dbt platform (Studio IDE and the home app), but dbt Wizard includes built-in dbt tools, such as [dbt Agent skills](https://github.com/dbt-labs/dbt-agent-skills) and product documentation fetching through the dbt MCP server.

## Supported MCP server types[​](#supported-mcp-server-types "Direct link to Supported MCP server types")

dbt Wizard supports two transports:

| Type                                                                                                                                        | Description                                                                                                                  |
| ------------------------------------------------------------------------------------------------------------------------------------------- | ---------------------------------------------------------------------------------------------------------------------------- |
| STDIO server (standard input/output, a server that runs as a program on your own computer, instead of one you connect to over the internet) | Runs as a local process that dbt Wizard starts with a command (for example, `npx` or `uvx`). Supports environment variables. |
| Streamable HTTP server                                                                                                                      | A server you reach at a URL. Supports bearer-token and OAuth authentication.                                                 |

Server instructions

For either transport, dbt Wizard reads the `instructions` field the server returns during initialization and uses it as cross-tool guidance.

## Add an MCP server[​](#add-an-mcp-server "Direct link to Add an MCP server")

Use the `wizard mcp add` command, or edit `~/.dbt/wizard/config.toml` directly. Either one writes user-level `[mcp_servers.NAME]` configuration.

* Add a STDIO server
* Add a streamable HTTP server
* Edit config.toml directly

```bash
wizard mcp add SERVER_NAME --env VAR1=value1 -- COMMAND ARGS
```

Where:

* `SERVER_NAME` is a name you choose for the server (for example, `filesystem`).
* `--env VAR1=value1` is optional and repeatable, and applies only to STDIO servers.
* Everything after `--` is the command dbt Wizard runs to launch the server, so the space after `--` is intentional.

For example, add a filesystem MCP server that runs locally through `npx`. This server needs no environment variables, so omit `--env`:

```bash
wizard mcp add filesystem -- npx -y @modelcontextprotocol/server-filesystem /Users/you/my-project
```

To connect the [dbt MCP server](https://docs.getdbt.com/docs/dbt-ai/about-mcp.md), use the streamable HTTP form below — refer to [dbt MCP server](#dbt-mcp-server) under Examples.

```bash
wizard mcp add SERVER_NAME --url https://example.com/mcp --bearer-token-env-var MY_TOKEN
```

To see all MCP subcommands, run `wizard mcp --help`. For the full list of flags, refer to the [CLI command reference](https://docs.getdbt.com/docs/dbt-ai/wizard-cli-reference.md).

Instead of the `wizard mcp add` command, you can edit `config.toml` yourself. dbt Wizard stores user-level MCP configuration in `~/.dbt/wizard/config.toml` alongside its other settings:

\~/.dbt/wizard/config.toml

```toml
# STDIO server (runs locally)
[mcp_servers.filesystem]
command = "npx"
args = ["-y", "@modelcontextprotocol/server-filesystem", "/Users/you/my-project"]

# Streamable HTTP server
[mcp_servers.github]
url = "https://api.githubcopilot.com/mcp/"
bearer_token_env_var = "GITHUB_MCP_TOKEN"
http_headers = { "X-Region" = "us-east-1" }
```

Restart `wizard` after editing `config.toml` — MCP servers are loaded at session start. For how settings resolve, refer to [Config precedence](https://docs.getdbt.com/docs/dbt-ai/wizard-config.md#config-precedence).

## Configuration keys[​](#configuration-keys "Direct link to Configuration keys")

These keys can be set under an `[mcp_servers.NAME]` block in `config.toml`.

| Key                           | Applies to | Description                                                                                                                                     |
| ----------------------------- | ---------- | ----------------------------------------------------------------------------------------------------------------------------------------------- |
| `command`                     | STDIO      | The command that launches the server (for example, `uvx` or `npx`).                                                                             |
| `args`                        | STDIO      | Array of arguments passed to `command`.                                                                                                         |
| `env`                         | STDIO      | Table of environment variables set when launching the server.                                                                                   |
| `env_vars`                    | STDIO      | Names of existing environment variables to pass through to the server.                                                                          |
| `url`                         | HTTP       | The server endpoint for a streamable HTTP server.                                                                                               |
| `bearer_token_env_var`        | HTTP       | Name of the environment variable to read a bearer token from. Sent as `Authorization: Bearer TOKEN`.                                            |
| `http_headers`                | HTTP       | Table of static HTTP headers to send with each request.                                                                                         |
| `env_http_headers`            | HTTP       | HTTP headers whose values are read from environment variables.                                                                                  |
| `scopes`                      | HTTP       | Array of OAuth scopes to request during login. Overrides the scopes the server advertises.                                                      |
| `oauth.client_id`             | HTTP       | OAuth client identifier presented during authorization and token exchange.                                                                      |
| `oauth_resource`              | HTTP       | OAuth resource parameter to include in login requests ([RFC 8707](https://datatracker.ietf.org/doc/html/rfc8707)).                              |
| `enabled`                     | Both       | Whether the server is active. Defaults to `true`.                                                                                               |
| `required`                    | Both       | When `true`, `wizard exec` errors if this server fails to initialize.                                                                           |
| `enabled_tools`               | Both       | Allowlist of tool names to expose from the server.                                                                                              |
| `disabled_tools`              | Both       | Blocklist of tool names to hide from the server.                                                                                                |
| `default_tools_approval_mode` | Both       | Default approval mode for this server's tools: `prompt` (ask before each call) or `auto`/`approve` (run without asking, which behave the same). |
| `startup_timeout_sec`         | Both       | How long to wait for the server to start and list its tools.                                                                                    |
| `tool_timeout_sec`            | Both       | How long to wait for an individual tool call.                                                                                                   |

Set per-tool approvals with a `[mcp_servers.NAME.tools.TOOL_NAME]` block and an `approval_mode` of `auto`, `prompt`, or `approve`:

```toml
[mcp_servers.github.tools.create_pull_request]
approval_mode = "approve"
```

## Authenticate a server[​](#authenticate-a-server "Direct link to Authenticate a server")

If a streamable HTTP server uses OAuth, you must authenticate from the CLI before dbt Wizard can use it. Run:

```bash
wizard mcp login SERVER_NAME
wizard mcp logout SERVER_NAME
```

To request specific scopes at login, pass the `--scopes` CLI flag with a comma-separated list. This requests the same scopes as the `scopes` key in `config.toml`, but only for that login:

```bash
wizard mcp login SERVER_NAME --scopes read,write
```

For servers that use a static token, set `bearer_token_env_var` to the name of an environment variable holding the token, and export that variable before starting `wizard`.

## Manage MCP servers (CLI)[​](#manage-mcp-servers-cli "Direct link to Manage MCP servers (CLI)")

Manage your configured servers through the dbt Wizard CLI, or by editing `config.toml` directly. The CLI provides these commands:

| Command                   | What it does                                                           |
| ------------------------- | ---------------------------------------------------------------------- |
| `wizard mcp list`         | List configured MCP servers. Add `--json` for machine-readable output. |
| `wizard mcp get NAME`     | Show the configuration for one server.                                 |
| `wizard mcp add NAME ...` | Add a STDIO or streamable HTTP server.                                 |
| `wizard mcp remove NAME`  | Remove a server's configuration.                                       |
| `wizard mcp login NAME`   | Authenticate with an OAuth server.                                     |
| `wizard mcp logout NAME`  | Sign out of an OAuth server.                                           |

## Approvals and tool permissions[​](#approvals-and-tool-permissions "Direct link to Approvals and tool permissions")

MCP tool calls follow the same [approval and sandboxing](https://docs.getdbt.com/docs/dbt-ai/wizard-how-it-works.md#approval-and-sandboxing) rules as the rest of dbt Wizard. Set `enabled_tools` and `disabled_tools` in `config.toml` to control which tools a server exposes (there's no dedicated CLI flag). That way, dbt Wizard calls only the tools you intend.

## Examples[​](#examples "Direct link to Examples")

The following examples show common scenarios for adding an MCP server and how to configure them.

### dbt MCP server[​](#dbt-mcp-server "Direct link to dbt MCP server")

The [dbt MCP server](https://docs.getdbt.com/docs/dbt-ai/about-mcp.md) gives dbt Wizard governed access to your project's models, metrics, lineage, freshness, and platform APIs. You can connect it two ways:

* Local (no account required)
* Remote (dbt platform account)

Runs on your machine through `uvx` and works with or without a dbt platform account — the best fit for development:

```bash
wizard mcp add dbt -- uvx dbt-mcp
```

The local server reads its connection settings (such as `DBT_HOST`, `DBT_TOKEN`, and `DBT_PROJECT_DIR`) from environment variables, typically a `.env` file in your dbt project root. You don't need a URL. For setup, refer to [Run dbt locally](https://docs.getdbt.com/docs/dbt-ai/mcp-quickstart-cli.md) and [Set up local MCP](https://docs.getdbt.com/docs/dbt-ai/setup-local-mcp.md).

Hosted on the platform with no local install. Build the URL from your platform host (`https://YOUR_DBT_HOST_URL/api/ai/v1/mcp/`, for example `https://cloud.getdbt.com/api/ai/v1/mcp/`), then authenticate:

```bash
wizard mcp add dbt --url https://YOUR_DBT_HOST_URL/api/ai/v1/mcp/
wizard mcp login dbt
```

For finding your host and token, refer to [Connect to the remote dbt MCP server](https://docs.getdbt.com/docs/dbt-ai/mcp-quickstart-remote.md) and [Connections and authentication (MCP)](https://docs.getdbt.com/docs/dbt-ai/wizard-how-it-works.md#connections-and-authentication-mcp).

Then prompt dbt Wizard:

```text
Use the dbt MCP server to find the most recent failed run for the
nightly job and summarize the error.
```

### GitHub MCP server for pull request review[​](#github-mcp-server-for-pull-request-review "Direct link to GitHub MCP server for pull request review")

Connect a GitHub MCP server so dbt Wizard can read a pull request and post review comments:

\~/.dbt/wizard/config.toml

```toml
[mcp_servers.github]
url = "https://api.githubcopilot.com/mcp/"
# This is the NAME of an environment variable, not the token itself.
# Keep your real token out of this file.
bearer_token_env_var = "GITHUB_MCP_TOKEN"
```

Then set that environment variable to your actual token before starting dbt Wizard:

```bash
export GITHUB_MCP_TOKEN="your-real-token-here"
```

At runtime, dbt Wizard reads the token from the environment and sends it as `Authorization: Bearer <the token>`. Store only the variable name in `config.toml` to keep the secret out of your committed config.

```text
Review the dbt model changes in PR #482 — check for missing tests on
new columns and confirm downstream refs still resolve.
```

## Related docs[​](#related-docs "Direct link to Related docs")

* [dbt MCP server](https://docs.getdbt.com/docs/dbt-ai/about-mcp.md): The dbt-maintained server and its available tools
* [Use subagents with dbt Wizard](https://docs.getdbt.com/docs/dbt-ai/wizard-subagents.md): Delegate work to specialized agents
* [dbt Wizard CLI config](https://docs.getdbt.com/docs/dbt-ai/wizard-config.md): `config.toml` keys and precedence
* [dbt Wizard CLI command reference](https://docs.getdbt.com/docs/dbt-ai/wizard-cli-reference.md): `wizard mcp` flags and subcommands
* [How dbt Wizard works](https://docs.getdbt.com/docs/dbt-ai/wizard-how-it-works.md): Approvals and sandboxing
