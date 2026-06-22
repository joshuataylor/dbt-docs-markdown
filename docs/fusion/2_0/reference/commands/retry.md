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

<!-- -->

The following flags are supported when you run `dbt retry` with the dbt Fusion engine:

| Flag                      | Input value | Description                                                      | Example                                          |
| ------------------------- | ----------- | ---------------------------------------------------------------- | ------------------------------------------------ |
| `-t, --target`            | target      | The target to execute                                            | `dbt retry -t prod`                              |
| `--project-dir`           | path        | The directory to load the dbt project from                       | `dbt retry --project-dir .`                      |
| `--profile`               | profile     | The profile to use                                               | `dbt retry --profile jaffle_shop`                |
| `--profiles-dir`          | path        | The directory to load the profiles from                          | `dbt retry --profiles-dir ~/.dbt`                |
| `--packages-install-path` | path        | The directory to install packages                                | `dbt retry --packages-install-path dbt_packages` |
| `--target-path`           | path        | The output directory for all produced assets                     | `dbt retry --target-path target`                 |
| `--vars`                  | vars        | Variables for the project (use the format shown in the CLI help) | `dbt retry --vars '{"my_var": "new_value"}'`     |

Search table...

|                  |   |   |   |   |
| ---------------- | - | - | - | - |
| Loading table... |   |   |   |   |

<br />

Run `dbt retry --help` for the full list of flags available.

### Fusion node selection[​](#fusion-node-selection "Direct link to Fusion node selection")

Unlike `dbt retry` with dbt Core, Fusion lets you narrow what gets retried using [`--select`](https://docs.getdbt.com/reference/node-selection/syntax.md), [`--exclude`](https://docs.getdbt.com/reference/node-selection/syntax.md), and [`--selector`](https://docs.getdbt.com/reference/node-selection/yaml-selectors.md). Those arguments override the prior invocation’s selection set for the retry run instead of only inheriting it.

#### Examples[​](#examples "Direct link to Examples")

```shell
dbt retry --select my_model+
```

```shell
dbt retry --exclude package:analytics --selector nightly_models
```

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

<!-- -->

With `dbt retry`, you can optionally pass new [`--select`](https://docs.getdbt.com/reference/node-selection/syntax.md), [`--exclude`](https://docs.getdbt.com/reference/node-selection/syntax.md), or [`--selector`](https://docs.getdbt.com/reference/node-selection/yaml-selectors.md) arguments to narrow the retry scope, as described in [Retry flags](#retry-flags).

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

## Was this page helpful?

YesNo

[Privacy policy](https://www.getdbt.com/cloud/privacy-policy)[Create a GitHub issue](https://github.com/dbt-labs/docs.getdbt.com/issues)

This site is protected by reCAPTCHA and the Google [Privacy Policy](https://policies.google.com/privacy) and [Terms of Service](https://policies.google.com/terms) apply.
