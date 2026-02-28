# If models can only be \`select\` statements, how do I insert records?

For those coming from an ETL (Extract Transform Load) paradigm, there's often a desire to write transformations as `insert` and `update` statements. In comparison, dbt will wrap your `select` query in a `create table as` statement, which can feel counter-productive.

* If you wish to use `insert` statements for performance reasons (i.e. to reduce data that is processed), consider [incremental models](https://docs.getdbt.com/docs/build/incremental-models.md)
* If you wish to use `insert` statements since your source data is constantly changing (e.g. to create "Type 2 Slowly Changing Dimensions"), consider [snapshotting your source data](https://docs.getdbt.com/docs/build/sources.md#source-data-freshness), and building models on top of your snaphots.

## Was this page helpful?

YesNo

[Privacy policy](https://www.getdbt.com/cloud/privacy-policy)[Create a GitHub issue](https://github.com/dbt-labs/docs.getdbt.com/issues)

This site is protected by reCAPTCHA and the Google [Privacy Policy](https://policies.google.com/privacy) and [Terms of Service](https://policies.google.com/terms) apply.
