# I'm receiving a 'This run exceeded your account's run memory limits' error in my failed job

If you're receiving a `This run exceeded your account's run memory limits` error in your failed job, it means that the job exceeded the [memory limits](https://docs.getdbt.com/docs/deploy/job-scheduler.md#job-memory) set for your account. All dbt accounts have a pod memory of 600Mib and memory limits are on a per run basis. They're typically influenced by the amount of result data that dbt has to ingest and process, which is small but can become bloated unexpectedly by project design choices.

### Common reasons[​](#common-reasons "Direct link to Common reasons")

Some common reasons for higher memory usage are:

* dbt run/build: Macros that capture large result sets from run query may not all be necessary and may be memory inefficient.
* dbt docs generate: Source or model schemas with large numbers of tables (even if those tables aren't all used by dbt) cause the ingest of very large results for catalog queries.

### Resolution[​](#resolution "Direct link to Resolution")

There are various reasons why you could be experiencing this error but they are mostly the outcome of retrieving too much data back into dbt. For example, using the `run_query()` operations or similar macros, or even using database/schemas that have a lot of other non-dbt related tables/views. Try to reduce the amount of data / number of rows retrieved back into dbt by refactoring the SQL in your `run_query()` operation using `group`, `where`, or `limit` clauses. Additionally, you can also use a database/schema with fewer non-dbt related tables/views.

Video example

As an additional resource, check out [this example video](https://www.youtube.com/watch?v=sTqzNaFXiZ8), which demonstrates how to refactor the sample code by reducing the number of rows returned.

If you've tried the earlier suggestions and are still experiencing failed job runs with this error about hitting the memory limits of your account, please [reach out to support](mailto:support@getdbt.com). We're happy to help!

### Additional resources[​](#additional-resources "Direct link to Additional resources")

* [Blog post on how we shaved 90 mins off](https://docs.getdbt.com/blog/how-we-shaved-90-minutes-off-model)

## Was this page helpful?

YesNo

[Privacy policy](https://www.getdbt.com/cloud/privacy-policy)[Create a GitHub issue](https://github.com/dbt-labs/docs.getdbt.com/issues)

This site is protected by reCAPTCHA and the Google [Privacy Policy](https://policies.google.com/privacy) and [Terms of Service](https://policies.google.com/terms) apply.
