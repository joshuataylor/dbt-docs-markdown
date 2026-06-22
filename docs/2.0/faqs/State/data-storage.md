# How is data stored in dbt State?

dbt State sends last-modified timestamps and SQL statements to dbt Labs servers. SQL statements are hashed and then discarded, so dbt Labs cannot see the contents after hashing. These hashes are used to identify whether a statement has changed by comparing them on each run.

## Was this page helpful?

YesNo

[Privacy policy](https://www.getdbt.com/cloud/privacy-policy)[Create a GitHub issue](https://github.com/dbt-labs/docs.getdbt.com/issues)

This site is protected by reCAPTCHA and the Google [Privacy Policy](https://policies.google.com/privacy) and [Terms of Service](https://policies.google.com/terms) apply.
