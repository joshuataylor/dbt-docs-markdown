💡Did you know\...

Available from dbt v

<!-- -->

1.12

<!-- -->

or with the

<!-- -->

[dbt "Latest" release track](https://docs.getdbt.com/docs/dbt-versions/dbt-release-tracks.md).

# latest\_version\_pointer [Beta](https://docs.getdbt.com/docs/dbt-versions/product-lifecycles "Go to https://docs.getdbt.com/docs/dbt-versions/product-lifecycles")

* Project file
* Property file
* SQL config

dbt\_project.yml

```yaml
models:
  <resource-path>:
    +latest_version_pointer:
      enabled: true | false
      alias: <string>
```

models/schema.yml

```yaml
models:
  - name: [<model-name>]
    config:
      latest_version_pointer:
        enabled: true | false
        alias: <string>
```

models/\<model\_name>.sql

```sql
{{ config(
    latest_version_pointer={
        "enabled": true | false,
        "alias": "<string>"
    }
) }}
```

## Definition[​](#definition "Direct link to Definition")

Beta feature

The `latest_version_pointer` config is a beta feature in dbt Core v1.12.

The `latest_version_pointer` config creates a view named after a [versioned model's](https://docs.getdbt.com/docs/mesh/govern/model-versions.md) base name (for example, `dim_customers`) that always points to the latest versioned relation (for example, `dim_customers_v2`). The view is created after the model with `is_latest_version = true` materializes successfully and is skipped for all other versions.

You can also enable this feature globally for all versioned models by setting the [`latest_version_pointer_enabled_by_default`](https://docs.getdbt.com/reference/global-configs/behavior-flag-introduction.md#latest-version-pointer-for-versioned-models) flag to `true` in `dbt_project.yml`:

dbt\_project.yml

```yaml
flags:
  latest_version_pointer_enabled_by_default: true
```

`latest_version_pointer` accepts two optional sub-keys for per-model control:

| Key       | Type    | Default         | Description                                                                                                                                                            |
| --------- | ------- | --------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `enabled` | boolean | —               | Enables or disables the pointer view for this model. Overrides the `latest_version_pointer_enabled_by_default` project flag when set; defers to the flag when not set. |
| `alias`   | string  | Model base name | Custom name for the pointer view. Overrides the default base name. For more information, see [Alias customization](#alias-customization).                              |

Search table...

|                  |   |   |   |   |
| ---------------- | - | - | - | - |
| Loading table... |   |   |   |   |

## Alias customization[​](#alias-customization "Direct link to Alias customization")

By default, the pointer view uses the model's base name (for example, `dim_customers`). You can customize this in two ways:

* **Per model**: Set `latest_version_pointer.alias` in the model config.
* **Globally**: Override the [`generate_latest_version_pointer_alias`](https://docs.getdbt.com/docs/build/custom-aliases.md#generate_latest_version_pointer_alias) macro in your project. This macro follows the same pattern as [`generate_alias_name`](https://docs.getdbt.com/docs/build/custom-aliases.md#generate_alias_name).

To prevent naming collisions, dbt raises an error if the latest version's alias is the same as the pointer name. For example, the following configuration would cause an error because both `dim_customers_v2` and the pointer view would resolve to `dim_customers`:

```yaml
models:
  - name: dim_customers
    versions:
      - v: 1
      - v: 2
        config:
          alias: dim_customers  # collides with the pointer view name
    config:
      latest_version_pointer:
        enabled: true
```

To fix this, select one of the following options:

* [Remove the `alias` (recommended)](#remove-the-alias-recommended)
* [Disable the latest version pointer for that model](#disable-the-latest-version-pointer-for-that-model)
* [Set a unique `alias`](#set-a-unique-alias)
* [Override the `generate_latest_version_pointer_alias` macro](#override-the-generate_latest_version_pointer_alias-macro)

#### Remove the `alias` (recommended)[​](#remove-the-alias-recommended "Direct link to remove-the-alias-recommended")

Remove the `alias` from the latest version and let the automatic pointer handle it:

```yaml
config:
  alias: dim_customers
```

#### Disable the latest version pointer for that model[​](#disable-the-latest-version-pointer-for-that-model "Direct link to Disable the latest version pointer for that model")

This approach is immediately backward-compatible for pre-existing `alias` configs:

```yaml
        config:
          alias: dim_customers
    config:
      latest_version_pointer:
        enabled: false
```

#### Set a unique `alias`[​](#set-a-unique-alias "Direct link to set-a-unique-alias")

```yaml
        config:
          alias: dim_customers_latest
```

#### Override the `generate_latest_version_pointer_alias` macro[​](#override-the-generate_latest_version_pointer_alias-macro "Direct link to override-the-generate_latest_version_pointer_alias-macro")

Override the [`generate_latest_version_pointer_alias`](https://docs.getdbt.com/docs/build/custom-aliases.md#generate_latest_version_pointer_alias) macro to use a different naming convention globally:

macros/generate\_latest\_version\_pointer\_alias.sql

```sql
{% macro generate_latest_version_pointer_alias(custom_alias_name=none, node=none) -%}
    {{ node.name ~ "_latest" }}
{%- endmacro %}
```

## Related documentation[​](#related-documentation "Direct link to Related documentation")

* [Model versions](https://docs.getdbt.com/docs/mesh/govern/model-versions.md)
* [Pointing to the latest version](https://docs.getdbt.com/docs/mesh/govern/model-versions.md#pointing-to-the-latest-version)
* [`latest_version_pointer_enabled_by_default` flag](https://docs.getdbt.com/reference/global-configs/behavior-flag-introduction.md#latest-version-pointer-for-versioned-models)
* [`versions`](https://docs.getdbt.com/reference/resource-properties/versions.md)
* [`latest_version`](https://docs.getdbt.com/reference/resource-properties/latest_version.md)

## Was this page helpful?

YesNo

[Privacy policy](https://www.getdbt.com/cloud/privacy-policy)[Create a GitHub issue](https://github.com/dbt-labs/docs.getdbt.com/issues)

This site is protected by reCAPTCHA and the Google [Privacy Policy](https://policies.google.com/privacy) and [Terms of Service](https://policies.google.com/terms) apply.
