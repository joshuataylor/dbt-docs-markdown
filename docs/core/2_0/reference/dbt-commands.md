# dbt Command reference

You can run dbt using the following tools:

* In your browser with the [Studio IDE](https://docs.getdbt.com/docs/platform/studio-ide/develop-in-studio.md)
* On the command line interface using the [dbt CLI](https://docs.getdbt.com/docs/platform/dbt-cli-installation.md) or open-source [dbt Core](https://docs.getdbt.com/docs/local/install-dbt.md).

A key distinction is that dbt CLI and Studio IDE are designed to support safe parallel execution of dbt commands, leveraging dbt platform's infrastructure and its comprehensive [features](https://docs.getdbt.com/docs/platform/about-platform/dbt-platform-features.md). In contrast, dbt Core *doesn't support* safe parallel execution for multiple invocations in the same process. Learn more in the [parallel execution](#parallel-execution) section.

## Parallel execution[​](#parallel-execution "Direct link to Parallel execution")

dbt platform allows for concurrent execution of commands, enhancing efficiency without compromising data integrity. This enables you to run multiple commands at the same time. However, it's important to understand which commands can be run in parallel and which can't.

In contrast, [`dbt-core` *doesn't* support](https://docs.getdbt.com/reference/programmatic-invocations.md#parallel-execution-not-supported) safe parallel execution for multiple invocations in the same process, and requires users to manage concurrency manually to ensure data integrity and system stability.

To ensure your dbt workflows are both efficient and safe, you can run different types of dbt commands in parallel — for example, `dbt build` (write operation) can safely run alongside `dbt parse` (read operation). However, you can't run `dbt build` and `dbt run` (both write operations) in parallel.

dbt commands can be `read` or `write` commands:

| Command type | Description                                                                                                                                                                                                                                                                                                                | Example                        |
| ------------ | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ------------------------------ |
| **Write**    | These commands perform actions that change data or metadata in your data platform.<br /><br />Limited to one invocation at any given time, which prevents any potential conflicts, such as overwriting the same table in your data platform at the same time.                                                              | `dbt build`<br />`dbt run`     |
| **Read**     | These commands involve operations that fetch or read data without making any changes to your data platform.<br /><br />Can have multiple invocations in parallel and aren't limited to one invocation at any given time. This means read commands can run in parallel with other read commands and a single write command. | `dbt parse`<br />`dbt compile` |

Search table...

|                  |   |   |   |   |
| ---------------- | - | - | - | - |
| Loading table... |   |   |   |   |

## Available commands[​](#available-commands "Direct link to Available commands")

The following sections outline the commands supported by dbt and their relevant flags. They are available in all tools and all [supported versions](https://docs.getdbt.com/docs/dbt-versions.md) unless noted otherwise. You can run these commands in your specific tool by prefixing them with `dbt` — for example, to run the `test` command, type `dbt test`.

You can also call these commands and flags programmatically using `dbtRunner.invoke`. For details, refer to [Programmatic invocations](https://docs.getdbt.com/reference/programmatic-invocations.md).

For information about selecting models on the command line, refer to [Model selection syntax](https://docs.getdbt.com/reference/node-selection/syntax.md).

Commands marked ❌ indicate write commands, commands marked ✅ indicate read commands, and commands marked N/A indicate that parallel execution isn't relevant for that command.

info

Some commands are not yet supported in the dbt Fusion engine or have limited functionality. Refer to the [Fusion supported features](https://docs.getdbt.com/docs/fusion/supported-features.md) page for details.

| Command                                                                      | Description                                                                                 | Parallel execution | Caveats                                                                                                                                 |
| ---------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------- | ------------------ | --------------------------------------------------------------------------------------------------------------------------------------- |
| [build](https://docs.getdbt.com/reference/commands/build.md)                 | Builds and tests all selected resources (models, seeds, tests, and more)                    | ❌                 | All tools<br />All [supported versions](https://docs.getdbt.com/docs/dbt-versions.md)                                                   |
| cancel                                                                       | Cancels the most recent invocation.                                                         | N/A                | dbt CLI<br />Requires [dbt v1.6 or higher](https://docs.getdbt.com/docs/dbt-versions.md)                                                |
| [clean](https://docs.getdbt.com/reference/commands/clean.md)                 | Deletes artifacts present in the dbt project                                                | ✅                 | All tools<br />All [supported versions](https://docs.getdbt.com/docs/dbt-versions.md)                                                   |
| [clone](https://docs.getdbt.com/reference/commands/clone.md)                 | Clones selected models from the specified state                                             | ❌                 | All tools<br />Requires [dbt v1.6 or higher](https://docs.getdbt.com/docs/dbt-versions.md)                                              |
| [compile](https://docs.getdbt.com/reference/commands/compile.md)             | Compiles (but does not run) the models in a project                                         | ✅                 | All tools<br />All [supported versions](https://docs.getdbt.com/docs/dbt-versions.md)                                                   |
| [debug](https://docs.getdbt.com/reference/commands/debug.md)                 | Debugs dbt connections and projects                                                         | ✅                 | All tools<br />All [supported versions](https://docs.getdbt.com/docs/dbt-versions.md)                                                   |
| [deps](https://docs.getdbt.com/reference/commands/deps.md)                   | Downloads dependencies for a project                                                        | ✅                 | All tools<br />All [supported versions](https://docs.getdbt.com/docs/dbt-versions.md)                                                   |
| [docs](https://docs.getdbt.com/reference/commands/cmd-docs.md)               | Generates documentation for a project                                                       | ✅                 | All tools<br />All [supported versions](https://docs.getdbt.com/docs/dbt-versions.md)                                                   |
| [environment](https://docs.getdbt.com/reference/commands/dbt-environment.md) | Enables you to interact with your dbt platform environment.                                 | N/A                | dbt CLI<br />Requires [dbt v1.5 or higher](https://docs.getdbt.com/docs/dbt-versions.md)                                                |
| help                                                                         | Displays help information for any command                                                   | N/A                | dbt Core, dbt CLI<br />All [supported versions](https://docs.getdbt.com/docs/dbt-versions.md)                                           |
| [init](https://docs.getdbt.com/reference/commands/init.md)                   | Initializes a new dbt project                                                               | ✅                 | Fusion<br />dbt Core<br />All [supported versions](https://docs.getdbt.com/docs/dbt-versions.md)                                        |
| [invocation](https://docs.getdbt.com/reference/commands/invocation.md)       | Enables users to debug long-running sessions by interacting with active invocations.        | N/A                | dbt CLI<br />Requires [dbt v1.5 or higher](https://docs.getdbt.com/docs/dbt-versions.md)                                                |
| [lint](https://docs.getdbt.com/reference/commands/lint.md)                   | Lints SQL files in a project for style, correctness, and convention violations              | ✅                 | Requires dbt platform project on Fusion                                                                                                 |
| [list](https://docs.getdbt.com/reference/commands/list.md)                   | Lists resources defined in a dbt project                                                    | ✅                 | All tools<br />All [supported versions](https://docs.getdbt.com/docs/dbt-versions.md)                                                   |
| [login](https://docs.getdbt.com/reference/commands/login.md)                 | Logs in to your dbt platform account                                                        | N/A                | dbt Core v2.0 and later<br />dbt CLI                                                                                                    |
| [parse](https://docs.getdbt.com/reference/commands/parse.md)                 | Parses a project and writes detailed timing info                                            | ✅                 | All tools<br />All [supported versions](https://docs.getdbt.com/docs/dbt-versions.md)                                                   |
| reattach                                                                     | Reattaches to the most recent invocation to retrieve logs and artifacts.                    | N/A                | dbt CLI<br />Requires [dbt v1.6 or higher](https://docs.getdbt.com/docs/dbt-versions.md)                                                |
| [retry](https://docs.getdbt.com/reference/commands/retry.md)                 | Retry the last run `dbt` command from the point of failure                                  | ✅                 | All tools<br />Requires [dbt v1.6 or higher](https://docs.getdbt.com/docs/dbt-versions.md)                                              |
| [run](https://docs.getdbt.com/reference/commands/run.md)                     | Runs the models in a project                                                                | ❌                 | All tools<br />All [supported versions](https://docs.getdbt.com/docs/dbt-versions.md)                                                   |
| [run-operation](https://docs.getdbt.com/reference/commands/run-operation.md) | Invokes a macro, including running arbitrary maintenance SQL against the database           | ❌                 | All tools<br />All [supported versions](https://docs.getdbt.com/docs/dbt-versions.md)                                                   |
| [seed](https://docs.getdbt.com/reference/commands/seed.md)                   | Loads CSV files into the database                                                           | ❌                 | All tools<br />All [supported versions](https://docs.getdbt.com/docs/dbt-versions.md)                                                   |
| [show](https://docs.getdbt.com/reference/commands/show.md)                   | Previews table rows post-transformation                                                     | ✅                 | All tools<br />All [supported versions](https://docs.getdbt.com/docs/dbt-versions.md)                                                   |
| [snapshot](https://docs.getdbt.com/reference/commands/snapshot.md)           | Executes "snapshot" jobs defined in a project                                               | ❌                 | All tools<br />All [supported versions](https://docs.getdbt.com/docs/dbt-versions.md)                                                   |
| [source](https://docs.getdbt.com/reference/commands/source.md)               | Provides tools for working with source data (including validating that sources are "fresh") | ✅                 | All tools<br />All [supported versions](https://docs.getdbt.com/docs/dbt-versions.md)                                                   |
| [test](https://docs.getdbt.com/reference/commands/test.md)                   | Executes tests defined in a project                                                         | ✅                 | All tools<br />All [supported versions](https://docs.getdbt.com/docs/dbt-versions.md)<br />Fusion flag `--warn-error` not yet supported |

Search table...

|                  |   |   |   |   |
| ---------------- | - | - | - | - |
| Loading table... |   |   |   |   |

Note, use the [`--version`](https://docs.getdbt.com/reference/commands/version.md) flag to display the installed dbt Core or dbt CLI version. (Not applicable for the Studio IDE). Available on all [supported versions](https://docs.getdbt.com/docs/dbt-versions.md).
