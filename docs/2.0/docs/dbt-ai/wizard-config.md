# dbt Wizard config reference [Beta](https://docs.getdbt.com/docs/dbt-versions/product-lifecycles "Go to https://docs.getdbt.com/docs/dbt-versions/product-lifecycles")

dbt Wizard stores configuration in two TOML files under `~/.dbt/wizard/`, each controlling a different part of the product.

## The two config files[​](#the-two-config-files "Direct link to The two config files")

See it in action and share your feedback

Want to see dbt Wizard in action? Check out the [demo video](https://www.youtube.com/watch?v=-lIzh1xQWMA).

We'd love to hear how dbt Wizard is working for you. Share your feedback by either running the `/feedback` slash command in your interactive terminal session or by going to the [#dbt-wizard](https://getdbt.slack.com/archives/C0B6KLW6T26) channel in the [dbt Community Slack](https://docs.getdbt.com/community/join?version=2.0).

Thanks so much for your help in improving dbt Wizard and dbt data development!

TOML (Tom's Obvious, Minimal Language) is a config file format. If you've written YAML or JSON before, it'll feel familiar — keys and values are separated by `=` (with spaces before and after the equals sign), and nested sections use `[section.name]` headers instead of indentation or curly braces. For example:

\~/.dbt/wizard/config.toml

```toml
model = "claude-sonnet-4-6"

[projects."/Users/you/jaffle-shop"]
trust_level = "trusted"
```

This config sets the default AI model for all new sessions and applies to all future invocations across all projects. You can override the default AI model for a specific project by setting the `model` key in the `wizard_config.toml` file for that project or using the `/model` picker in the text-based user interface (TUI).

| File                               | Controls                                                                     | When to use                                             |
| ---------------------------------- | ---------------------------------------------------------------------------- | ------------------------------------------------------- |
| `~/.dbt/wizard/config.toml`        | Agent runtime — AI model, MCP servers, approval rules, trusted projects      | Change agent-level behavior across all sessions         |
| `~/.dbt/wizard/wizard_config.toml` | Per-project settings — dbt executable path, deferral, saved model preference | Change how dbt Wizard works with a specific dbt project |

Note that `dbt_project.yml` is separate from both and controls how dbt builds your project.

## Common tasks[​](#common-tasks "Direct link to Common tasks")

| Goal                                      | Edit                                                               | Restart needed? |
| ----------------------------------------- | ------------------------------------------------------------------ | --------------- |
| Change the default AI model               | `config.toml` → `model = "claude-sonnet-4-6"`                      | Yes             |
| Point dbt Wizard at a specific dbt binary | `wizard_config.toml` → `path`                                      | Yes             |
| Add the dbt MCP server                    | `config.toml` → `[mcp_servers.dbt]`                                | Yes             |
| Mark a repo as trusted                    | `config.toml` → `trust_level = "trusted"` under `[projects."..."]` | Yes             |
| Change approval or sandbox defaults       | `config.toml`                                                      | Yes             |

## Config precedence[​](#config-precedence "Direct link to Config precedence")

Settings resolve in this order (highest to lowest):

1. CLI flags (`-m`, `-c`) at invocation
2. In-session model picker (`/model`)
3. `~/.dbt/wizard/config.toml`
4. `~/.dbt/wizard/wizard_config.toml`
5. Built-in defaults

## config.toml[​](#configtoml "Direct link to config.toml")

Global agent settings for models, approvals, sandboxing, web search, and MCP servers.

`~/.dbt/wizard/config.toml` controls how the agent session behaves. It's created automatically when dbt Wizard is installed.

### Configuration keys[​](#configuration-keys "Direct link to Configuration keys")

This is the list of configuration keys that can be set in `config.toml`.

* `model`
* `approval_policy`
* `sandbox_mode`
* `web_search`

You can view more by running `wizard config --help`.

#### AI model and API[​](#ai-model-and-api "Direct link to AI model and API")

| Key     | Type   | Default | Description                                                                                                                                 |
| ------- | ------ | ------- | ------------------------------------------------------------------------------------------------------------------------------------------- |
| `model` | string | —       | Default AI model for new sessions. Refer to [AI model ID format](#ai-model-id-format) below. Change interactively with `/model` in the TUI. |

#### Behavior[​](#behavior "Direct link to Behavior")

| Key               | Type   | Default      | Description                                                                                                                                                       |
| ----------------- | ------ | ------------ | ----------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `approval_policy` | string | `on-request` | When Wizard asks before executing commands. Options: `untrusted` (ask for non-trusted commands), `on-request` (dbt Wizard decides), `never` (run without asking). |
| `sandbox_mode`    | string | `read-only`  | Shell sandbox policy. Options: `read-only` (default), `workspace-write` (allow writes inside project dir), `danger-full-access` (no restrictions).                |
| `web_search`      | string | `cached`     | Web search mode. Options: `disabled`, `cached`, or `live`.                                                                                                        |

#### Project trust[​](#project-trust "Direct link to Project trust")

Set `trust_level` per-project to allow dbt Wizard to use project-local config:

```toml
[projects."/absolute/path/to/your-repo"]
trust_level = "trusted"
```

#### MCP servers[​](#mcp-servers "Direct link to MCP servers")

```toml
[mcp_servers.dbt]
command = "DBT_MCP_ENDPOINT"
```

### AI model ID format[​](#ai-model-id-format "Direct link to AI model ID format")

AI model IDs in `config.toml` use the public model ID, such as `claude-sonnet-4-6`.

To list all available AI model IDs:

```bash
wizard debug models
```

### Environment variables[​](#environment-variables "Direct link to Environment variables")

Any key can be set as an environment variable using the `DBT_WIZARD_` prefix in `SCREAMING_SNAKE_CASE`:

```bash
export DBT_WIZARD_MODEL=claude-sonnet-4-6
export DBT_WIZARD_APPROVAL_POLICY=never
export OPENAI_API_KEY=sk-...        # OpenAI (no prefix needed)
export ANTHROPIC_API_KEY=sk-ant-... # Anthropic (no prefix needed)
```

`OPENAI_API_KEY` and `ANTHROPIC_API_KEY` are read directly without the `DBT_WIZARD_` prefix, following each provider's convention.

### When it's read[​](#when-its-read "Direct link to When it's read")

`config.toml` is read at session start when you run `wizard`. Changes take effect only after you exit and restart the CLI. Per-invocation `-c` flags and the in-session `/model` picker override this file for that session only.

### Example[​](#example "Direct link to Example")

```toml
model = "claude-sonnet-4-6"

[projects."/Users/you/jaffle-shop"]
trust_level = "trusted"

[mcp_servers.dbt]
command = "DBT_MCP_ENDPOINT"
```

## wizard\_config.toml[​](#wizard_configtoml "Direct link to wizard_config.toml")

Per-project settings Wizard writes during onboarding, including dbt path and deferral settings.

`~/.dbt/wizard/wizard_config.toml` stores what dbt Wizard knows about each of your dbt projects. dbt Wizard writes to this file automatically during onboarding and when you change project settings. You can also edit it manually.

### Configuration keys[​](#configuration-keys-1 "Direct link to Configuration keys")

| Key                               | Description                                                                                                                                                                                                                                                                                                                                                                                               |
| --------------------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `version`                         | Schema version for this file (currently `1`). Don't change this manually.                                                                                                                                                                                                                                                                                                                                 |
| `global.terms_of_use_accepted_at` | Timestamp of when you accepted the dbt Wizard Terms of Use. Written once under the `[global]` section and applies across all projects.                                                                                                                                                                                                                                                                    |
| `onboarded_at`                    | Timestamp of when you onboarded this project in dbt Wizard.                                                                                                                                                                                                                                                                                                                                               |
| `path`                            | Path to the dbt binary dbt Wizard should use for this project.                                                                                                                                                                                                                                                                                                                                            |
| `deferral.mode`                   | Who handles [deferral](https://docs.getdbt.com/reference/node-selection/defer.md) for this project. `"wizard"` means dbt Wizard handles it — you tell it which `profiles.yml` target to defer to, and it reuses that target's models instead of rebuilding everything. `"fusion_cloud"` means the dbt platform handles deferral against your connected environment. Refer to [Deferral](#deferral) below. |
| `deferral.target`                 | The [target](https://docs.getdbt.com/docs/local/profiles.yml.md) from your `profiles.yml` that dbt Wizard compiles and defers to when `deferral.mode` is `"wizard"` (for example, `"dev"`).                                                                                                                                                                                                               |

Settings are stored per-project under `[projects."/absolute/path/to/repo"]` blocks:

\~/.dbt/wizard/wizard\_config.toml

```toml
version = 1

[global]
terms_of_use_accepted_at = "2026-05-28T15:06:54Z"

[projects."/Users/you/jaffle-shop"]
onboarded_at = "2026-05-20T11:09:01.410346"
path = "/Users/you/jaffle-shop/.venv/bin/dbt"

[projects."/Users/you/jaffle-shop".deferral]
mode = "wizard"
target = "dev"
```

#### Deferral[​](#deferral "Direct link to Deferral")

[Deferral](https://docs.getdbt.com/reference/node-selection/defer.md) lets dbt Wizard reuse models that are already built elsewhere (for example, in production) instead of rebuilding everything when you're only working on part of a project, saving you time and warehouse cost.

The `deferral.mode` setting in `wizard_config.toml` controls who handles deferral. It accepts five values, and dbt Wizard usually sets it for you during onboarding:

| `deferral.mode` value | What it means                                                                                                                                                                                                                                                                                                                                             |
| --------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `"wizard"`            | dbt Wizard handles deferral for you. You tell dbt Wizard which [target](https://docs.getdbt.com/docs/local/profiles.yml.md) from your `profiles.yml` to defer to (it tries to detect one automatically when you first set up the project). dbt Wizard then compiles that target and reuses its models for any upstream models you haven't built yourself. |
| `"fusion_cloud"`      | The dbt platform handles deferral against your connected environment, so dbt Wizard doesn't manage any local state. Set automatically when you're connected to the platform.                                                                                                                                                                              |
| `"dbt-state"`         | dbt state handles deferral, so dbt Wizard skips its own production compile.                                                                                                                                                                                                                                                                               |
| `"manual"`            | You maintain the deferral manifest path manually.                                                                                                                                                                                                                                                                                                         |
| `"disabled"`          | Deferral is disabled for the project.                                                                                                                                                                                                                                                                                                                     |

For more about dbt State, refer to [About dbt State](https://docs.getdbt.com/docs/deploy/dbt-state-about.md).

**About [favor-state](https://docs.getdbt.com/reference/node-selection/defer.md?version=2.0#favor-state):** favor-state is a built-in dbt deferral option, not something you need to configure here — dbt Wizard handles it for you during deferral compiles. You don't need to add `favor_state` to `wizard_config.toml`. What matters instead is that dbt Wizard uses a valid deferral mode and state path.

In practice, when dbt Wizard handles deferral it uses your own dev version of a model when you've already built one, and only falls back to the version from the deferred target for models you haven't built yet.

For how this behaves during a session, refer to [Deferral and state](https://docs.getdbt.com/docs/dbt-ai/wizard-how-it-works.md#deferral-and-state).

## Troubleshooting[​](#troubleshooting "Direct link to Troubleshooting")

Here are some common config mistakes and how to fix them:

| Symptom                                                  | Likely cause                                 | Fix                                                 |
| -------------------------------------------------------- | -------------------------------------------- | --------------------------------------------------- |
| Still on old AI model after editing `wizard_config.toml` | `wizard_config.toml` doesn't store the model | Set `model` in `config.toml` and restart `wizard`   |
| AI model unchanged after editing `config.toml`           | Old session still running                    | Press Ctrl+C, then run `wizard` again               |
| "Invalid model" error                                    | Wrong AI model ID                            | Run `wizard debug models` to see valid AI model IDs |

### Re-trigger onboarding flows[​](#re-trigger-onboarding-flows "Direct link to Re-trigger onboarding flows")

If you need to re-run part of the setup — for example, after deleting config entries by mistake or switching to a new project — you can reset individual flows by deleting the relevant file. dbt Wizard will re-prompt you the next time it runs.

To delete a file, run the command in your terminal (macOS/Linux). For example:

```bash
rm ~/.dbt/wizard/auth.json
```

If you get a `No such file or directory` error, the file doesn't exist — which means that part of the setup hasn't run yet, or has already been reset. You can ignore the error.

| What you want to redo             | File to delete                                                                                                                                  |
| --------------------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------- |
| Re-run the trusted folder prompt  | Remove your project's entry from `~/.dbt/wizard/config.toml` under `[projects."..."]`                                                           |
| Re-authenticate with dbt platform | `~/.dbt/wizard/auth.json`                                                                                                                       |
| Re-run dbt project setup          | Remove your project's entry from `~/.dbt/wizard/wizard_config.toml` under `[projects."..."]`, or delete the file entirely to reset all projects |
| Re-trigger BYOK key configuration | `~/.dbt/wizard/provider*` (this matches all provider files)                                                                                     |

caution

`config.toml` and `wizard_config.toml` are separate files that control different things. If you want to re-run project setup, edit `wizard_config.toml` — not `config.toml`. Editing the wrong file won't re-run the onboarding flow you expect. Refer to [The two config files](#the-two-config-files) at the top of this page for the difference.

## Related docs[​](#related-docs "Direct link to Related docs")

Links to BYOK setup, approval behavior, and CLI command reference.

* [Configure BYOK](https://docs.getdbt.com/docs/dbt-ai/wizard-byok.md)
* [Approval and sandboxing](https://docs.getdbt.com/docs/dbt-ai/wizard-how-it-works.md#approval-and-sandboxing)
* [dbt Wizard command reference](https://docs.getdbt.com/docs/dbt-ai/wizard-cli-reference.md)
* [How dbt Wizard works](https://docs.getdbt.com/docs/dbt-ai/wizard-how-it-works.md)

## Was this page helpful?

YesNo

[Privacy policy](https://www.getdbt.com/cloud/privacy-policy)[Create a GitHub issue](https://github.com/dbt-labs/docs.getdbt.com/issues)

This site is protected by reCAPTCHA and the Google [Privacy Policy](https://policies.google.com/privacy) and [Terms of Service](https://policies.google.com/terms) apply.
