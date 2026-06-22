# column\_types

## Description[​](#description "Direct link to Description")

Optionally specify the database type of columns in a [seed](https://docs.getdbt.com/docs/build/seeds.md), by providing a dictionary where the keys are the column names, and the values are a valid datatype (this varies across databases).

Without specifying this, dbt will infer the datatype based on the column values in your seed file.

## Usage[​](#usage "Direct link to Usage")

Specify column types in your `dbt_project.yml` file:

dbt\_project.yml

```yml
seeds:
  jaffle_shop:
    country_codes:
      +column_types:
        country_code: varchar(2)
        country_name: varchar(32)
```

Or:

seeds/properties.yml

```yml

seeds:
  - name: country_codes
    config:
      column_types:
        country_code: varchar(2)
        country_name: varchar(32)
```

If you have previously run `dbt seed`, you'll need to run `dbt seed --full-refresh` for the changes to take effect.

Note that you will need to use the fully directory path of a seed when configuring `column_types`. For example, for a seed file at `seeds/marketing/utm_mappings.csv`, you will need to configure it like so:

dbt\_project.yml

```yml
seeds:
  jaffle_shop:
    marketing:
      utm_mappings:
        +column_types:
          ...
```

## Examples[​](#examples "Direct link to Examples")

### Use a varchar column type to preserve leading zeros in a zipcode[​](#use-a-varchar-column-type-to-preserve-leading-zeros-in-a-zipcode "Direct link to Use a varchar column type to preserve leading zeros in a zipcode")

dbt\_project.yml

```yml
seeds:
  jaffle_shop: # you must include the project name
    warehouse_locations:
      +column_types:
        zipcode: varchar(5)
```

## Recommendation[​](#recommendation "Direct link to Recommendation")

Use this configuration only when required, i.e. when the type inference is not working as expected. Otherwise you can omit this configuration.

## Troubleshooting[​](#troubleshooting "Direct link to Troubleshooting")

Note: The `column_types` configuration is case-sensitive, regardless of quoting configuration. If you specify a column as `Country_Name` in your Seed, you should reference it as `Country_Name`, and not `country_name`.

## Was this page helpful?

YesNo

[Privacy policy](https://www.getdbt.com/cloud/privacy-policy)[Create a GitHub issue](https://github.com/dbt-labs/docs.getdbt.com/issues)

This site is protected by reCAPTCHA and the Google [Privacy Policy](https://policies.google.com/privacy) and [Terms of Service](https://policies.google.com/terms) apply.
