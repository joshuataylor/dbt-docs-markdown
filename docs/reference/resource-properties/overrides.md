# overrides

Deprecation

The `overrides` property is [deprecated in v1.10](https://docs.getdbt.com/reference/deprecations.md#sourceoverridedeprecation). You can [enable or disable a source](https://docs.getdbt.com/reference/source-configs.md#configuring-sources) from a package instead.

models/\<filename>.yml

```yml

sources:
  - name: <source_name>
    overrides: <package name> # deprecated in v1.10

    database: ...
    schema: ...
```

## Definition[​](#definition "Direct link to Definition")

Override a source defined in an included package. The properties defined in the overriding source will be applied on top of the base properties of the overridden source.

The following source properties can be overridden:

* [description](https://docs.getdbt.com/reference/resource-properties/description.md)
* [meta](https://docs.getdbt.com/reference/resource-configs/meta.md)
* [database](https://docs.getdbt.com/reference/resource-properties/database.md)
* [schema](https://docs.getdbt.com/reference/resource-properties/schema.md)
* [loader](https://docs.getdbt.com/reference/resource-properties/loader.md)
* [quoting](https://docs.getdbt.com/reference/resource-properties/quoting.md)
* [freshness](https://docs.getdbt.com/reference/resource-properties/freshness.md)
* [loaded\_at\_field](https://docs.getdbt.com/reference/resource-properties/freshness.md#loaded_at_field)
* [tags](https://docs.getdbt.com/reference/resource-configs/tags.md)

## Examples[​](#examples "Direct link to Examples")

### Supply your database and schema name for a source defined in a package[​](#supply-your-database-and-schema-name-for-a-source-defined-in-a-package "Direct link to Supply your database and schema name for a source defined in a package")

This example is based on the [Fivetran GitHub Source package](https://github.com/fivetran/dbt_github_source/blob/830ba43ac2948e4853a3c167ab7ee88b8b425fa0/models/src_github.yml#L3-L29). Here, the database and schema are overridden in the parent dbt project which includes the `github_source` package.

models/src\_github.yml

```yml

sources:
  - name: github
    overrides: github_source # deprecated in v1.10

    database: RAW
    schema: github_data
```

### Configure your own source freshness for a source table in a package[​](#configure-your-own-source-freshness-for-a-source-table-in-a-package "Direct link to Configure your own source freshness for a source table in a package")

You can override configurations at both the source and the table level

models/src\_github.yml

```yml

sources:
  - name: github
    overrides: github_source # deprecated in v1.10
    config:
      freshness: # changed to config in v1.9
        warn_after:
          count: 1
          period: day
        error_after:
          count: 2
          period: day

    tables:
      - name: issue_assignee
        config:
          freshness:
            warn_after:
              count: 2
              period: day
            error_after:
              count: 4
              period: day
```

## Was this page helpful?

YesNo

[Privacy policy](https://www.getdbt.com/cloud/privacy-policy)[Create a GitHub issue](https://github.com/dbt-labs/docs.getdbt.com/issues)

This site is protected by reCAPTCHA and the Google [Privacy Policy](https://policies.google.com/privacy) and [Terms of Service](https://policies.google.com/terms) apply.
