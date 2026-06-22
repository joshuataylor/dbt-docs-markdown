# loader

models/\<filename>.yml

```yml

sources:
  - name: <source_name>
    database: <database_name>
    loader: <string>
    tables:
      - ...
```

## Definition[​](#definition "Direct link to Definition")

Describe the tool that loads this source into your warehouse. Note that this property is for documentation purposes only — dbt does not meaningfully use this.

## Examples[​](#examples "Direct link to Examples")

### Indicate which EL tool loaded data[​](#indicate-which-el-tool-loaded-data "Direct link to Indicate which EL tool loaded data")

models/\<filename>.yml

```yml

sources:
  - name: jaffle_shop
    loader: fivetran
    tables:
      - name: orders
      - name: customers

  - name: stripe
    loader: stitch
    tables:
      - name: payments
```

## Was this page helpful?

YesNo

[Privacy policy](https://www.getdbt.com/cloud/privacy-policy)[Create a GitHub issue](https://github.com/dbt-labs/docs.getdbt.com/issues)

This site is protected by reCAPTCHA and the Google [Privacy Policy](https://policies.google.com/privacy) and [Terms of Service](https://policies.google.com/terms) apply.
