# About dbt retry command

Retry re-executes the last invocation from the point of failure.

* If no nodes are executed before the failure (for example, if a run failed early due to a warehouse connection or permission errors), retry won't run anything since there are no recorded nodes to retry from.
* In these cases, we recommend checking your [`run_results.json` file](https://docs.getdbt.com/reference/artifacts/run-results-json.md) and manually re-running the full job so the nodes build.
* Once some nodes have run, you can use retry to re-execute from any new point of failure.
* If the previously executed command completed successfully, retry will finish as `no operation`.

## Retry flags[​](#retry-flags "Direct link to Retry flags")

The `dbt retry` flags apply when you use a local dbt installation or the Studio IDE.

dbt platform CLI

If you use the [dbt platform CLI](https://docs.getdbt.com/docs/platform/dbt-cli-installation.md) against your cloud environment, `dbt retry` accepts only a small subset of overrides—typically `--threads`, `--vars`, and related options. Use `dbt retry --help` on your machine for the exact list your CLI build supports.

The following flags are supported when you run `dbt retry` with the dbt Core engine:

| Flag             | Input value | Description                                                                                              | Example                                      |
| ---------------- | ----------- | -------------------------------------------------------------------------------------------------------- | -------------------------------------------- |
| `--threads`      | int         | Override the number of threads used in the original run                                                  | `dbt retry --threads 8`                      |
| `--vars`         | YAML        | Override variables from the original run                                                                 | `dbt retry --vars '{"my_var": "new_value"}'` |
| `--target`       | target      | Override the target from the original run                                                                | `dbt retry --target prod`                    |
| `--profile`      | profile     | Override the profile from the original run                                                               | `dbt retry --profile jaffle_shop`            |
| `--profiles-dir` | path        | Path to the directory containing `profiles.yml`                                                          | `dbt retry --profiles-dir ~/.dbt`            |
| `--project-dir`  | path        | Path to the directory containing `dbt_project.yml`                                                       | `dbt retry --project-dir .`                  |
| `--target-path`  | path        | Override the target directory path                                                                       | `dbt retry --target-path target`             |
| `--state`        | path        | Path to a directory containing `run_results.json` from a previous run (defaults to the target directory) | `dbt retry --state path/to/previous/run`     |
| `--full-refresh` | —           | Override incremental models to run as full refreshes                                                     | `dbt retry --full-refresh`                   |

Search table...

|                  |   |   |   |   |
| ---------------- | - | - | - | - |
| Loading table... |   |   |   |   |

<br />

Run `dbt retry --help` for the full list of flags available.

<!-- -->

## Supported commands[​](#supported-commands "Direct link to Supported commands")

Retry works with the following commands:

* [`build`](https://docs.getdbt.com/reference/commands/build.md)
* [`compile`](https://docs.getdbt.com/reference/commands/compile.md)
* [`clone`](https://docs.getdbt.com/reference/commands/clone.md)
* [`docs generate`](https://docs.getdbt.com/reference/commands/cmd-docs.md#dbt-docs-generate)
* [`seed`](https://docs.getdbt.com/reference/commands/seed.md)
* [`snapshot`](https://docs.getdbt.com/reference/commands/snapshot.md)
* [`test`](https://docs.getdbt.com/reference/commands/test.md)
* [`run`](https://docs.getdbt.com/reference/commands/run.md)
* [`run-operation`](https://docs.getdbt.com/reference/commands/run-operation.md)

Retry references [run\_results.json](https://docs.getdbt.com/reference/artifacts/run-results-json.md) to determine where to start. Executing retry without correcting the previous failures yields idempotent results.

`dbt retry` reuses the prior command’s selection, including any [`--select`](https://docs.getdbt.com/reference/node-selection/syntax.md), [`--exclude`](https://docs.getdbt.com/reference/node-selection/syntax.md), or [`--selector`](https://docs.getdbt.com/reference/node-selection/yaml-selectors.md) arguments. You cannot override those selectors on retry with dbt Core or the dbt platform CLI.

<!-- -->

Example results of executing `dbt retry` after a successful `dbt run`:

```shell
Running with dbt=1.6.1
Registered adapter: duckdb=1.6.0
Found 5 models, 3 seeds, 20 tests, 0 sources, 0 exposures, 0 metrics, 348 macros, 0 groups, 0 semantic models
 
Nothing to do. Try checking your model configs and model specification args
```

Example of when `dbt run` encounters a syntax error in a model:

```shell
Running with dbt=1.6.1
Registered adapter: duckdb=1.6.0
Found 5 models, 3 seeds, 20 tests, 0 sources, 0 exposures, 0 metrics, 348 macros, 0 groups, 0 semantic models

Concurrency: 24 threads (target='dev')
 
1 of 5 START sql view model main.stg_customers ................................. [RUN]
2 of 5 START sql view model main.stg_orders .................................... [RUN]
3 of 5 START sql view model main.stg_payments .................................. [RUN]
1 of 5 OK created sql view model main.stg_customers ............................ [OK in 0.06s]
2 of 5 OK created sql view model main.stg_orders ............................... [OK in 0.06s]
3 of 5 OK created sql view model main.stg_payments ............................. [OK in 0.07s]
4 of 5 START sql table model main.customers .................................... [RUN]
5 of 5 START sql table model main.orders ....................................... [RUN]
4 of 5 ERROR creating sql table model main.customers ........................... [ERROR in 0.03s]
5 of 5 OK created sql table model main.orders .................................. [OK in 0.04s]
 
Finished running 3 view models, 2 table models in 0 hours 0 minutes and 0.15 seconds (0.15s).
  
Completed with 1 error and 0 warnings:
  
Runtime Error in model customers (models/customers.sql)
 Parser Error: syntax error at or near "selct"

Done. PASS=4 WARN=0 ERROR=1 SKIP=0 TOTAL=5
```

Example of a subsequent failed `dbt retry` run without fixing the error(s):

```shell
Running with dbt=1.6.1
Registered adapter: duckdb=1.6.0
Found 5 models, 3 seeds, 20 tests, 0 sources, 0 exposures, 0 metrics, 348 macros, 0 groups, 0 semantic models

Concurrency: 24 threads (target='dev')

1 of 1 START sql table model main.customers .................................... [RUN]
1 of 1 ERROR creating sql table model main.customers ........................... [ERROR in 0.03s]

Done. PASS=4 WARN=0 ERROR=1 SKIP=0 TOTAL=5
```

Example of a successful `dbt retry` run after fixing error(s):

```shell
Running with dbt=1.6.1
Registered adapter: duckdb=1.6.0
Found 5 models, 3 seeds, 20 tests, 0 sources, 0 exposures, 0 metrics, 348 macros, 0 groups, 0 semantic models
 
Concurrency: 24 threads (target='dev')

1 of 1 START sql table model main.customers .................................... [RUN]
1 of 1 OK created sql table model main.customers ............................... [OK in 0.05s]

Finished running 1 table model in 0 hours 0 minutes and 0.09 seconds (0.09s).
 
Completed successfully
  
Done. PASS=1 WARN=0 ERROR=0 SKIP=0 TOTAL=1
```

In each scenario `dbt retry` picks up from the error rather than running all of the upstream dependencies again.
