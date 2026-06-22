# Receiving a 'Could not parse dbt\_project.yml' error in dbt job

The error message `Could not parse dbt_project.yml: while scanning for...` in your dbt job run or development usually occurs for several reasons:

* There's a parsing failure in a YAML file (such as a tab indentation or Unicode characters).
* Your `dbt_project.yml` file has missing fields or incorrect formatting.
* Your `dbt_project.yml` file doesn't exist in your dbt project repository.

To resolve this issue, consider the following:

* Use an online YAML parser or validator to check for any parsing errors in your YAML file. Some known parsing errors include missing fields, incorrect formatting, or tab indentation.
* Or ensure your `dbt_project.yml` file exists.

Once you've identified the issue, you can fix the error and rerun your dbt job.

## Was this page helpful?

YesNo

[Privacy policy](https://www.getdbt.com/cloud/privacy-policy)[Create a GitHub issue](https://github.com/dbt-labs/docs.getdbt.com/issues)

This site is protected by reCAPTCHA and the Google [Privacy Policy](https://policies.google.com/privacy) and [Terms of Service](https://policies.google.com/terms) apply.
