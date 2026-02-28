# Using threads

When dbt runs, it creates a directed acyclic graph (DAG) of links between models. The number of threads represents the maximum number of paths through the graph dbt may work on at once – increasing the number of threads can minimize the run time of your project.

For example, if you specify `threads: 1`, dbt will start building only one model, and finish it, before moving onto the next. Specifying `threads: 8` means that dbt will work on *up to* 8 models at once without violating dependencies – the actual number of models it can work on will likely be constrained by the available paths through the dependency graph.

There's no set limit of the maximum number of threads you can set – while increasing the number of threads generally decreases execution time, there are a number of things to consider:

* Increasing the number of threads increases the load on your warehouse, which may impact other tools in your data stack. For example, if your BI tool uses the same compute resources as dbt, their queries may get queued during a dbt run.
* The number of concurrent queries your database will allow you to run may be a limiting factor in how many models can be actively built – some models may queue while waiting for an available query slot.

Generally the optimal number of threads depends on your data warehouse and its configuration. It’s best to test different values to find the best number of threads for your project. We recommend setting this to 4 to start with.

You can use a different number of threads than the value defined in your target by using the `--threads` option when executing a dbt command.

You will define the number of threads in your `profiles.yml` file (when developing locally with dbt Core and the dbt Fusion engine), dbt job definition, and dbt development credentials under your profile.

## Fusion engine thread optimization[​](#fusion-engine-thread-optimization "Direct link to Fusion engine thread optimization")

The dbt Fusion engine handles threading differently than dbt Core. Rather than treating `threads` as a strict limit, Fusion manages parallelism based on each adapter's characteristics.

### Adapter-specific behavior[​](#adapter-specific-behavior "Direct link to Adapter-specific behavior")

| Adapter        | Behavior                                                                                                                                                                                              |
| -------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **Snowflake**  | Fusion ignores user-set threads and automatically optimizes parallelism for maximum performance.<br />The only supported override is `threads: 1`, which can also help resolve timeout issues if set. |
| **Databricks** | Fusion ignores user-set threads and automatically optimizes parallelism for maximum performance.<br />The only supported override is `threads: 1`, which can also help resolve timeout issues if set. |
| **BigQuery**   | Fusion respects user-set threads to manage API rate limits.<br />Setting `--threads 0` (or omitting the setting) allows Fusion to dynamically optimize parallelism.                                   |
| **Redshift**   | Fusion respects user-set threads to manage concurrency limits.<br />Setting `--threads 0` (or omitting the setting) allows Fusion to dynamically optimize parallelism.                                |

Search table...

|                  |   |   |   |   |
| ---------------- | - | - | - | - |
| Loading table... |   |   |   |   |

For more information about Fusion's approach to parallelism, refer to [the dbt Fusion engine](https://docs.getdbt.com/docs/fusion.md) page.

## Related docs[​](#related-docs "Direct link to Related docs")

* [About profiles.yml](https://docs.getdbt.com/docs/core/connect-data-platform/profiles.yml.md)
* [dbt job scheduler](https://docs.getdbt.com/docs/deploy/job-scheduler.md)

## Was this page helpful?

YesNo

[Privacy policy](https://www.getdbt.com/cloud/privacy-policy)[Create a GitHub issue](https://github.com/dbt-labs/docs.getdbt.com/issues)

This site is protected by reCAPTCHA and the Google [Privacy Policy](https://policies.google.com/privacy) and [Terms of Service](https://policies.google.com/terms) apply.
