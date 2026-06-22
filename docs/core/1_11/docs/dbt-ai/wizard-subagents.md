# Use subagents with dbt Wizard CLI [Beta](https://docs.getdbt.com/docs/dbt-versions/product-lifecycles "Go to https://docs.getdbt.com/docs/dbt-versions/product-lifecycles")

Subagents let dbt Wizard spin up focused, parallel agents to handle parts of a larger task — one to explore your project, one to make changes, one to review them. dbt Wizard orchestrates them and consolidates the results back into your session.

Use subagents when you can split a task into independent pieces of work. They're useful for reviewing a large pull request, debugging a failed job, adding tests across multiple models, or researching documentation while another agent inspects your project.

For smaller, direct tasks, you usually don't need subagents. Ask a single question, like "What does this model do?" or "Fix this failing test," without splitting the work across agents.

Subagents run work in parallel, which uses more tokens than handling the same task in a single-agent session. Use them when the task benefits from dividing the work.

See it in action and share your feedback

Want to see dbt Wizard in action? Check out the [demo video](https://www.youtube.com/watch?v=-lIzh1xQWMA).

We'd love to hear how dbt Wizard is working for you. Share your feedback by either running the `/feedback` slash command in your interactive terminal session or by going to the [#dbt-wizard](https://getdbt.slack.com/archives/C0B6KLW6T26) channel in the [dbt Community Slack](https://docs.getdbt.com/community/join?version=2.0).

Thanks so much for your help in improving dbt Wizard and dbt data development!

## Locations and precedence[​](#locations-and-precedence "Direct link to Locations and precedence")

You can define custom agent roles by adding standalone TOML files at the project level or user level. Use project-level agents for roles that should travel with a repo, and user-level agents for roles you want across all local projects.

The following table summarizes where dbt Wizard looks for custom agents. Use the intended locations for new agents, and keep compatibility locations only when migrating existing setup. Project-level agent config loads only for trusted projects. Within project-level locations, the current working directory wins over parent directories. Project-level agents win over user-level agents with the same name.

| Location                         | Level   | Use for new agents?                                          | Precedence                                                                                            |
| -------------------------------- | ------- | ------------------------------------------------------------ | ----------------------------------------------------------------------------------------------------- |
| `.dbt/wizard/agents/NAME.toml`   | Project | Yes. Use this for agents shared with a repo or subdirectory. | Highest project agent location; closer to the current working directory wins over parent directories. |
| `~/.dbt/wizard/agents/NAME.toml` | User    | Yes. Use this for agents you want across all local projects. | Below project agents, above compatibility imports.                                                    |
| `.claude/agents/NAME.md`         | Project | No. Compatibility import for existing Claude Code agents.    | Below `.dbt/wizard/agents/` in the same project location.                                             |
| `~/.claude/agents/NAME.md`       | User    | No. Compatibility import for existing Claude Code agents.    | Below `~/.dbt/wizard/agents/`.                                                                        |

If two custom agents use the same name, dbt Wizard uses the higher-precedence location and fills in any missing fields from lower-precedence locations when possible. Avoid duplicate names unless you intentionally want to override an existing role.

## Where you can use subagents[​](#where-you-can-use-subagents "Direct link to Where you can use subagents")

Subagents work in the [dbt Wizard CLI](https://docs.getdbt.com/docs/dbt-ai/about-dbt-wizard-cli.md).

You can define custom agent roles, set display nicknames, and configure global limits through the `config.toml` file.

## How subagents work[​](#how-subagents-work "Direct link to How subagents work")

An agent is a role that describes a type of work, like `explorer`, `worker`, or `test_writer`.

A subagent is a running instance of one of those roles. For example, if you start two explorer agents to answer two different questions, you have two subagents that share the same agent role.

dbt Wizard handles orchestration for you. It starts subagents, routes work to them, waits for their results, and consolidates their output into your session.

You can use subagents in the following ways:

| How subagents start                                                                                  | Example                                                                                                                                  |
| ---------------------------------------------------------------------------------------------------- | ---------------------------------------------------------------------------------------------------------------------------------------- |
| You ask to split up the work.                                                                        | `Review PR #123. Use one agent to map what changed, one to check tests and downstream impact, and one to look up the relevant dbt docs.` |
| dbt Wizard automatically uses a configured agent because your task matches that agent's description. | `Add tests for stg_customers and check whether similar staging models are missing tests.`                                                |

Search table...

|                  |   |   |   |   |
| ---------------- | - | - | - | - |
| Loading table... |   |   |   |   |

dbt Wizard then spawns the agents, lets them work in parallel, and brings their findings back into one response.

## Built-in agents[​](#built-in-agents "Direct link to Built-in agents")

dbt Wizard ships with built-in agents that it can spawn without any configuration. Several are purpose-built for dbt work:

| Agent         | What it's for                                                                                                                                                                                         | Example                                                                         |
| ------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------- |
| `explorer`    | Answers specific, well-scoped questions about your project. Fast and read-only — spawn several in parallel for independent questions.                                                                 | `Use explorer to explain what depends on fct_orders.`                           |
| `worker`      | Performs execution and production work, such as implementing part of a feature, fixing tests or bugs, or splitting a large refactor into independent chunks.                                          | `Use worker to update these staging models to follow our naming convention.`    |
| `validation`  | Provides dbt validation. After model edits, runs structured validation — SQL check, `dbt run` with `--defer`, prod vs. dev comparison, and impact analysis — to validate changes before you merge.    | `Use validation to check whether my changes to int_payments are safe to merge.` |
| `test_writer` | Improves dbt test coverage. Analyzes project metadata and warehouse data to find coverage gaps, validates assumptions with queries, and writes `schema.yml` tests for models with low or no coverage. | `Use test_writer to add tests to stg_customers.`                                |

You don't need to declare these — dbt Wizard routes to them automatically when a task fits, or you can ask for one by name. For example, both of these prompts can use the `test_writer` agent:

| Prompt style              | Example                                          |
| ------------------------- | ------------------------------------------------ |
| Ask naturally             | Add useful tests for stg\_customers.             |
| Ask for the agent by name | Use test\_writer to add tests to stg\_customers. |

Search table...

|                  |   |   |   |   |
| ---------------- | - | - | - | - |
| Loading table... |   |   |   |   |

To add your own roles, refer to [Custom agents](#custom-agents-cli).

## Manage subagents[​](#manage-subagents "Direct link to Manage subagents")

Use slash commands inside an interactive session to inspect and steer agent threads:

| Command      | What it does                                                     | Example                                                                                                                  |
| ------------ | ---------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------ |
| `/agent`     | Switch the active agent thread to inspect or steer ongoing work. | Use `/agent` when you want to jump from the main session into a specific agent thread to give it more direction.         |
| `/subagents` | View and switch between running subagent threads.                | Use `/subagents` to see which helper agents are active during a delegated task, then select one to inspect its progress. |

You can also tell dbt Wizard in plain language to steer, stop, or close an agent thread. For example, `Stop the docs researcher agent and continue with the reviewer findings.`

For the full list of session commands, refer to the [slash command reference](https://docs.getdbt.com/docs/dbt-ai/wizard-slash-commands.md).

## Approvals and sandbox[​](#approvals-and-sandbox "Direct link to Approvals and sandbox")

Subagents inherit the parent session's [approval and sandbox policy](https://docs.getdbt.com/docs/dbt-ai/wizard-how-it-works.md#approval-and-sandboxing). Any runtime overrides you set for a turn (permissions, sandbox mode) apply to the subagents it spawns. When a subagent requests approval, your session shows the request with its source so you know which agent is asking.

A custom agent can override sandbox settings for itself — useful when, for example, an exploration agent should stay read-only while a build agent needs workspace write access.

## Custom agents[​](#custom-agents "Direct link to Custom agents")

A custom agent role is a reusable role for a particular type of work you want dbt Wizard to perform, such as writing UDFs, exploring a project, or debugging an issue. You can create any role name that fits your workflow; it does not have to be one of the built-in roles.

Each file defines one custom agent role. The file name must match the agent's `name` field. For example, an agent with `name = "udf_helper"` must be defined in an `agents/udf_helper.toml` file.

dbt Wizard loads each custom agent file as a configuration layer for spawned agent sessions. This means a custom agent can override the same settings as a normal dbt Wizard session config, such as the model, instructions, sandbox mode, and MCP servers.

The best custom agents are narrow and opinionated. Give each one a clear job, the tools it needs for that job, and instructions that keep it from drifting into adjacent work.

Every custom agent file must define:

| Field                    | Required | Description                                                                                                                                                                     |
| ------------------------ | -------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `name`                   | Yes      | Agent name dbt Wizard uses when spawning or referring to this agent. Must match the file name without `.toml`. For example, `name = "udf_helper"` must be in `udf_helper.toml`. |
| `description`            | Yes      | Explains when to use the agent. dbt Wizard reads this description to decide whether the agent fits a task.                                                                      |
| `developer_instructions` | Yes      | Core instructions that define the agent's behavior.                                                                                                                             |
| `nickname_candidates`    | No       | Display-only labels for spawned instances of this agent in the UI, such as `UDF helper` or `UDF queen`. The nickname does not identify the agent.                               |
| `model`                  | No       | Model this agent should use. Inherits from the parent session when omitted.                                                                                                     |
| `sandbox_mode`           | No       | Sandbox mode for this agent. Inherits from the parent session when omitted.                                                                                                     |
| `mcp_servers`            | No       | MCP servers available to this agent. Inherits from the parent session when omitted.                                                                                             |

You can also include other supported [`config.toml`](https://docs.getdbt.com/docs/dbt-ai/wizard-config.md#configtoml) keys in a custom agent file. Any setting you don't define in the custom agent file inherits from the parent session.

### Example custom agent[​](#example-custom-agent "Direct link to Example custom agent")

Create a standalone custom agent file. The file name and `name` value must match:

\~/.dbt/wizard/agents/udf\_helper.toml

```toml
name = "udf_helper" # matches the udf_helper.toml file name
description = "Helps design and implement dbt UDFs and models that use them."

developer_instructions = """
You are an expert in dbt UDFs.

Focus on:
- choosing the right UDF pattern
- writing clear function definitions
- configuring function paths
- using {{ function() }} references correctly
- adding focused models and tests that demonstrate the UDF
- keeping changes small and additive

Before editing files, explain the proposed UDF design.
When editing files, avoid unrelated changes.
"""

sandbox_mode = "workspace-write"
nickname_candidates = ["UDF helper", "UDF queen"]
```

Then ask dbt Wizard to use the agent by name:

```text
Use udf_helper to create a UDF model that checks whether customer IDs are positive integers.
```

You can also let dbt Wizard choose the agent automatically by describing the task:

```text
Create a dbt UDF that checks whether customer IDs are positive integers, then add a small model that uses it.
```

### Display nicknames[​](#display-nicknames "Direct link to Display nicknames")

When several instances of the same agent run at once, use `nickname_candidates` to give each instance a readable label in the UI. For example, two spawned `udf_helper` agents might appear as `UDF helper` and `UDF queen`.

Nicknames are display-only. dbt Wizard identifies the agent by its `name` field, not by the nickname shown in the UI.

### Example with an MCP server[​](#example-with-an-mcp-server "Direct link to Example with an MCP server")

Custom agent files can include MCP server configuration when the agent needs additional tools or context.

Again, the file name and `name` value must match:

\~/.dbt/wizard/agents/dbt\_docs\_helper.toml

```toml
name = "dbt_docs_helper"
description = "Looks up relevant dbt documentation for patterns referenced in a change."

developer_instructions = """
Use the dbt docs MCP server to confirm APIs, options, and version-specific behavior.
Return concise answers with links or exact references when available.
Do not edit files.
"""

sandbox_mode = "read-only"

[mcp_servers.dbt]
command = "uvx"
args = ["dbt-mcp"]
```

This setup works well for prompts like:

```text
Have udf_helper design the UDF change and dbt_docs_helper verify the dbt function configuration.
```

### Global settings[​](#global-settings "Direct link to Global settings")

Global subagent settings live under the `[agents]` section in `~/.dbt/wizard/config.toml`:

\~/.dbt/wizard/config.toml

```toml
[agents]
max_depth = 2
job_max_runtime_seconds = 1800
interrupt_message = true
```

| Key                       | Description                                                                                      |
| ------------------------- | ------------------------------------------------------------------------------------------------ |
| `max_depth`               | Maximum nesting depth for spawned agent threads. Root sessions start at depth 0.                 |
| `job_max_runtime_seconds` | Default maximum runtime, in seconds, for agent job workers.                                      |
| `interrupt_message`       | Whether to record a model-visible message when an agent turn is interrupted. Defaults to `true`. |

## Examples[​](#examples "Direct link to Examples")

### UDF implementation[​](#udf-implementation "Direct link to UDF implementation")

Define a custom agent that specializes in dbt UDF work. Each custom agent is a standalone TOML file under `~/.dbt/wizard/agents/`.

\~/.dbt/wizard/agents/udf\_helper.toml

```toml
name = "udf_helper"
description = "Helps design and implement dbt UDFs and models that use them."

developer_instructions = """
You are an expert in dbt UDFs.

Focus on:
- choosing the right UDF pattern
- writing clear function definitions
- configuring function paths
- using {{ function() }} references correctly
- adding focused models and tests that demonstrate the UDF
- keeping changes small and additive

Before editing files, explain the proposed UDF design.
When editing files, avoid unrelated changes.
"""

sandbox_mode = "workspace-write"
nickname_candidates = ["UDF helper", "UDF queen"]
```

Optionally, configure global subagent settings in your main `config.toml`:

\~/.dbt/wizard/config.toml

```toml
[agents]
max_depth = 2
```

Then prompt dbt Wizard:

```text
Use udf_helper to create a UDF that checks whether customer IDs are positive integers, then add a small model that uses it.
```

dbt Wizard starts the requested agent, lets it work on the UDF implementation, and consolidates the result into your session.

### Debug a failed job run (home app)[​](#debug-a-failed-job-run-home-app "Direct link to Debug a failed job run (home app)")

In the dbt platform home app, ask dbt Wizard to investigate a failed job by delegating to focused agents:

```text
The nightly job failed. Use one agent to pull the run error and logs,
one to trace the failing model's lineage and find the root cause, and
one to propose a fix. Summarize what each found.
```

dbt Wizard spawns the agents, each one works on its part of your connected project, and dbt Wizard consolidates the diagnosis and proposed fix into one response.

## Related docs[​](#related-docs "Direct link to Related docs")

* [Use MCP servers with the dbt Wizard CLI](https://docs.getdbt.com/docs/dbt-ai/wizard-mcp.md) to give agents more tools and context
* [Use skills](https://docs.getdbt.com/docs/dbt-ai/wizard-skills.md) for reusable, project-specific instructions
* [dbt Wizard slash commands](https://docs.getdbt.com/docs/dbt-ai/wizard-slash-commands.md) for `/agent` and `/subagents`
* [dbt Wizard CLI config](https://docs.getdbt.com/docs/dbt-ai/wizard-config.md) for `config.toml` keys and precedence
* [How dbt Wizard works](https://docs.getdbt.com/docs/dbt-ai/wizard-how-it-works.md) for approvals and sandboxing
