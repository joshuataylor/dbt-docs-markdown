# How does increasing job frequency affect cost reduction estimates?

Cost reduction metrics reflect how dbt optimizes compute costs by reusing existing results instead of running the same model again.

When you increase your job run frequency (for example, because performance improvements make it easier to schedule jobs more often), dbt has more opportunities to reuse models. As reuse increases, dbt optimizes more compute, which means your reported cost reductions may also increase.

This metric shows the efficiency impact of reuse within your current workload. It reflects the compute costs that dbt reduces by reusing models instead of rebuilding them, rather than showing your total warehouse spend reduction.

## Was this page helpful?

YesNo

[Privacy policy](https://www.getdbt.com/cloud/privacy-policy)[Create a GitHub issue](https://github.com/dbt-labs/docs.getdbt.com/issues)

This site is protected by reCAPTCHA and the Google [Privacy Policy](https://policies.google.com/privacy) and [Terms of Service](https://policies.google.com/terms) apply.
