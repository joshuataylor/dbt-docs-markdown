# One of my tests failed, how can I debug it?

To debug a failing test, find the SQL that dbt ran by:

* dbt:

  * Within the test output, click on the failed test, and then select "Details".

* dbt Core:

  * Open the file path returned as part of the error message.
  * Navigate to the `target/compiled/schema_tests` directory for all compiled test queries.

Copy the SQL into a query editor (in dbt, you can paste it into a new `Statement`), and run the query to find the records that failed.

## Was this page helpful?

YesNo

[Privacy policy](https://www.getdbt.com/cloud/privacy-policy)[Create a GitHub issue](https://github.com/dbt-labs/docs.getdbt.com/issues)

This site is protected by reCAPTCHA and the Google [Privacy Policy](https://policies.google.com/privacy) and [Terms of Service](https://policies.google.com/terms) apply.
