# Can I add tests and descriptions in a SQL config block?

dbt has the ability to define node configs in YAML files, in addition to `config()` blocks and `dbt_project.yml`. But the reverse isn't always true: there are some things in `.yml` files that can *only* be defined there.

Certain properties are special, because:

* They have a unique Jinja rendering context
* They create new project resources
* They don't make sense as hierarchical configuration
* They're older properties that haven't yet been redefined as configs

These properties are:

* [`description`](https://docs.getdbt.com/reference/resource-properties/description.md)
* [`tests`](https://docs.getdbt.com/reference/resource-properties/data-tests.md)
* [`docs`](https://docs.getdbt.com/reference/resource-configs/docs.md)
* `columns`
* [`quote`](https://docs.getdbt.com/reference/resource-properties/columns.md#quote)
* [`source` properties](https://docs.getdbt.com/reference/source-properties.md) (e.g. `loaded_at_field`, `freshness`)
* [`exposure` properties](https://docs.getdbt.com/reference/exposure-properties.md) (e.g. `type`, `maturity`)
* [`macro` properties](https://docs.getdbt.com/reference/resource-properties/arguments.md) (e.g. `arguments`)

## Was this page helpful?

YesNo

[Privacy policy](https://www.getdbt.com/cloud/privacy-policy)[Create a GitHub issue](https://github.com/dbt-labs/docs.getdbt.com/issues)

This site is protected by reCAPTCHA and the Google [Privacy Policy](https://policies.google.com/privacy) and [Terms of Service](https://policies.google.com/terms) apply.
