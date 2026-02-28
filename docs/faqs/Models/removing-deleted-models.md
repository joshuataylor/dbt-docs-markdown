# How do I remove deleted models from my data warehouse?

If you delete a model from your dbt project, dbt does not automatically drop the relation from your schema. This means that you can end up with extra objects in schemas that dbt creates, which can be confusing to other users.

(This can also happen when you switch a model from being a view or table, to ephemeral)

When you remove models from your dbt project, you should manually drop the related relations from your schema.

## Was this page helpful?

YesNo

[Privacy policy](https://www.getdbt.com/cloud/privacy-policy)[Create a GitHub issue](https://github.com/dbt-labs/docs.getdbt.com/issues)

This site is protected by reCAPTCHA and the Google [Privacy Policy](https://policies.google.com/privacy) and [Terms of Service](https://policies.google.com/terms) apply.
