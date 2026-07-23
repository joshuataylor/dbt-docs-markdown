# Optimize costs in dbt

dbt offers ways to optimize your model’s built usage and warehouse costs.

### Best practices for optimizing cost with dbt State[​](#best-practices-for-optimizing-cost-with-dbt-state "Direct link to Best practices for optimizing cost with dbt State")

#### Use `lag_tolerance` to reduce unnecessary model execution[​](#use-lag_tolerance-to-reduce-unnecessary-model-execution "Direct link to use-lag_tolerance-to-reduce-unnecessary-model-execution")

You can save even more time and compute by defining how old your data can be before a model should be triggered. We’ve introduced lag\_tolerance so that you can do things like differentiate development needs vs prod.

For example:

dbt\_project.yml

```yaml
models:
  +state:
    lag_tolerance: "{{ '4h' if target.name == 'prod' else '7d' }}"
```

In this example, models in the `prod` target rebuild only when upstream data is more than 4 hours old. In all other environments, models wait 7 days before rebuilding.

For more details, refer to the [`lag_tolerance` config reference](https://docs.getdbt.com/reference/resource-configs/lag-tolerance.md).

#### Use selectors with `dbt build` to run limited upstream nodes[​](#use-selectors-with-dbt-build-to-run-limited-upstream-nodes "Direct link to use-selectors-with-dbt-build-to-run-limited-upstream-nodes")

In development, use [selectors](https://docs.getdbt.com/reference/node-selection/yaml-selectors.md) with `dbt build` to limit how many upstream nodes run. Nodes that are not selected can be [deferred](https://docs.getdbt.com/reference/node-selection/defer.md) instead of rebuilt, which avoids extra dbt State activity on those targets. Automatic `state:modified` selection in development may be supported in a future release.

#### Avoid conditional materializations[​](#avoid-conditional-materializations "Direct link to Avoid conditional materializations")

Avoid conditional materialization patterns such as `table` in production and `view` in development for the same model. Different materializations between environments can prevent dbt State from matching targets correctly and reduce skip/clone effectiveness.

### Best practices for optimizing successful models built[​](#best-practices-for-optimizing-successful-models-built "Direct link to Best practices for optimizing successful models built")

You can reduce costs from successful models built while still following best practices. Combine the approaches below to fit your needs. If you exclude views from your scheduled job runs, set up a [merge job](#exclude-views-while-running-tests) to deploy updated view logic when changes are detected.

#### Exclude views in a dbt job[​](#exclude-views-in-a-dbt-job "Direct link to Exclude views in a dbt job")

Many dbt users utilize views, which don’t always need to be rebuilt every time you run a job. For any jobs that contain views that *do not* include macros that dynamically generate code (for example, case statements) based on upstream tables and also *do not* have tests, you can implement these steps:

1. Go to your current production deployment job in dbt.
2. Modify your command to include: `--exclude config.materialized:view`.
3. Save your job changes.

If you have views that contain macros with case statements based on upstream tables, these will need to be run each time to account for new values. If you still need to test your views with each run, follow the [Exclude views while still running tests](#exclude-views-while-running-tests) best practice to create a custom selector.

#### Exclude views while running tests[​](#exclude-views-while-running-tests "Direct link to Exclude views while running tests")

Running tests for views in every job run can help keep data quality intact and save you from the need to rerun failed jobs. To exclude views from your job run while running tests, you can follow these steps to create a custom [selector](https://docs.getdbt.com/reference/node-selection/yaml-selectors.md) for your job command.

1. Open your dbt project in the Studio IDE.

2. Add a file called `selectors.yml` in your top-level project folder.

3. In the file, add the following code:

   ```yaml
    selectors:
      - name: skip_views_but_test_views
        description: >
          A default selector that will exclude materializing views
          without skipping tests on views.
        default: true
        definition:
          union:
            - union: 
              - method: path
                value: "*"
              - exclude: 
                - method: config.materialized
                  value: view
            - method: resource_type
              value: test
   ```

4. Save the file and commit it to your project.

5. Modify your dbt jobs to include `dbt run --select selector:skip_views_but_test_views`.

#### Build only changed views[​](#build-only-changed-views "Direct link to Build only changed views")

If you want to ensure that you're building views whenever the logic is changed, create a merge job that gets triggered when code is merged into main:

1. Ensure you have a [CI job setup](https://docs.getdbt.com/docs/deploy/ci-jobs.md) in your environment.
2. Create a new [deploy job](https://docs.getdbt.com/docs/deploy/deploy-jobs.md#create-and-schedule-jobs) and call it “Merge Job".
3. Set the **Environment** to your CI environment. Refer to [Types of environments](https://docs.getdbt.com/docs/deploy/deploy-environments.md#types-of-environments) for more details.
4. Set **Commands** to: `dbt run -s state:modified+`. Executing `dbt build` in this context is unnecessary because the CI job was used to both run and test the code that just got merged into main.
5. Under the **Execution Settings**, select the default production job to compare changes against:
   <!-- -->
   * **Defer to a previous run state** — Select the “Merge Job” you created so the job compares and identifies what has changed since the last merge.
6. Follow [Customizing CI/CD with custom pipelines](https://docs.getdbt.com/guides/custom-cicd-pipelines.md) to create a script that triggers the dbt API to run your job after a merge, or watch this [video](https://www.loom.com/share/e7035c61dbed47d2b9b36b5effd5ee78?sid=bcf4dd2e-b249-4e5d-b173-8ca204d9becb).

The merge job immediately deploys PR changes to production and keeps production views current with your codebase while staying cost-efficient. Decide whether this change is right for your dbt project.

### Rework inefficient models[​](#rework-inefficient-models "Direct link to Rework inefficient models")

#### Job Insights tab[​](#job-insights-tab "Direct link to Job Insights tab")

To reduce warehouse spend, use the **Insights** tab on the **Job** page to find which models take longest to build. The chart shows each model's average run time over its last 20 runs; the slowest models are prime candidates for optimization.

#### Model Timing tab[​](#model-timing-tab "Direct link to Model Timing tab")

To see how long each model takes within a specific run, select that run on the **Run History** page and click the **Model Timing** tab.

Once you've identified which models could be optimized, check out these other resources that walk through how to optimize your work:

* [Build scalable and trustworthy data pipelines with dbt and BigQuery](https://services.google.com/fh/files/misc/dbt_bigquery_whitepaper.pdf)
* [Best Practices for Optimizing Your dbt and Snowflake Deployment](https://www.snowflake.com/wp-content/uploads/2021/10/Best-Practices-for-Optimizing-Your-dbt-and-Snowflake-Deployment.pdf)
* [How to optimize and troubleshoot dbt models on Databricks](https://docs.getdbt.com/guides/optimize-dbt-models-on-databricks.md)

For answers to common plan and billing questions, refer to [Billing FAQs](https://docs.getdbt.com/docs/platform/billing-faqs.md).
