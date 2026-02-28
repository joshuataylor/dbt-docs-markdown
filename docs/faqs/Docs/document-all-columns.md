# Do I need to add a YAML entry for column for it to appear in the docs site?

Fortunately, no!

dbt will introspect your warehouse to generate a list of columns in each relation, and match it with the list of columns in your `.yml` files. As such, any undocumented columns will still appear in your documentation!

## Was this page helpful?

YesNo

[Privacy policy](https://www.getdbt.com/cloud/privacy-policy)[Create a GitHub issue](https://github.com/dbt-labs/docs.getdbt.com/issues)

This site is protected by reCAPTCHA and the Google [Privacy Policy](https://policies.google.com/privacy) and [Terms of Service](https://policies.google.com/terms) apply.
