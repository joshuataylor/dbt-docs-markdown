# Syntax overview

dbt's node selection syntax makes it possible to run only specific resources in a given invocation of dbt. This selection syntax is used for the following subcommands:

| command                                                                 | argument(s)                                                         |
| ----------------------------------------------------------------------- | ------------------------------------------------------------------- |
| [run](https://docs.getdbt.com/reference/commands/run.md)                | `--select`, `--exclude`, `--selector`, `--defer`                    |
| [test](https://docs.getdbt.com/reference/commands/test.md)              | `--select`, `--exclude`, `--selector`, `--defer`                    |
| [seed](https://docs.getdbt.com/reference/commands/seed.md)              | `--select`, `--exclude`, `--selector`                               |
| [snapshot](https://docs.getdbt.com/reference/commands/snapshot.md)      | `--select`, `--exclude` `--selector`                                |
| [ls (list)](https://docs.getdbt.com/reference/commands/list.md)         | `--select`, `--exclude`, `--selector`, `--resource-type`            |
| [compile](https://docs.getdbt.com/reference/commands/compile.md)        | `--select`, `--exclude`, `--selector`, `--inline`                   |
| [freshness](https://docs.getdbt.com/reference/commands/source.md)       | `--select`, `--exclude`, `--selector`                               |
| [build](https://docs.getdbt.com/reference/commands/build.md)            | `--select`, `--exclude`, `--selector`, `--resource-type`, `--defer` |
| [docs generate](https://docs.getdbt.com/reference/commands/cmd-docs.md) | `--select`, `--exclude`, `--selector`                               |

Search table...

|                  |   |   |   |   |
| ---------------- | - | - | - | - |
| Loading table... |   |   |   |   |

Nodes and resources

We use the terms ["nodes"](https://en.wikipedia.org/wiki/Vertex_\(graph_theory\)) and "resources" interchangeably. These encompass all the models, tests, sources, seeds, snapshots, exposures, and analyses in your project. They are the objects that make up dbt's DAG (directed acyclic graph).

The `--select` and `--selector` arguments are similar in that they both allow you to select resources. To understand the difference, see [Differences between `--select` and `--selector`](https://docs.getdbt.com/reference/node-selection/yaml-selectors.md#difference-between---select-and---selector).

## Specifying resources[​](#specifying-resources "Direct link to Specifying resources")

By default, `dbt run` executes *all* of the models in the dependency graph; `dbt seed` creates all seeds, `dbt snapshot` performs every snapshot. The `--select` flag is used to specify a subset of nodes to execute.

To follow [POSIX standards](https://pubs.opengroup.org/onlinepubs/9699919799/basedefs/V1_chap12.html) and make things easier to understand, we recommend CLI users use quotes when passing arguments to the `--select` or `--exclude` option (including single or multiple space-delimited, or comma-delimited arguments). Not using quotes might not work reliably on all operating systems, terminals, and user interfaces. For example, `dbt run --select "my_dbt_project_name"` runs all models in your project.

### How does selection work?[​](#how-does-selection-work "Direct link to How does selection work?")

1. dbt gathers all the resources that are matched by one or more of the `--select` criteria, in the order of [selection methods](https://docs.getdbt.com/reference/node-selection/methods.md) (e.g. `tag:`), then [graph operators](https://docs.getdbt.com/reference/node-selection/graph-operators.md) (e.g. `+`), then finally set operators ([unions](https://docs.getdbt.com/reference/node-selection/set-operators.md#unions), [intersections](https://docs.getdbt.com/reference/node-selection/set-operators.md#intersections), [exclusions](https://docs.getdbt.com/reference/node-selection/exclude.md)).

<!-- -->

tip

You can combine multiple selector methods in one `--select` command by separating them with commas (`,`) without whitespace (for example, `dbt run --select "marts.finance,tag:nightly"`). This only selects resources that satisfy *all* arguments. In this example, the command runs models that are in the `marts/finance` subdirectory and tagged `nightly`. For more information, see [Set operators](https://docs.getdbt.com/reference/node-selection/set-operators.md).

2. The selected resources may be models, sources, seeds, snapshots, tests. (Tests can also be selected "indirectly" via their parents; see [test selection examples](https://docs.getdbt.com/reference/node-selection/test-selection-examples.md) for details.)

3. dbt now has a list of still-selected resources of varying types. As a final step, it tosses away any resource that does not match the resource type of the current task. (Only seeds are kept for `dbt seed`, only models for `dbt run`, only tests for `dbt test`, and so on.)

## Shorthand[​](#shorthand "Direct link to Shorthand")

Select resources to build (run, test, seed, snapshot) or check freshness: `--select`, `-s`

### Examples[​](#examples "Direct link to Examples")

By default, `dbt run` will execute *all* of the models in the dependency graph. During development (and deployment), it is useful to specify only a subset of models to run. Use the `--select` flag with `dbt run` to select a subset of models to run. Note that the following arguments (`--select`, `--exclude`, and `--selector`) also apply to other dbt tasks, such as `test` and `build`.

* Examples of select flag
* Examples of subsets of nodes

The `--select` flag accepts one or more arguments. Each argument can be one of:

1. a package name
2. a model name
3. a fully-qualified path to a directory of models
4. a selection method (`path:`, `tag:`, `config:`, `test_type:`, `test_name:`)

Examples:

```bash
dbt run --select "my_dbt_project_name"   # runs all models in your project
dbt run --select "my_dbt_model"          # runs a specific model
dbt run --select "path/to/my/models"     # runs all models in a specific directory
dbt run --select "my_package.some_model" # run a specific model in a specific package
dbt run --select "tag:nightly"           # run models with the "nightly" tag
dbt run --select "path/to/models"        # run models contained in path/to/models
dbt run --select "path/to/my_model.sql"  # run a specific model by its path
```

dbt supports a shorthand language for defining subsets of nodes. This language uses the following characters:

* plus operator [(`+`)](https://docs.getdbt.com/reference/node-selection/graph-operators.md#the-plus-operator)
* at operator [(`@`)](https://docs.getdbt.com/reference/node-selection/graph-operators.md#the-at-operator)
* asterisk operator (`*`)
* comma operator (`,`)

Examples:

```bash
# multiple arguments can be provided to --select
dbt run --select "my_first_model my_second_model"

# select my_model and all of its children
dbt run --select "my_model+"     

# select my_model, its children, and the parents of its children
dbt run --select @my_model          

# these arguments can be projects, models, directory paths, tags, or sources
dbt run --select "tag:nightly my_model finance.base.*"

# use methods and intersections for more complex selectors
dbt run --select "path:marts/finance,tag:nightly,config.materialized:table"
```

As your selection logic gets more complex, and becomes unwieldly to type out as command-line arguments, consider using a [yaml selector](https://docs.getdbt.com/reference/node-selection/yaml-selectors.md). You can use a predefined definition with the `--selector` flag. Note that when you're using `--selector`, most other flags (namely `--select` and `--exclude`) will be ignored.

The `--select` and `--selector` arguments are similar in that they both allow you to select resources. To understand the difference between `--select` and `--selector` arguments, see [this section](https://docs.getdbt.com/reference/node-selection/yaml-selectors.md#difference-between---select-and---selector) for more details.

### Troubleshoot with the `ls` command[​](#troubleshoot-with-the-ls-command "Direct link to troubleshoot-with-the-ls-command")

Constructing and debugging your selection syntax can be challenging. To get a "preview" of what will be selected, we recommend using the [`list` command](https://docs.getdbt.com/reference/commands/list.md). This command, when combined with your selection syntax, will output a list of the nodes that meet that selection criteria. The `dbt ls` command supports all types of selection syntax arguments, for example:

```bash
dbt ls --select "path/to/my/models" # Lists all models in a specific directory.
dbt ls --select "source_status:fresher+" # Shows sources updated since the last dbt source freshness run.
dbt ls --select state:modified+ # Displays nodes modified in comparison to a previous state.
dbt ls --select "result:<status>+" state:modified+ --state ./<dbt-artifact-path> # Lists nodes that match certain result statuses and are modified.
```

### Questions from the Community[​](#questions-from-the-community "Direct link to Questions from the Community")

![Loading](/img/loader-icon.svg)[Ask the Community](https://discourse.getdbt.com/new-topic?category=help\&tags=node-selection "Ask the Community")

## Was this page helpful?

YesNo

[Privacy policy](https://www.getdbt.com/cloud/privacy-policy)[Create a GitHub issue](https://github.com/dbt-labs/docs.getdbt.com/issues)

This site is protected by reCAPTCHA and the Google [Privacy Policy](https://policies.google.com/privacy) and [Terms of Service](https://policies.google.com/terms) apply.
