# About dbt compile command

`dbt compile` generates executable SQL from source files for:

<!-- -->

* [Models](https://docs.getdbt.com/docs/build/models.md)
* [Data tests](https://docs.getdbt.com/docs/build/data-tests.md)
* [Analyses](https://docs.getdbt.com/docs/build/analyses.md)
* [Functions](https://docs.getdbt.com/docs/build/udfs.md)
* [Snapshots](https://docs.getdbt.com/docs/build/snapshots.md) (available in dbt Core v1.12)

You can find these compiled SQL files in the `target/` directory of your dbt project.

The `compile` command is useful for:

* Visually inspecting the compiled output of resource files. This is useful for validating complex Jinja logic or macro usage.
* Manually running compiled SQL. While debugging a model or [data test](https://docs.getdbt.com/docs/build/data-tests.md), it's often useful to execute the underlying `select` statement to find the source of the bug.
* Compiling `analysis` files. Read more about analysis files [here](https://docs.getdbt.com/docs/build/analyses.md).

Some common misconceptions:

* `dbt compile` is *not* a pre-requisite of `dbt run`, or other building commands. Those commands will handle compilation themselves.
* If you just want dbt to read and validate your project code, without connecting to the data warehouse, use `dbt parse` instead.

### Interactive compile[​](#interactive-compile "Direct link to Interactive compile")

Starting in dbt v1.5, `compile` can be "interactive" in the CLI, by displaying the compiled code of a node or arbitrary dbt-SQL query:

* `--select` a specific node *by name*
* `--inline` an arbitrary dbt-SQL query

This will log the compiled SQL to the terminal, in addition to writing to the `target/` directory.

For example:

```bash
dbt compile --select "stg_orders"                           
dbt compile --inline "select * from {{ ref('raw_orders') }}"
```

returns the following:

```bash
dbt compile --select "stg_orders"                           

21:17:09  Running with dbt=1.7.5
21:17:09  Registered adapter: postgres=1.7.5
21:17:09  Found 5 models, 3 seeds, 20 tests, 0 sources, 0 exposures, 0 metrics, 401 macros, 0 groups, 0 semantic models
21:17:09  
21:17:09 Concurrency: 24 threads (target='dev')
21:17:09  
21:17:09  Compiled node 'stg_orders' is:
with source as (
    select * from "jaffle_shop"."main"."raw_orders"

),

renamed as (

    select
        id as order_id,
        user_id as customer_id,
        order_date,
        status

    from source

)

select * from renamed
```

```bash
dbt compile --inline "select * from {{ ref('raw_orders') }}"

18:15:49  Running with dbt=1.7.5
18:15:50  Registered adapter: postgres=1.7.5
18:15:50  Found 5 models, 3 seeds, 20 tests, 0 sources, 0 exposures, 0 metrics, 401 macros, 0 groups, 0 semantic models
18:15:50  
18:15:50  Concurrency: 5 threads (target='postgres')
18:15:50  
18:15:50  Compiled inline node is:
select * from "jaffle_shop"."main"."raw_orders"
```

The command accesses the data platform to cache-related metadata, and to run introspective queries. Use the flags:

* `--no-populate-cache` to disable the initial cache population. If metadata is needed, it will be a cache miss, requiring dbt to run the metadata query. This is a `dbt` flag, which means you need to add `dbt` as a prefix. For example: `dbt --no-populate-cache`.
* `--no-introspect` to disable [introspective queries](https://docs.getdbt.com/faqs/Warehouse/db-connection-dbt-compile.md#introspective-queries). dbt will raise an error if a resource's definition requires running one. This is a `dbt compile` flag, which means you need to add `dbt compile` as a prefix. For example: `dbt compile --no-introspect`.

Resources that use introspective queries

Compiled SQL for resources that use introspective queries may depend on metadata from your warehouse. Compilation may be incomplete or may differ depending on the state of that metadata.

### Compiling tests with `--select`[​](#compiling-tests-with---select "Direct link to compiling-tests-with---select")

You can use `dbt compile` to compile tests, as long as your selector matches a test node in the project.

You can also target groups of tests with selector methods:

**Compile all test nodes:**

```bash
dbt compile --select "resource_type:test"
```

**Compile only generic tests:**

```bash
dbt compile --select "test_type:generic"
```

**Compile only singular tests:**

```bash
dbt compile --select "test_type:singular"
```

If dbt returns `selection does not match any nodes`, your selector did not match a discovered node. To troubleshoot:

1. List tests for a model selector:

```bash
dbt ls --resource-type test --select "MODEL_NAME"
```

2. Copy one of the returned test node names into `dbt compile --select`:

```bash
dbt compile --select "TEST_NODE_NAME"
```

For example, a returned test node name may look like this:

```text
FULL_TEST_NODE_NAME
```

3. If no tests are returned, check test definitions and project paths before running `compile` again.

For more selector patterns, refer to [Test selection examples](https://docs.getdbt.com/reference/node-selection/test-selection-examples.md).

### FAQs[​](#faqs "Direct link to FAQs")

Why dbt compile needs a data platform connection

`dbt compile` needs a data platform connection in order to gather the info it needs (including from introspective queries) to prepare the SQL for every model in your project.

### dbt compile[​](#dbt-compile "Direct link to dbt compile")

The [`dbt compile` command](https://docs.getdbt.com/reference/commands/compile.md) generates executable SQL from `source`, `model`, `test`, and `analysis` files. `dbt compile` is similar to `dbt run` except that it doesn't materialize the model's compiled SQL into an existing table. So, up until the point of materialization, `dbt compile` and `dbt run` are similar because they both require a data platform connection, run queries, and have an [`execute` variable](https://docs.getdbt.com/reference/dbt-jinja-functions/execute.md) set to `True`.

However, here are some things to consider:

* You don't need to execute `dbt compile` before `dbt run`
* In dbt, `compile` doesn't mean `parse`. This is because `parse` validates your written `YAML`, configured tags, and so on.

### Introspective queries[​](#introspective-queries "Direct link to Introspective queries")

To generate the compiled SQL for many models, dbt needs to run introspective queries, (which is when dbt needs to run SQL in order to pull data back and do something with it) against the data platform.

These introspective queries include:

* Populating the relation cache. For more information, refer to the [Create new materializations](https://docs.getdbt.com/guides/create-new-materializations.md) guide. Caching speeds up the metadata checks, including whether an [incremental model](https://docs.getdbt.com/docs/build/incremental-models.md) already exists in the data platform.
* Resolving [macros](https://docs.getdbt.com/docs/build/jinja-macros.md#macros), such as `run_query` or `dbt_utils.get_column_values` that you're using to template out your SQL. This is because dbt needs to run those queries during model SQL compilation.
* [`dbt docs generate`](https://docs.getdbt.com/reference/commands/cmd-docs.md) compiles your project by default (unless you pass [`--no-compile`](https://docs.getdbt.com/reference/commands/cmd-docs.md)), so introspective macros such as [`run_query`](https://docs.getdbt.com/reference/dbt-jinja-functions/run_query.md) run against the warehouse during documentation builds the same way they do during other compile workflows. Refer to [`run_query`](https://docs.getdbt.com/reference/dbt-jinja-functions/run_query.md) for how that works and for using `flags.WHICH` when you want to limitDML or other side-effecting SQL to specific dbt commands.

Without a data platform connection, dbt can't perform these introspective queries and won't be able to generate the compiled SQL needed for the next steps in the dbt workflow. You can [`parse`](https://docs.getdbt.com/reference/commands/parse.md) a project and use the [`list`](https://docs.getdbt.com/reference/commands/list.md) resources in the project, without an internet or data platform connection. Parsing a project is enough to produce a [manifest](https://docs.getdbt.com/reference/artifacts/manifest-json.md), however, keep in mind that the written-out manifest won't include compiled SQL.

To configure a project, you do need a [connection profile](https://docs.getdbt.com/docs/local/profiles.yml.md) (`profiles.yml` if using the CLI). You need this file because the project's configuration depends on its contents. For example, you may need to use [`{{target}}`](https://docs.getdbt.com/reference/dbt-jinja-functions/target.md) for conditional configs or know what platform you're running against so that you can choose the right flavor of SQL.

## Was this page helpful?

YesNo

[Privacy policy](https://www.getdbt.com/cloud/privacy-policy)[Create a GitHub issue](https://github.com/dbt-labs/docs.getdbt.com/issues)

This site is protected by reCAPTCHA and the Google [Privacy Policy](https://policies.google.com/privacy) and [Terms of Service](https://policies.google.com/terms) apply.
