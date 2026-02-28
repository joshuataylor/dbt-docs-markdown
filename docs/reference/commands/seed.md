# About dbt seed command

The `dbt seed` command will load `csv` files located in the `seed-paths` directory of your dbt project into your data warehouse.

### Selecting seeds to run[​](#selecting-seeds-to-run "Direct link to Selecting seeds to run")

Specific seeds can be run using the `--select` flag to `dbt seed`. Example:

```text
$ dbt seed --select "country_codes"
Found 2 models, 3 tests, 0 archives, 0 analyses, 53 macros, 0 operations, 2 seed files

14:46:15 | Concurrency: 1 threads (target='dev')
14:46:15 |
14:46:15 | 1 of 1 START seed file analytics.country_codes........................... [RUN]
14:46:15 | 1 of 1 OK loaded seed file analytics.country_codes....................... [INSERT 3 in 0.01s]
14:46:16 |
14:46:16 | Finished running 1 seed in 0.14s.
```

For information about configuring seeds (for example, column types and quoting behavior), see [Seed configurations](https://docs.getdbt.com/reference/seed-configs.md).

## Was this page helpful?

YesNo

[Privacy policy](https://www.getdbt.com/cloud/privacy-policy)[Create a GitHub issue](https://github.com/dbt-labs/docs.getdbt.com/issues)

This site is protected by reCAPTCHA and the Google [Privacy Policy](https://policies.google.com/privacy) and [Terms of Service](https://policies.google.com/terms) apply.
