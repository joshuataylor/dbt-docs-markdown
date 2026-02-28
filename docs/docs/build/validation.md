# Validations

Validations refer to the process of checking whether a system or configuration meets the expected requirements or constraints. In the case of the Semantic Layer, powered by MetricFlow, there are three built-in validations — [parsing](#parsing), [semantic](#semantic), and [data platform](#data-platform).

These validations ensure that configuration files follow the expected schema, the semantic graph doesn't violate any constraints, and semantic definitions in the graph exist in the physical table — providing effective data governance support. These three validation steps occur sequentially and must succeed before proceeding to the next step.

The code that handles validation [can be found here](https://github.com/dbt-labs/dbt-semantic-interfaces/tree/main/dbt_semantic_interfaces/validations) for those who want to dive deeper into this topic.

## Validations command[​](#validations-command "Direct link to Validations command")

You can run validations from the dbt platform or the command line with the following [MetricFlow commands](https://docs.getdbt.com/docs/build/metricflow-commands.md). In dbt, you need developer credentials to run `dbt sl validate` in the IDE or CLI, and deployment credentials to run it in CI.

* For Fusion and dbt users in the dbt platform CLI or locally with a valid `dbt_cloud.yml`:

  ```bash
  dbt sl validate
  ```

  This runs parsing, semantic, and (where supported) data platform validations.

  When using `dbt sl validate` locally, the command validates your local semantic manifest, and not the platform's manifest. This means your uncommitted local changes are included in the validation.

* For dbt Core (open source) users or Fusion CLI users not connected to dbt platform and using local MetricFlow:

  ```bash
  mf validate-configs
  ```

  This runs parsing and semantic validations.

## Availability by environment[​](#availability-by-environment "Direct link to Availability by environment")

Validation behavior and availability differ depending on your environment and setup:

| Environment       | Who can use                                         | Parsing | Semantic syntax | Data platform | How to run                                                                                                                                                                                                                                                                                                                                                                                                  |
| ----------------- | --------------------------------------------------- | ------- | --------------- | ------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| dbt Fusion engine | dbt platform users for full Semantic Layer features | ✅      | ✅ \*           | ✅            | - Parsing validations run automatically while generating the semantic manifest.<br />- When running in development, semantic syntax validations run automatically on dbt platform if `dbt_cloud.yml` is configured. If not, run manually using `mf-validate-configs`.<br />- Data platform validations don't run automatically for Fusion. You must run `dbt sl validate` to run data platform validations. |
| dbt CLI           | dbt platform users                                  | ✅      | ✅              | ✅            | Run any dbt CLI command; validations execute automatically except data platform validations. You must run `dbt sl validate` to run data platform validations.                                                                                                                                                                                                                                               |
| dbt Core          | Open source users                                   | ✅      | ✅              | ❌            | Use dbt Core for parsing/builds. Run additional validation manually with the MetricFlow CLI.                                                                                                                                                                                                                                                                                                                |
| MetricFlow CLI    | Open source users                                   | ✅      | ✅              | ✅            | Run `mf validate-configs` locally to validate and test metrics.                                                                                                                                                                                                                                                                                                                                             |

Search table...

|                  |   |   |   |   |
| ---------------- | - | - | - | - |
| Loading table... |   |   |   |   |

\*Jobs run in **Orchestration** or **Studio IDE** run this validation automatically.

## Parsing[​](#parsing "Direct link to Parsing")

In this validation step, we ensure your config files follow the defined schema for each semantic graph object and can be parsed successfully. It validates the schema for the following core objects:

<!-- -->

<!-- -->

## Semantic syntax[​](#semantic-syntax "Direct link to Semantic syntax")

This syntactic validation step occurs after we've built your semantic graph. The Semantic Layer, powered by MetricFlow, runs a suite of tests to ensure that your semantic graph doesn't violate any constraints. For example, we check to see if <!-- -->names are unique, or if metrics referenced in materialization exist. The current semantic rules we check for are:

<!-- -->

<!-- -->

## Data platform[​](#data-platform "Direct link to Data platform")

This type of validation checks to see if the semantic definitions in your semantic graph exist in the underlying physical table. To test this, we run queries against your data platform to ensure the generated SQL for semantic models, dimensions, and metrics will execute.

We run the following checks:

<!-- -->

<!-- -->

You can run semantic validations (against your semantic layer) in a CI job to guarantee any code changes made to dbt models don't break these metrics. For more information, refer to [semantic validation in CI](https://docs.getdbt.com/docs/deploy/ci-jobs.md#semantic-validations-in-ci).

## Was this page helpful?

YesNo

[Privacy policy](https://www.getdbt.com/cloud/privacy-policy)[Create a GitHub issue](https://github.com/dbt-labs/docs.getdbt.com/issues)

This site is protected by reCAPTCHA and the Google [Privacy Policy](https://policies.google.com/privacy) and [Terms of Service](https://policies.google.com/terms) apply.
