# Are the results of freshness stored anywhere?

Yes!

(Applies to dbt v2.0 and later)

The [`dbt freshness`](../../reference/commands/freshness.md) command measures how recently data was loaded and reports a pass/warning/error for each source or model based on your `warn_after` and `error_after` thresholds.

dbt writes the model and source freshness results to `target/freshness.json`. When sources are included, it also writes `target/sources.json` for backward compatibility.

After enabling source freshness within a job, configure [Artifacts](../../docs/deploy/artifacts.md) in your **Project Details** page, which you can find by selecting your account name on the left side menu in dbt and clicking **Account settings**. You can see the current status for source freshness by clicking **View Sources** in the job page.
