# \_wizard-cli-full-generated

## How to read this reference[​](#how-to-read-this-reference "Direct link to How to read this reference")

* **Flag** — the full flag as typed. Short flags (e.g. `-m`) are listed in the Short column.
* **Type** — `boolean` (presence/absence), `string`, `path`, or an enumeration of allowed values.
* **Description** — what the flag does and when to use it.
* Global flags apply to the base interactive `wizard` command. Subcommands can have different flag sets; use each command's section for automation.

See it in action and share your feedback

Want to see dbt Wizard in action? Check out the [demo video](https://www.youtube.com/watch?v=-lIzh1xQWMA).

We'd love to hear how dbt Wizard is working for you. Share your feedback by either running the `/feedback` slash command in your interactive terminal session or by going to the [#dbt-wizard](https://getdbt.slack.com/archives/C0B6KLW6T26) channel in the [dbt Community Slack](https://docs.getdbt.com/community/join?version=2.0).

Thanks so much for your help in improving dbt Wizard and dbt data development!

## Global flags[​](#global-flags "Direct link to Global flags")

These flags work on the base interactive `wizard` command.

| Flag                                         | Short | Type    | Description                                                                                                                                                                                                                                                            |
| -------------------------------------------- | ----- | ------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `-c, --config <key=value>`                   | `-c`  | string  | Override a configuration value that would otherwise be loaded from `~/.dbt/wizard/config.toml`. Use a dotted path (`foo.bar.baz`) to override nested values. The `value` portion is parsed as TOML. If it fails to parse as TOML, the raw string is used as a literal. |
| `--enable <FEATURE>`                         | —     | string  | Enable a feature (repeatable). Equivalent to `-c features.&lt;name&gt;=true`                                                                                                                                                                                           |
| `--disable <FEATURE>`                        | —     | string  | Disable a feature (repeatable). Equivalent to `-c features.&lt;name&gt;=false`                                                                                                                                                                                         |
| `--remote <ADDR>`                            | —     | string  | Connect the TUI to a remote app server endpoint.                                                                                                                                                                                                                       |
| `--remote-auth-token-env <ENV_VAR>`          | —     | string  | Name of the environment variable containing the bearer token to send to a remote app server websocket                                                                                                                                                                  |
| `--strict-config`                            | —     | boolean | Error out when config.toml contains fields that are not recognized by this version of wizard                                                                                                                                                                           |
| `-i, --image <FILE>...`                      | `-i`  | path    | Optional image(s) to attach to the initial prompt                                                                                                                                                                                                                      |
| `-m, --model <MODEL>`                        | `-m`  | enum    | Model the agent should use                                                                                                                                                                                                                                             |
| `--oss`                                      | —     | boolean | Use open-source provider                                                                                                                                                                                                                                               |
| `--local-provider <OSS_PROVIDER>`            | —     | enum    | Specify which local provider to use (lmstudio or ollama). If not specified with --oss, will use config default or show selection                                                                                                                                       |
| `-p, --profile <CONFIG_PROFILE>`             | `-p`  | path    | Configuration profile from config.toml to specify default options                                                                                                                                                                                                      |
| `--profile-v2 <CONFIG_PROFILE_V2>`           | —     | path    | Layer $DBT\_WIZARD\_HOME/\<name>.config.toml on top of the base user config                                                                                                                                                                                            |
| `-s, --sandbox <SANDBOX_MODE>`               | `-s`  | enum    | Select the sandbox policy to use when executing model-generated shell commands                                                                                                                                                                                         |
| `--dangerously-bypass-approvals-and-sandbox` | —     | boolean | Skip all confirmation prompts and execute commands without sandboxing. EXTREMELY DANGEROUS. Intended solely for running in environments that are externally sandboxed                                                                                                  |
| `--dangerously-bypass-hook-trust`            | —     | boolean | Run enabled hooks without requiring persisted hook trust for this invocation. DANGEROUS. Intended only for automation that already vets hook sources                                                                                                                   |
| `-C, --cd <DIR>`                             | `-C`  | path    | Tell the agent to use the specified directory as its working root                                                                                                                                                                                                      |
| `--add-dir <DIR>`                            | —     | path    | Additional directories that should be writable alongside the primary workspace                                                                                                                                                                                         |
| `-a, --ask-for-approval <APPROVAL_POLICY>`   | `-a`  | enum    | Configure when the model requires human approval before executing a command                                                                                                                                                                                            |
| `--search`                                   | —     | boolean | Enable live web search. When enabled, the native Responses `web_search` tool is available to the model (no per‑call approval)                                                                                                                                          |
| `--no-alt-screen`                            | —     | boolean | Disable alternate screen mode                                                                                                                                                                                                                                          |

Search table...

|                  |   |   |   |   |
| ---------------- | - | - | - | - |
| Loading table... |   |   |   |   |

## Commands[​](#commands "Direct link to Commands")

| Command                             | Aliases | Description                                                                                       |
| ----------------------------------- | ------- | ------------------------------------------------------------------------------------------------- |
| [`exec`](#exec)                     | `e`     | Run wizard non-interactively                                                                      |
| [`review`](#review)                 | —       | Run a code review non-interactively                                                               |
| [`login`](#login)                   | —       | Manage login                                                                                      |
| [`logout`](#logout)                 | —       | Remove stored authentication credentials                                                          |
| [`mcp`](#mcp)                       | —       | Manage external MCP servers for wizard                                                            |
| [`plugin`](#plugin)                 | —       | Manage wizard plugins                                                                             |
| [`providers`](#providers)           | —       | Manage model providers                                                                            |
| [`mcp-server`](#mcp-server)         | —       | Start wizard as an MCP server (stdio)                                                             |
| [`app-server`](#app-server)         | —       | \[experimental] Run the app server or related tooling                                             |
| [`remote-control`](#remote-control) | —       | \[experimental] Manage the app-server daemon with remote control enabled                          |
| [`completion`](#completion)         | —       | Generate shell completion scripts                                                                 |
| [`update`](#update)                 | —       | Update wizard to the latest version                                                               |
| [`doctor`](#doctor)                 | —       | Diagnose local wizard installation, config, auth, and runtime health                              |
| [`sandbox`](#sandbox)               | —       | Run commands within a wizard-provided sandbox                                                     |
| [`debug`](#debug)                   | —       | Debugging tools                                                                                   |
| [`apply`](#apply)                   | `a`     | Apply the latest diff produced by wizard agent as a `git apply` to your local working tree        |
| [`resume`](#resume)                 | —       | Resume a previous interactive session (picker by default; use --last to continue the most recent) |
| [`fork`](#fork)                     | —       | Fork a previous interactive session (picker by default; use --last to fork the most recent)       |
| [`cloud`](#cloud)                   | —       | \[EXPERIMENTAL] Browse tasks from wizard in dbt platform and apply changes locally                |
| [`exec-server`](#exec-server)       | —       | \[EXPERIMENTAL] Run the standalone exec-server service                                            |
| [`features`](#features)             | —       | Inspect feature flags                                                                             |

Search table...

|                  |   |   |   |   |
| ---------------- | - | - | - | - |
| Loading table... |   |   |   |   |

## exec[​](#exec "Direct link to exec")

Run wizard non-interactively

```bash
wizard exec [OPTIONS] <COMMAND> [ARGS]
```

**Arguments:**

| Argument   | Description                                                                                                                                                                                                             |
| ---------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `[PROMPT]` | Initial instructions for the agent. If not provided as an argument (or if `-` is used), instructions are read from stdin. If stdin is piped and a prompt is also provided, stdin is appended as a `&lt;stdin&gt;` block |

Search table...

|                  |   |   |   |   |
| ---------------- | - | - | - | - |
| Loading table... |   |   |   |   |

| Flag                                         | Short | Type    | Description                                                                                                                                                                                                                                                            |
| -------------------------------------------- | ----- | ------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `-c, --config <key=value>`                   | `-c`  | string  | Override a configuration value that would otherwise be loaded from `~/.dbt/wizard/config.toml`. Use a dotted path (`foo.bar.baz`) to override nested values. The `value` portion is parsed as TOML. If it fails to parse as TOML, the raw string is used as a literal. |
| `--enable <FEATURE>`                         | —     | string  | Enable a feature (repeatable). Equivalent to `-c features.&lt;name&gt;=true`                                                                                                                                                                                           |
| `--disable <FEATURE>`                        | —     | string  | Disable a feature (repeatable). Equivalent to `-c features.&lt;name&gt;=false`                                                                                                                                                                                         |
| `--strict-config`                            | —     | boolean | Error out when config.toml contains fields that are not recognized by this version of wizard                                                                                                                                                                           |
| `-i, --image <FILE>...`                      | `-i`  | path    | Optional image(s) to attach to the initial prompt                                                                                                                                                                                                                      |
| `-m, --model <MODEL>`                        | `-m`  | enum    | Model the agent should use                                                                                                                                                                                                                                             |
| `--oss`                                      | —     | boolean | Use open-source provider                                                                                                                                                                                                                                               |
| `--local-provider <OSS_PROVIDER>`            | —     | enum    | Specify which local provider to use (lmstudio or ollama). If not specified with --oss, will use config default or show selection                                                                                                                                       |
| `-p, --profile <CONFIG_PROFILE>`             | `-p`  | path    | Configuration profile from config.toml to specify default options                                                                                                                                                                                                      |
| `--profile-v2 <CONFIG_PROFILE_V2>`           | —     | path    | Layer $DBT\_WIZARD\_HOME/\<name>.config.toml on top of the base user config                                                                                                                                                                                            |
| `-s, --sandbox <SANDBOX_MODE>`               | `-s`  | enum    | Select the sandbox policy to use when executing model-generated shell commands                                                                                                                                                                                         |
| `--dangerously-bypass-approvals-and-sandbox` | —     | boolean | Skip all confirmation prompts and execute commands without sandboxing. EXTREMELY DANGEROUS. Intended solely for running in environments that are externally sandboxed                                                                                                  |
| `--dangerously-bypass-hook-trust`            | —     | boolean | Run enabled hooks without requiring persisted hook trust for this invocation. DANGEROUS. Intended only for automation that already vets hook sources                                                                                                                   |
| `-C, --cd <DIR>`                             | `-C`  | path    | Tell the agent to use the specified directory as its working root                                                                                                                                                                                                      |
| `--add-dir <DIR>`                            | —     | path    | Additional directories that should be writable alongside the primary workspace                                                                                                                                                                                         |
| `--skip-git-repo-check`                      | —     | boolean | Allow running wizard outside a Git repository                                                                                                                                                                                                                          |
| `--ephemeral`                                | —     | boolean | Run without persisting session files to disk                                                                                                                                                                                                                           |
| `--ignore-user-config`                       | —     | boolean | Do not load `$DBT_WIZARD_HOME/config.toml`; auth still uses `DBT_WIZARD_HOME`                                                                                                                                                                                          |
| `--ignore-rules`                             | —     | boolean | Do not load user or project execpolicy `.rules` files                                                                                                                                                                                                                  |
| `--output-schema <FILE>`                     | —     | path    | Path to a JSON Schema file describing the model's final response shape                                                                                                                                                                                                 |
| `--color <COLOR>`                            | —     | enum    | Specifies color settings for use in the output                                                                                                                                                                                                                         |
| `--json`                                     | —     | boolean | Print events to stdout as JSONL                                                                                                                                                                                                                                        |
| `--include-subagent-events`                  | —     | boolean | When `--json` is set, capture and emit sub-agent events tagged with their thread ID. Each sub-agent event gets a `sub_agent_thread_id` field added to the JSONL line so consumers can distinguish it from the primary agent's events                                   |
| `--no-validation`                            | —     | boolean | When set, the validation gate always responds with "declined" so the validation sub-agent is never spawned. Useful for benchmarking runs where sub-agent overhead is not desired                                                                                       |
| `-o, --output-last-message <FILE>`           | `-o`  | path    | Specifies file where the last message from the agent should be written                                                                                                                                                                                                 |

Search table...

|                  |   |   |   |   |
| ---------------- | - | - | - | - |
| Loading table... |   |   |   |   |

## review[​](#review "Direct link to review")

Run a code review non-interactively

**Arguments:**

| Argument   | Description                                                 |
| ---------- | ----------------------------------------------------------- |
| `[PROMPT]` | Custom review instructions. If `-` is used, read from stdin |

Search table...

|                  |   |   |   |   |
| ---------------- | - | - | - | - |
| Loading table... |   |   |   |   |

| Flag                       | Short | Type    | Description                                                                                                                                                                                                                                                            |
| -------------------------- | ----- | ------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `-c, --config <key=value>` | `-c`  | string  | Override a configuration value that would otherwise be loaded from `~/.dbt/wizard/config.toml`. Use a dotted path (`foo.bar.baz`) to override nested values. The `value` portion is parsed as TOML. If it fails to parse as TOML, the raw string is used as a literal. |
| `--strict-config`          | —     | boolean | Error out when config.toml contains fields that are not recognized by this version of wizard                                                                                                                                                                           |
| `--enable <FEATURE>`       | —     | string  | Enable a feature (repeatable). Equivalent to `-c features.&lt;name&gt;=true`                                                                                                                                                                                           |
| `--uncommitted`            | —     | boolean | Review staged, unstaged, and untracked changes                                                                                                                                                                                                                         |
| `--base <BRANCH>`          | —     | string  | Review changes against the given base branch                                                                                                                                                                                                                           |
| `--disable <FEATURE>`      | —     | string  | Disable a feature (repeatable). Equivalent to `-c features.&lt;name&gt;=false`                                                                                                                                                                                         |
| `--commit <SHA>`           | —     | string  | Review the changes introduced by a commit                                                                                                                                                                                                                              |
| `--title <TITLE>`          | —     | string  | Optional commit title to display in the review summary                                                                                                                                                                                                                 |

Search table...

|                  |   |   |   |   |
| ---------------- | - | - | - | - |
| Loading table... |   |   |   |   |

## login[​](#login "Direct link to login")

Manage login

| Flag                       | Short | Type   | Description                                                                                                                                                                                                                                                            |
| -------------------------- | ----- | ------ | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `-c, --config <key=value>` | `-c`  | string | Override a configuration value that would otherwise be loaded from `~/.dbt/wizard/config.toml`. Use a dotted path (`foo.bar.baz`) to override nested values. The `value` portion is parsed as TOML. If it fails to parse as TOML, the raw string is used as a literal. |
| `--enable <FEATURE>`       | —     | string | Enable a feature (repeatable). Equivalent to `-c features.&lt;name&gt;=true`                                                                                                                                                                                           |
| `--disable <FEATURE>`      | —     | string | Disable a feature (repeatable). Equivalent to `-c features.&lt;name&gt;=false`                                                                                                                                                                                         |

Search table...

|                  |   |   |   |   |
| ---------------- | - | - | - | - |
| Loading table... |   |   |   |   |

## logout[​](#logout "Direct link to logout")

Remove stored authentication credentials

| Flag                       | Short | Type   | Description                                                                                                                                                                                                                                                            |
| -------------------------- | ----- | ------ | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `-c, --config <key=value>` | `-c`  | string | Override a configuration value that would otherwise be loaded from `~/.dbt/wizard/config.toml`. Use a dotted path (`foo.bar.baz`) to override nested values. The `value` portion is parsed as TOML. If it fails to parse as TOML, the raw string is used as a literal. |
| `--enable <FEATURE>`       | —     | string | Enable a feature (repeatable). Equivalent to `-c features.&lt;name&gt;=true`                                                                                                                                                                                           |
| `--disable <FEATURE>`      | —     | string | Disable a feature (repeatable). Equivalent to `-c features.&lt;name&gt;=false`                                                                                                                                                                                         |

Search table...

|                  |   |   |   |   |
| ---------------- | - | - | - | - |
| Loading table... |   |   |   |   |

## mcp[​](#mcp "Direct link to mcp")

Manage external MCP servers for dbt Wizard

| Flag                       | Short | Type   | Description                                                                                                                                                                                                                                                            |
| -------------------------- | ----- | ------ | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `-c, --config <key=value>` | `-c`  | string | Override a configuration value that would otherwise be loaded from `~/.dbt/wizard/config.toml`. Use a dotted path (`foo.bar.baz`) to override nested values. The `value` portion is parsed as TOML. If it fails to parse as TOML, the raw string is used as a literal. |
| `--enable <FEATURE>`       | —     | string | Enable a feature (repeatable). Equivalent to `-c features.&lt;name&gt;=true`                                                                                                                                                                                           |
| `--disable <FEATURE>`      | —     | string | Disable a feature (repeatable). Equivalent to `-c features.&lt;name&gt;=false`                                                                                                                                                                                         |

Search table...

|                  |   |   |   |   |
| ---------------- | - | - | - | - |
| Loading table... |   |   |   |   |

## plugin[​](#plugin "Direct link to plugin")

Manage dbt Wizard plugins

| Flag                       | Short | Type   | Description                                                                                                                                                                                                                                                            |
| -------------------------- | ----- | ------ | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `-c, --config <key=value>` | `-c`  | string | Override a configuration value that would otherwise be loaded from `~/.dbt/wizard/config.toml`. Use a dotted path (`foo.bar.baz`) to override nested values. The `value` portion is parsed as TOML. If it fails to parse as TOML, the raw string is used as a literal. |
| `--enable <FEATURE>`       | —     | string | Enable a feature (repeatable). Equivalent to `-c features.&lt;name&gt;=true`                                                                                                                                                                                           |
| `--disable <FEATURE>`      | —     | string | Disable a feature (repeatable). Equivalent to `-c features.&lt;name&gt;=false`                                                                                                                                                                                         |

Search table...

|                  |   |   |   |   |
| ---------------- | - | - | - | - |
| Loading table... |   |   |   |   |

## providers[​](#providers "Direct link to providers")

Manage model providers

| Flag                       | Short | Type   | Description                                                                                                                                                                                                                                                            |
| -------------------------- | ----- | ------ | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `-c, --config <key=value>` | `-c`  | string | Override a configuration value that would otherwise be loaded from `~/.dbt/wizard/config.toml`. Use a dotted path (`foo.bar.baz`) to override nested values. The `value` portion is parsed as TOML. If it fails to parse as TOML, the raw string is used as a literal. |
| `--enable <FEATURE>`       | —     | string | Enable a feature (repeatable). Equivalent to `-c features.&lt;name&gt;=true`                                                                                                                                                                                           |
| `--disable <FEATURE>`      | —     | string | Disable a feature (repeatable). Equivalent to `-c features.&lt;name&gt;=false`                                                                                                                                                                                         |

Search table...

|                  |   |   |   |   |
| ---------------- | - | - | - | - |
| Loading table... |   |   |   |   |

## mcp-server[​](#mcp-server "Direct link to mcp-server")

Start dbt Wizard as an MCP server (stdio)

| Flag                       | Short | Type    | Description                                                                                                                                                                                                                                                            |
| -------------------------- | ----- | ------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `-c, --config <key=value>` | `-c`  | string  | Override a configuration value that would otherwise be loaded from `~/.dbt/wizard/config.toml`. Use a dotted path (`foo.bar.baz`) to override nested values. The `value` portion is parsed as TOML. If it fails to parse as TOML, the raw string is used as a literal. |
| `--strict-config`          | —     | boolean | Error out when config.toml contains fields that are not recognized by this version of wizard                                                                                                                                                                           |
| `--enable <FEATURE>`       | —     | string  | Enable a feature (repeatable). Equivalent to `-c features.&lt;name&gt;=true`                                                                                                                                                                                           |
| `--disable <FEATURE>`      | —     | string  | Disable a feature (repeatable). Equivalent to `-c features.&lt;name&gt;=false`                                                                                                                                                                                         |

Search table...

|                  |   |   |   |   |
| ---------------- | - | - | - | - |
| Loading table... |   |   |   |   |

## app-server[​](#app-server "Direct link to app-server")

\[experimental] Run the app server or related tooling

| Flag                                    | Short | Type    | Description                                                                                                                                                                                                                                                            |
| --------------------------------------- | ----- | ------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `-c, --config <key=value>`              | `-c`  | string  | Override a configuration value that would otherwise be loaded from `~/.dbt/wizard/config.toml`. Use a dotted path (`foo.bar.baz`) to override nested values. The `value` portion is parsed as TOML. If it fails to parse as TOML, the raw string is used as a literal. |
| `--enable <FEATURE>`                    | —     | string  | Enable a feature (repeatable). Equivalent to `-c features.&lt;name&gt;=true`                                                                                                                                                                                           |
| `--disable <FEATURE>`                   | —     | string  | Disable a feature (repeatable). Equivalent to `-c features.&lt;name&gt;=false`                                                                                                                                                                                         |
| `--strict-config`                       | —     | boolean | Error out when config.toml contains fields that are not recognized by this version of wizard                                                                                                                                                                           |
| `--listen <URL>`                        | —     | string  | Transport endpoint URL. Supported values: `stdio://` (default), `unix://`, `unix://PATH`, `ws://IP:PORT`, `off`                                                                                                                                                        |
| `--analytics-default-enabled`           | —     | boolean | Controls whether analytics are enabled by default.                                                                                                                                                                                                                     |
| `--ws-auth <MODE>`                      | —     | enum    | Websocket auth mode for non-loopback listeners                                                                                                                                                                                                                         |
| `--ws-token-file <PATH>`                | —     | path    | Absolute path to the capability-token file                                                                                                                                                                                                                             |
| `--ws-token-sha256 <HEX>`               | —     | string  | Hex-encoded SHA-256 digest of the capability token                                                                                                                                                                                                                     |
| `--ws-shared-secret-file <PATH>`        | —     | path    | Absolute path to the shared secret file for signed JWT bearer tokens                                                                                                                                                                                                   |
| `--ws-issuer <ISSUER>`                  | —     | string  | Expected issuer for signed JWT bearer tokens                                                                                                                                                                                                                           |
| `--ws-audience <AUDIENCE>`              | —     | string  | Expected audience for signed JWT bearer tokens                                                                                                                                                                                                                         |
| `--ws-max-clock-skew-seconds <SECONDS>` | —     | string  | Maximum clock skew when validating signed JWT bearer tokens                                                                                                                                                                                                            |

Search table...

|                  |   |   |   |   |
| ---------------- | - | - | - | - |
| Loading table... |   |   |   |   |

## remote-control[​](#remote-control "Direct link to remote-control")

\[experimental] Manage the app-server daemon with remote control enabled

| Flag                       | Short | Type    | Description                                                                                                                                                                                                                                                            |
| -------------------------- | ----- | ------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `-c, --config <key=value>` | `-c`  | string  | Override a configuration value that would otherwise be loaded from `~/.dbt/wizard/config.toml`. Use a dotted path (`foo.bar.baz`) to override nested values. The `value` portion is parsed as TOML. If it fails to parse as TOML, the raw string is used as a literal. |
| `--json`                   | —     | boolean | Emit machine-readable JSON                                                                                                                                                                                                                                             |
| `--enable <FEATURE>`       | —     | string  | Enable a feature (repeatable). Equivalent to `-c features.&lt;name&gt;=true`                                                                                                                                                                                           |
| `--disable <FEATURE>`      | —     | string  | Disable a feature (repeatable). Equivalent to `-c features.&lt;name&gt;=false`                                                                                                                                                                                         |

Search table...

|                  |   |   |   |   |
| ---------------- | - | - | - | - |
| Loading table... |   |   |   |   |

## completion[​](#completion "Direct link to completion")

Generate shell completion scripts

**Arguments:**

| Argument  | Description                       |
| --------- | --------------------------------- |
| `[SHELL]` | Shell to generate completions for |

Search table...

|                  |   |   |   |   |
| ---------------- | - | - | - | - |
| Loading table... |   |   |   |   |

| Flag                       | Short | Type   | Description                                                                                                                                                                                                                                                            |
| -------------------------- | ----- | ------ | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `-c, --config <key=value>` | `-c`  | string | Override a configuration value that would otherwise be loaded from `~/.dbt/wizard/config.toml`. Use a dotted path (`foo.bar.baz`) to override nested values. The `value` portion is parsed as TOML. If it fails to parse as TOML, the raw string is used as a literal. |
| `--enable <FEATURE>`       | —     | string | Enable a feature (repeatable). Equivalent to `-c features.&lt;name&gt;=true`                                                                                                                                                                                           |
| `--disable <FEATURE>`      | —     | string | Disable a feature (repeatable). Equivalent to `-c features.&lt;name&gt;=false`                                                                                                                                                                                         |

Search table...

|                  |   |   |   |   |
| ---------------- | - | - | - | - |
| Loading table... |   |   |   |   |

## update[​](#update "Direct link to update")

Update dbt Wizard to the latest version

| Flag                       | Short | Type   | Description                                                                                                                                                                                                                                                            |
| -------------------------- | ----- | ------ | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `-c, --config <key=value>` | `-c`  | string | Override a configuration value that would otherwise be loaded from `~/.dbt/wizard/config.toml`. Use a dotted path (`foo.bar.baz`) to override nested values. The `value` portion is parsed as TOML. If it fails to parse as TOML, the raw string is used as a literal. |
| `--enable <FEATURE>`       | —     | string | Enable a feature (repeatable). Equivalent to `-c features.&lt;name&gt;=true`                                                                                                                                                                                           |
| `--disable <FEATURE>`      | —     | string | Disable a feature (repeatable). Equivalent to `-c features.&lt;name&gt;=false`                                                                                                                                                                                         |

Search table...

|                  |   |   |   |   |
| ---------------- | - | - | - | - |
| Loading table... |   |   |   |   |

## doctor[​](#doctor "Direct link to doctor")

Diagnose local dbt Wizard installation, config, auth, and runtime health

| Flag                       | Short | Type    | Description                                                                                                                                                                                                                                                            |
| -------------------------- | ----- | ------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `-c, --config <key=value>` | `-c`  | string  | Override a configuration value that would otherwise be loaded from `~/.dbt/wizard/config.toml`. Use a dotted path (`foo.bar.baz`) to override nested values. The `value` portion is parsed as TOML. If it fails to parse as TOML, the raw string is used as a literal. |
| `--json`                   | —     | boolean | Emit a redacted machine-readable report                                                                                                                                                                                                                                |
| `--enable <FEATURE>`       | —     | string  | Enable a feature (repeatable). Equivalent to `-c features.&lt;name&gt;=true`                                                                                                                                                                                           |
| `--summary`                | —     | boolean | Only show grouped check rows and the final count summary                                                                                                                                                                                                               |
| `--all`                    | —     | boolean | Expand long lists in detailed human output                                                                                                                                                                                                                             |
| `--disable <FEATURE>`      | —     | string  | Disable a feature (repeatable). Equivalent to `-c features.&lt;name&gt;=false`                                                                                                                                                                                         |
| `--no-color`               | —     | boolean | Disable ANSI color in human output                                                                                                                                                                                                                                     |
| `--ascii`                  | —     | boolean | Use ASCII status labels and separators in human output                                                                                                                                                                                                                 |

Search table...

|                  |   |   |   |   |
| ---------------- | - | - | - | - |
| Loading table... |   |   |   |   |

## sandbox[​](#sandbox "Direct link to sandbox")

Run commands within a dbt Wizard-provided sandbox

| Flag                       | Short | Type   | Description                                                                                                                                                                                                                                                            |
| -------------------------- | ----- | ------ | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `-c, --config <key=value>` | `-c`  | string | Override a configuration value that would otherwise be loaded from `~/.dbt/wizard/config.toml`. Use a dotted path (`foo.bar.baz`) to override nested values. The `value` portion is parsed as TOML. If it fails to parse as TOML, the raw string is used as a literal. |
| `--enable <FEATURE>`       | —     | string | Enable a feature (repeatable). Equivalent to `-c features.&lt;name&gt;=true`                                                                                                                                                                                           |
| `--disable <FEATURE>`      | —     | string | Disable a feature (repeatable). Equivalent to `-c features.&lt;name&gt;=false`                                                                                                                                                                                         |

Search table...

|                  |   |   |   |   |
| ---------------- | - | - | - | - |
| Loading table... |   |   |   |   |

## debug[​](#debug "Direct link to debug")

Debugging tools

| Flag                       | Short | Type   | Description                                                                                                                                                                                                                                                            |
| -------------------------- | ----- | ------ | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `-c, --config <key=value>` | `-c`  | string | Override a configuration value that would otherwise be loaded from `~/.dbt/wizard/config.toml`. Use a dotted path (`foo.bar.baz`) to override nested values. The `value` portion is parsed as TOML. If it fails to parse as TOML, the raw string is used as a literal. |
| `--enable <FEATURE>`       | —     | string | Enable a feature (repeatable). Equivalent to `-c features.&lt;name&gt;=true`                                                                                                                                                                                           |
| `--disable <FEATURE>`      | —     | string | Disable a feature (repeatable). Equivalent to `-c features.&lt;name&gt;=false`                                                                                                                                                                                         |

Search table...

|                  |   |   |   |   |
| ---------------- | - | - | - | - |
| Loading table... |   |   |   |   |

## apply[​](#apply "Direct link to apply")

Apply the latest diff produced by dbt Wizard agent as a `git apply` to your local working tree

**Arguments:**

| Argument    | Description |
| ----------- | ----------- |
| `<TASK_ID>` |             |

Search table...

|                  |   |   |   |   |
| ---------------- | - | - | - | - |
| Loading table... |   |   |   |   |

| Flag                       | Short | Type   | Description                                                                                                                                                                                                                                                            |
| -------------------------- | ----- | ------ | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `-c, --config <key=value>` | `-c`  | string | Override a configuration value that would otherwise be loaded from `~/.dbt/wizard/config.toml`. Use a dotted path (`foo.bar.baz`) to override nested values. The `value` portion is parsed as TOML. If it fails to parse as TOML, the raw string is used as a literal. |
| `--enable <FEATURE>`       | —     | string | Enable a feature (repeatable). Equivalent to `-c features.&lt;name&gt;=true`                                                                                                                                                                                           |
| `--disable <FEATURE>`      | —     | string | Disable a feature (repeatable). Equivalent to `-c features.&lt;name&gt;=false`                                                                                                                                                                                         |

Search table...

|                  |   |   |   |   |
| ---------------- | - | - | - | - |
| Loading table... |   |   |   |   |

## resume[​](#resume "Direct link to resume")

Resume a previous interactive session (picker by default; use --last to continue the most recent)

**Arguments:**

| Argument       | Description                                                                                                                                        |
| -------------- | -------------------------------------------------------------------------------------------------------------------------------------------------- |
| `[SESSION_ID]` | Conversation/session id (UUID) or thread name. UUIDs take precedence if it parses. If omitted, use --last to pick the most recent recorded session |
| `[PROMPT]`     | Optional user prompt to start the session                                                                                                          |

Search table...

|                  |   |   |   |   |
| ---------------- | - | - | - | - |
| Loading table... |   |   |   |   |

| Flag                                         | Short | Type    | Description                                                                                                                                                                                                                                                            |
| -------------------------------------------- | ----- | ------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `-c, --config <key=value>`                   | `-c`  | string  | Override a configuration value that would otherwise be loaded from `~/.dbt/wizard/config.toml`. Use a dotted path (`foo.bar.baz`) to override nested values. The `value` portion is parsed as TOML. If it fails to parse as TOML, the raw string is used as a literal. |
| `--last`                                     | —     | boolean | Continue the most recent session without showing the picker                                                                                                                                                                                                            |
| `--all`                                      | —     | boolean | Show all sessions (disables cwd filtering and shows CWD column)                                                                                                                                                                                                        |
| `--enable <FEATURE>`                         | —     | string  | Enable a feature (repeatable). Equivalent to `-c features.&lt;name&gt;=true`                                                                                                                                                                                           |
| `--disable <FEATURE>`                        | —     | string  | Disable a feature (repeatable). Equivalent to `-c features.&lt;name&gt;=false`                                                                                                                                                                                         |
| `--include-non-interactive`                  | —     | boolean | Include non-interactive sessions in the resume picker and --last selection                                                                                                                                                                                             |
| `--remote <ADDR>`                            | —     | string  | Connect the TUI to a remote app server endpoint.                                                                                                                                                                                                                       |
| `--remote-auth-token-env <ENV_VAR>`          | —     | string  | Name of the environment variable containing the bearer token to send to a remote app server websocket                                                                                                                                                                  |
| `--strict-config`                            | —     | boolean | Error out when config.toml contains fields that are not recognized by this version of wizard                                                                                                                                                                           |
| `-i, --image <FILE>...`                      | `-i`  | path    | Optional image(s) to attach to the initial prompt                                                                                                                                                                                                                      |
| `-m, --model <MODEL>`                        | `-m`  | enum    | Model the agent should use                                                                                                                                                                                                                                             |
| `--oss`                                      | —     | boolean | Use open-source provider                                                                                                                                                                                                                                               |
| `--local-provider <OSS_PROVIDER>`            | —     | enum    | Specify which local provider to use (lmstudio or ollama). If not specified with --oss, will use config default or show selection                                                                                                                                       |
| `-p, --profile <CONFIG_PROFILE>`             | `-p`  | path    | Configuration profile from config.toml to specify default options                                                                                                                                                                                                      |
| `--profile-v2 <CONFIG_PROFILE_V2>`           | —     | path    | Layer $DBT\_WIZARD\_HOME/\<name>.config.toml on top of the base user config                                                                                                                                                                                            |
| `-s, --sandbox <SANDBOX_MODE>`               | `-s`  | enum    | Select the sandbox policy to use when executing model-generated shell commands                                                                                                                                                                                         |
| `--dangerously-bypass-approvals-and-sandbox` | —     | boolean | Skip all confirmation prompts and execute commands without sandboxing. EXTREMELY DANGEROUS. Intended solely for running in environments that are externally sandboxed                                                                                                  |
| `--dangerously-bypass-hook-trust`            | —     | boolean | Run enabled hooks without requiring persisted hook trust for this invocation. DANGEROUS. Intended only for automation that already vets hook sources                                                                                                                   |
| `-C, --cd <DIR>`                             | `-C`  | path    | Tell the agent to use the specified directory as its working root                                                                                                                                                                                                      |
| `--add-dir <DIR>`                            | —     | path    | Additional directories that should be writable alongside the primary workspace                                                                                                                                                                                         |
| `-a, --ask-for-approval <APPROVAL_POLICY>`   | `-a`  | enum    | Configure when the model requires human approval before executing a command                                                                                                                                                                                            |
| `--search`                                   | —     | boolean | Enable live web search. When enabled, the native Responses `web_search` tool is available to the model (no per‑call approval)                                                                                                                                          |
| `--no-alt-screen`                            | —     | boolean | Disable alternate screen mode                                                                                                                                                                                                                                          |

Search table...

|                  |   |   |   |   |
| ---------------- | - | - | - | - |
| Loading table... |   |   |   |   |

## fork[​](#fork "Direct link to fork")

Fork a previous interactive session (picker by default; use --last to fork the most recent)

**Arguments:**

| Argument       | Description                                                                                                                        |
| -------------- | ---------------------------------------------------------------------------------------------------------------------------------- |
| `[SESSION_ID]` | Conversation/session id (UUID). When provided, forks this session. If omitted, use --last to pick the most recent recorded session |
| `[PROMPT]`     | Optional user prompt to start the session                                                                                          |

Search table...

|                  |   |   |   |   |
| ---------------- | - | - | - | - |
| Loading table... |   |   |   |   |

| Flag                                         | Short | Type    | Description                                                                                                                                                                                                                                                            |
| -------------------------------------------- | ----- | ------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `-c, --config <key=value>`                   | `-c`  | string  | Override a configuration value that would otherwise be loaded from `~/.dbt/wizard/config.toml`. Use a dotted path (`foo.bar.baz`) to override nested values. The `value` portion is parsed as TOML. If it fails to parse as TOML, the raw string is used as a literal. |
| `--last`                                     | —     | boolean | Fork the most recent session without showing the picker                                                                                                                                                                                                                |
| `--all`                                      | —     | boolean | Show all sessions (disables cwd filtering and shows CWD column)                                                                                                                                                                                                        |
| `--enable <FEATURE>`                         | —     | string  | Enable a feature (repeatable). Equivalent to `-c features.&lt;name&gt;=true`                                                                                                                                                                                           |
| `--disable <FEATURE>`                        | —     | string  | Disable a feature (repeatable). Equivalent to `-c features.&lt;name&gt;=false`                                                                                                                                                                                         |
| `--remote <ADDR>`                            | —     | string  | Connect the TUI to a remote app server endpoint.                                                                                                                                                                                                                       |
| `--remote-auth-token-env <ENV_VAR>`          | —     | string  | Name of the environment variable containing the bearer token to send to a remote app server websocket                                                                                                                                                                  |
| `--strict-config`                            | —     | boolean | Error out when config.toml contains fields that are not recognized by this version of wizard                                                                                                                                                                           |
| `-i, --image <FILE>...`                      | `-i`  | path    | Optional image(s) to attach to the initial prompt                                                                                                                                                                                                                      |
| `-m, --model <MODEL>`                        | `-m`  | enum    | Model the agent should use                                                                                                                                                                                                                                             |
| `--oss`                                      | —     | boolean | Use open-source provider                                                                                                                                                                                                                                               |
| `--local-provider <OSS_PROVIDER>`            | —     | enum    | Specify which local provider to use (lmstudio or ollama). If not specified with --oss, will use config default or show selection                                                                                                                                       |
| `-p, --profile <CONFIG_PROFILE>`             | `-p`  | path    | Configuration profile from config.toml to specify default options                                                                                                                                                                                                      |
| `--profile-v2 <CONFIG_PROFILE_V2>`           | —     | path    | Layer $DBT\_WIZARD\_HOME/\<name>.config.toml on top of the base user config                                                                                                                                                                                            |
| `-s, --sandbox <SANDBOX_MODE>`               | `-s`  | enum    | Select the sandbox policy to use when executing model-generated shell commands                                                                                                                                                                                         |
| `--dangerously-bypass-approvals-and-sandbox` | —     | boolean | Skip all confirmation prompts and execute commands without sandboxing. EXTREMELY DANGEROUS. Intended solely for running in environments that are externally sandboxed                                                                                                  |
| `--dangerously-bypass-hook-trust`            | —     | boolean | Run enabled hooks without requiring persisted hook trust for this invocation. DANGEROUS. Intended only for automation that already vets hook sources                                                                                                                   |
| `-C, --cd <DIR>`                             | `-C`  | path    | Tell the agent to use the specified directory as its working root                                                                                                                                                                                                      |
| `--add-dir <DIR>`                            | —     | path    | Additional directories that should be writable alongside the primary workspace                                                                                                                                                                                         |
| `-a, --ask-for-approval <APPROVAL_POLICY>`   | `-a`  | enum    | Configure when the model requires human approval before executing a command                                                                                                                                                                                            |
| `--search`                                   | —     | boolean | Enable live web search. When enabled, the native Responses `web_search` tool is available to the model (no per‑call approval)                                                                                                                                          |
| `--no-alt-screen`                            | —     | boolean | Disable alternate screen mode                                                                                                                                                                                                                                          |

Search table...

|                  |   |   |   |   |
| ---------------- | - | - | - | - |
| Loading table... |   |   |   |   |

## cloud[​](#cloud "Direct link to cloud")

\[EXPERIMENTAL] Browse tasks from dbt Wizard in dbt platform and apply changes locally

| Flag                       | Short | Type   | Description                                                                                                                                                                                                                                                            |
| -------------------------- | ----- | ------ | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `-c, --config <key=value>` | `-c`  | string | Override a configuration value that would otherwise be loaded from `~/.dbt/wizard/config.toml`. Use a dotted path (`foo.bar.baz`) to override nested values. The `value` portion is parsed as TOML. If it fails to parse as TOML, the raw string is used as a literal. |
| `--enable <FEATURE>`       | —     | string | Enable a feature (repeatable). Equivalent to `-c features.&lt;name&gt;=true`                                                                                                                                                                                           |
| `--disable <FEATURE>`      | —     | string | Disable a feature (repeatable). Equivalent to `-c features.&lt;name&gt;=false`                                                                                                                                                                                         |

Search table...

|                  |   |   |   |   |
| ---------------- | - | - | - | - |
| Loading table... |   |   |   |   |

## exec-server[​](#exec-server "Direct link to exec-server")

\[EXPERIMENTAL] Run the standalone exec-server service

| Flag                        | Short | Type    | Description                                                                                                                                                                                                                                                            |
| --------------------------- | ----- | ------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `-c, --config <key=value>`  | `-c`  | string  | Override a configuration value that would otherwise be loaded from `~/.dbt/wizard/config.toml`. Use a dotted path (`foo.bar.baz`) to override nested values. The `value` portion is parsed as TOML. If it fails to parse as TOML, the raw string is used as a literal. |
| `--listen <URL>`            | —     | string  | Transport endpoint URL. Supported values: `ws://IP:PORT` (default), `stdio`, `stdio://`                                                                                                                                                                                |
| `--enable <FEATURE>`        | —     | string  | Enable a feature (repeatable). Equivalent to `-c features.&lt;name&gt;=true`                                                                                                                                                                                           |
| `--remote <URL>`            | —     | string  | Register this exec-server as a remote environment using the given base URL                                                                                                                                                                                             |
| `--disable <FEATURE>`       | —     | string  | Disable a feature (repeatable). Equivalent to `-c features.&lt;name&gt;=false`                                                                                                                                                                                         |
| `--environment-id <ID>`     | —     | string  | Environment id to attach to when registering remotely                                                                                                                                                                                                                  |
| `--name <NAME>`             | —     | string  | Human-readable environment name                                                                                                                                                                                                                                        |
| `--use-agent-identity-auth` | —     | boolean | Use Agent Identity auth from DBT\_WIZARD\_ACCESS\_TOKEN for remote registration                                                                                                                                                                                        |

Search table...

|                  |   |   |   |   |
| ---------------- | - | - | - | - |
| Loading table... |   |   |   |   |

## features[​](#features "Direct link to features")

Inspect feature flags

| Flag                       | Short | Type   | Description                                                                                                                                                                                                                                                            |
| -------------------------- | ----- | ------ | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `-c, --config <key=value>` | `-c`  | string | Override a configuration value that would otherwise be loaded from `~/.dbt/wizard/config.toml`. Use a dotted path (`foo.bar.baz`) to override nested values. The `value` portion is parsed as TOML. If it fails to parse as TOML, the raw string is used as a literal. |
| `--enable <FEATURE>`       | —     | string | Enable a feature (repeatable). Equivalent to `-c features.&lt;name&gt;=true`                                                                                                                                                                                           |
| `--disable <FEATURE>`      | —     | string | Disable a feature (repeatable). Equivalent to `-c features.&lt;name&gt;=false`                                                                                                                                                                                         |

Search table...

|                  |   |   |   |   |
| ---------------- | - | - | - | - |
| Loading table... |   |   |   |   |

## Was this page helpful?

YesNo

[Privacy policy](https://www.getdbt.com/cloud/privacy-policy)[Create a GitHub issue](https://github.com/dbt-labs/docs.getdbt.com/issues)

This site is protected by reCAPTCHA and the Google [Privacy Policy](https://policies.google.com/privacy) and [Terms of Service](https://policies.google.com/terms) apply.
