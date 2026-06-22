# Does dbt State support incremental models?

dbt State works with incremental models. When you make a change to an incremental model and run it in development, dbt State automatically clones the model from production if it exists, then runs the new model logic on top of the clone.

If you want to revert to the original dbt behavior and fully refresh the incremental model, pass the [`--full-refresh` flag](https://docs.getdbt.com/reference/commands/run.md#refresh-incremental-models).

## Was this page helpful?

YesNo

[Privacy policy](https://www.getdbt.com/cloud/privacy-policy)[Create a GitHub issue](https://github.com/dbt-labs/docs.getdbt.com/issues)

This site is protected by reCAPTCHA and the Google [Privacy Policy](https://policies.google.com/privacy) and [Terms of Service](https://policies.google.com/terms) apply.
