# How do I populate the owner column in the generated docs?

Due to the nature of the field, you won't be able to change the owner column in your generated documentation.

The *owner* field in `dbt-docs` is pulled from database metadata (`catalog.json`), meaning the owner of that table in the database. With the exception of exposures, it's not pulled from an `owner` field set within dbt.

Generally, dbt's database user owns the tables created in the database. Source tables are usually owned by the service responsible for ingesting/loading them.

If you set `meta.owner`, you should now be seeing that field appear under **meta** (pulled from dbt), but still not under the top-level **owner** field.

## Was this page helpful?

YesNo

[Privacy policy](https://www.getdbt.com/cloud/privacy-policy)[Create a GitHub issue](https://github.com/dbt-labs/docs.getdbt.com/issues)

This site is protected by reCAPTCHA and the Google [Privacy Policy](https://policies.google.com/privacy) and [Terms of Service](https://policies.google.com/terms) apply.
