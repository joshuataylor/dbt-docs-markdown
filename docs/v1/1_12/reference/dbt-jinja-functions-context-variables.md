*
* [Jinja reference](../category/jinja-reference.md)
* dbt Jinja context functions

# dbt Jinja functions and context variables

In addition to the standard Jinja library, we've added additional functions and variables to the Jinja context that are useful when working with a dbt project. Note that dependencies.yml is not Jinja rendered and therefore does not support Jinja.

## [adapter](./dbt-jinja-functions/adapter.md)

[Wrap the internal database adapter with the Jinja object \`adapter\`.](./dbt-jinja-functions/adapter.md)

## [as\_bool](./dbt-jinja-functions/as_bool.md)

[Use this filter to coerce a Jinja output into boolean value.](./dbt-jinja-functions/as_bool.md)

## [as\_native](./dbt-jinja-functions/as_native.md)

[Use this filter to coerce Jinja-compiled output into its native python.](./dbt-jinja-functions/as_native.md)

## [as\_number](./dbt-jinja-functions/as_number.md)

[Use this filter to convert Jinja-compiled output to a numeric value..](./dbt-jinja-functions/as_number.md)

## [builtins](./dbt-jinja-functions/builtins.md)

[Read this guide to understand the builtins Jinja variable in dbt.](./dbt-jinja-functions/builtins.md)

## [config](./dbt-jinja-functions/config.md)

[Read this guide to understand the config Jinja function in dbt.](./dbt-jinja-functions/config.md)

## [cross-database macros](./dbt-jinja-functions/cross-database-macros.md)

[Read this guide to understand cross-database macros in dbt.](./dbt-jinja-functions/cross-database-macros.md)

## [dbt\_project.yml context](./dbt-jinja-functions/dbt-project-yml-context.md)

[The context methods and variables available when configuring resources in the dbt\_project.yml file.](./dbt-jinja-functions/dbt-project-yml-context.md)

## [dbt\_version](./dbt-jinja-functions/dbt_version.md)

[Read this guide to understand the dbt\_version Jinja function in dbt.](./dbt-jinja-functions/dbt_version.md)

## [debug](./dbt-jinja-functions/debug-method.md)

[The \`{{ debug() }}\` macro will open an iPython debugger.](./dbt-jinja-functions/debug-method.md)

## [dispatch](./dbt-jinja-functions/dispatch.md)

[dbt extends functionality across data platforms using multiple dispatch.](./dbt-jinja-functions/dispatch.md)

## [doc](./dbt-jinja-functions/doc.md)

[Use \`doc()\` in description fields to reference docs blocks.](./dbt-jinja-functions/doc.md)

## [env\_var](./dbt-jinja-functions/env_var.md)

[Incorporate environment variables using \`env\_var\` function.](./dbt-jinja-functions/env_var.md)

## [exceptions](./dbt-jinja-functions/exceptions.md)

[Raise warnings and errors with the \`exceptions\` namespace.](./dbt-jinja-functions/exceptions.md)

## [execute](./dbt-jinja-functions/execute.md)

[Use \`execute\` to return True when dbt is in 'execute' mode.](./dbt-jinja-functions/execute.md)

## [flags](./dbt-jinja-functions/flags.md)

[The \`flags\` variable contains values of flags provided on the cli.](./dbt-jinja-functions/flags.md)

## [fromjson](./dbt-jinja-functions/fromjson.md)

[Deserialize a JSON string into python with \`fromjson\` context method.](./dbt-jinja-functions/fromjson.md)

## [fromyaml](./dbt-jinja-functions/fromyaml.md)

[Deserialize a YAML string into python with \`fromyaml\` context method.](./dbt-jinja-functions/fromyaml.md)

## [graph](./dbt-jinja-functions/graph.md)

[The \`graph\` context variable contains info about nodes in your project.](./dbt-jinja-functions/graph.md)

## [invocation\_id](./dbt-jinja-functions/invocation_id.md)

[The \`invocation\_id\` outputs a UUID generated for this dbt command.](./dbt-jinja-functions/invocation_id.md)

## [local\_md5](./dbt-jinja-functions/local_md5.md)

[Calculate an MD5 hash of a string with \`local\_md5\` context variable.](./dbt-jinja-functions/local_md5.md)

## [log](./dbt-jinja-functions/log.md)

[Learn more about the log Jinja function in dbt.](./dbt-jinja-functions/log.md)

## [model](./dbt-jinja-functions/model.md)

[\`model\` is the dbt graph object (or node) for the current model.](./dbt-jinja-functions/model.md)

## [modules](./dbt-jinja-functions/modules.md)

[\`modules\` Jinja variable exposes useful Python modules for operating on data.](./dbt-jinja-functions/modules.md)

## [on-run-end context](./dbt-jinja-functions/on-run-end-context.md)

[Use these variables in the context for \`on-run-end\` hooks.](./dbt-jinja-functions/on-run-end-context.md)

## [packages.yml context](<https://docs.getdbt.com/reference/dbt-jinja-functions/packages.yml context.md>)

[Use these context methods to configure dependencies in the packages.yml file.](<https://docs.getdbt.com/reference/dbt-jinja-functions/packages.yml context.md>)

## [print](./dbt-jinja-functions/print.md)

[Use the \`print()\` to print messages to the log file and stdout.](./dbt-jinja-functions/print.md)

## [profiles.yml context](./dbt-jinja-functions/profiles-yml-context.md)

[Use these context methods to configure resources in \`profiles.yml\` file.](./dbt-jinja-functions/profiles-yml-context.md)

## [project\_name](./dbt-jinja-functions/project_name.md)

[Read this guide to understand the project\_name Jinja function in dbt.](./dbt-jinja-functions/project_name.md)

## [properties.yml context](./dbt-jinja-functions/dbt-properties-yml-context.md)

[The context methods and variables available when configuring resources in a properties.yml file.](./dbt-jinja-functions/dbt-properties-yml-context.md)

## [ref](./dbt-jinja-functions/ref.md)

[Read this guide to understand the ref Jinja function in dbt.](./dbt-jinja-functions/ref.md)

## [return](./dbt-jinja-functions/return.md)

[Read this guide to understand the return Jinja function in dbt.](./dbt-jinja-functions/return.md)

## [run\_query](./dbt-jinja-functions/run_query.md)

[Use the \`run\_query\` macro to run queries and fetch results; learn when it runs against the warehouse during compilation and \`dbt docs generate\`, and how to scope side-effecting SQL with \`flags.WHICH\`.](./dbt-jinja-functions/run_query.md)

## [run\_started\_at](./dbt-jinja-functions/run_started_at.md)

[Use \`run\_started\_at\` to output the timestamp the run started.](./dbt-jinja-functions/run_started_at.md)

## [schema](./dbt-jinja-functions/schema.md)

[The schema that the model is configured to be materialized in.](./dbt-jinja-functions/schema.md)

## [schemas](./dbt-jinja-functions/schemas.md)

[A list of schemas where dbt built objects during the current run.](./dbt-jinja-functions/schemas.md)

## [selected\_resources](./dbt-jinja-functions/selected_resources.md)

[Contains a list of all the nodes selected by current dbt command.](./dbt-jinja-functions/selected_resources.md)

## [set](./dbt-jinja-functions/set.md)

[Converts any iterable to a sequence of iterable and unique elements.](./dbt-jinja-functions/set.md)

## [source](./dbt-jinja-functions/source.md)

[Read this guide to understand the source Jinja function in dbt.](./dbt-jinja-functions/source.md)

## [statement blocks](./dbt-jinja-functions/statement-blocks.md)

[SQL queries that hit database and return results to your Jinja context.](./dbt-jinja-functions/statement-blocks.md)

## [target](./dbt-jinja-functions/target.md)

[The \`target\` variable contains information about your connection to the warehouse.](./dbt-jinja-functions/target.md)

## [this](./dbt-jinja-functions/this.md)

[Represents the current model in the database.](./dbt-jinja-functions/this.md)

## [thread\_id](./dbt-jinja-functions/thread_id.md)

[The \`thread\_id\` outputs an identifier for the current Python thread.](./dbt-jinja-functions/thread_id.md)

## [tojson](./dbt-jinja-functions/tojson.md)

[Use this context method to serialize a Python object primitive.](./dbt-jinja-functions/tojson.md)

## [toyaml](./dbt-jinja-functions/toyaml.md)

[Used to serialize a Python object primitive.](./dbt-jinja-functions/toyaml.md)

## [var](./dbt-jinja-functions/var.md)

[Pass variables from \`dbt\_project.yml\` file into models.](./dbt-jinja-functions/var.md)

## [zip](./dbt-jinja-functions/zip.md)

[Use this context method to return an iterator of tuples.](./dbt-jinja-functions/zip.md)

[Previous](../category/jinja-reference.md)

[Jinja reference](../category/jinja-reference.md)

[Next](./dbt-jinja-functions/adapter.md)

[adapter](./dbt-jinja-functions/adapter.md)
