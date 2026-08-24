*
* [Commands](../reference/dbt-commands.md)
* List of commands

# List of commands

The list of commands available in dbt.

## [build](../reference/commands/build.md)

[The dbt build command will:](../reference/commands/build.md)

## [clean](../reference/commands/clean.md)

[dbt clean is a utility function that deletes the paths specified within the clean-targets list in the dbt\_project.yml file. It helps by removing unnecessary files or directories generated during the execution of other dbt commands, ensuring a clean state for the project.](../reference/commands/clean.md)

## [clone](../reference/commands/clone.md)

[The dbt clone command clones selected nodes from the specified state to the target schema(s). This command makes use of the clone materialization:](../reference/commands/clone.md)

## [docs](../reference/commands/cmd-docs.md)

[Generate and serve the docs for your dbt project.](../reference/commands/cmd-docs.md)

## [compile](../reference/commands/compile.md)

[The dbt compile command creates executable SQL from models, data tests, analyses, functions, and snapshots.](../reference/commands/compile.md)

## [debug](../reference/commands/debug.md)

[Use dbt debug to test database connections and check system setup.](../reference/commands/debug.md)

## [deps](../reference/commands/deps.md)

[dbt deps pulls the most recent version of the dependencies listed in your packages.yml from git. See Package-Management for more information.](../reference/commands/deps.md)

## [init](../reference/commands/init.md)

[dbt init helps get you started using !](../reference/commands/init.md)

## [ls (list)](../reference/commands/list.md)

[Read this guide on how dbt's ls (list) command can be used to list resources in your dbt project.](../reference/commands/list.md)

## [parse](../reference/commands/parse.md)

[Read this guide on how dbt's parse command can be used to parse your dbt project and write detailed timing information.](../reference/commands/parse.md)

## [retry](../reference/commands/retry.md)

[Retry re-executes the last invocation from the point of failure.](../reference/commands/retry.md)

## [rpc](../reference/commands/rpc.md)

[Remote Procedure Call (rpc) dbt server compiles and runs queries, and provides methods that enable you to list and terminate running processes.](../reference/commands/rpc.md)

## [run](../reference/commands/run.md)

[The dbt run command executes your compiled SQL models against a target database.](../reference/commands/run.md)

## [run-operation](../reference/commands/run-operation.md)

[Read this guide on how dbt's run-operation command can be used to invoke a macro or execute SQL directly.](../reference/commands/run-operation.md)

## [seed](../reference/commands/seed.md)

[The dbt seed command loads static CSV files from your project’s seed-paths into your as tables. Use seeds for small, version-controlled reference datasets you want to keep alongside your project, such as country codes, region mappings, or a list of business-defined categories.](../reference/commands/seed.md)

## [show](../reference/commands/show.md)

[Use dbt show to:](../reference/commands/show.md)

## [snapshot](../reference/commands/snapshot.md)

[The dbt snapshot command executes the Snapshots defined in your project. Snapshots record changes to your source data over time by implementing type-2 Slowly Changing Dimensions. Run dbt snapshot on a schedule (for example, daily) to capture changes in your source tables.](../reference/commands/snapshot.md)

## [source](../reference/commands/source.md)

[The dbt source command provides subcommands that are useful when working with source data. This command provides one subcommand, dbt source freshness.](../reference/commands/source.md)

## [test](../reference/commands/test.md)

[dbt test runs data tests defined on models, sources, snapshots, and seeds and unit tests defined on SQL models. It expects that you have already created those resources through the appropriate commands.](../reference/commands/test.md)

## [version](../reference/commands/version.md)

[The --version command-line flag returns information about the currently installed version of , the , or the . This flag is not supported when invoking dbt in other runtimes (for example, the IDE or scheduled runs).](../reference/commands/version.md)

[Previous](../reference/dbt-commands.md)

[dbt Command reference](../reference/dbt-commands.md)

[Next](../reference/commands/build.md)

[build](../reference/commands/build.md)
