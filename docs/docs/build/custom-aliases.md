# Custom aliases

## Overview[​](#overview "Direct link to Overview")

When dbt runs a model, it will generally create a relation (either a table or a view ) in the database, except in the case of an [ephemeral model](https://docs.getdbt.com/docs/build/materializations.md), when it will create a CTE for use in another model. By default, dbt uses the model's filename as the identifier for the relation or CTE it creates. This identifier can be overridden using the [`alias`](https://docs.getdbt.com/reference/resource-configs/alias.md) model configuration.

### Why alias model names?[​](#why-alias-model-names "Direct link to Why alias model names?")

The names of schemas and tables are effectively the "user interface" of your data warehouse. Well-named schemas and tables can help provide clarity and direction for consumers of this data. In combination with [custom schemas](https://docs.getdbt.com/docs/build/custom-schemas.md), model aliasing is a powerful mechanism for designing your warehouse.

The file naming scheme that you use to organize your models may also interfere with your data platform's requirements for identifiers. For example, you might wish to namespace your files using a period (`.`), but your data platform's SQL dialect may interpret periods to indicate a separation between schema names and table names in identifiers, or it may forbid periods from being used at all in CTE identifiers. In cases like these, model aliasing can allow you to retain flexibility in the way you name your model files without violating your data platform's identifier requirements.

### Usage[​](#usage "Direct link to Usage")

The `alias` config can be used to change the name of a model's identifier in the database. The following table shows examples of database identifiers for models both with and without a supplied `alias`, and with different materializations.

| Model            | Config                                                      | Relation Type | Database Identifier              |
| ---------------- | ----------------------------------------------------------- | ------------- | -------------------------------- |
| ga\_sessions.sql | {{ config(materialization='view') }}                        | view          | "analytics"."ga\_sessions"       |
| ga\_sessions.sql | {{ config(materialization='view', alias='sessions') }}      | view          | "analytics"."sessions"           |
| ga\_sessions.sql | {{ config(materialization='ephemeral') }}                   | CTE           | "\_\_dbt\_\_cte\_\_ga\_sessions" |
| ga\_sessions.sql | {{ config(materialization='ephemeral', alias='sessions') }} | CTE           | "\_\_dbt\_\_cte\_\_sessions"     |

Search table...

|                  |   |   |   |   |
| ---------------- | - | - | - | - |
| Loading table... |   |   |   |   |

To configure an alias for a model, supply a value for the model's `alias` configuration parameter. For example:

models/google\_analytics/ga\_sessions.sql

```sql

-- This model will be created in the database with the identifier `sessions`
-- Note that in this example, `alias` is used along with a custom schema
{{ config(alias='sessions', schema='google_analytics') }}

select * from ...
```

Or in a `schema.yml` file.

models/google\_analytics/schema.yml

```yaml
models:
  - name: ga_sessions
    config:
      alias: sessions
```

When referencing the `ga_sessions` model above from a different model, use the `ref()` function with the model's *filename* as usual. For example:

models/combined\_sessions.sql

```sql

-- Use the model's filename in ref's, regardless of any aliasing configs

select * from {{ ref('ga_sessions') }}
union all
select * from {{ ref('snowplow_sessions') }}
```

### generate\_alias\_name[​](#generate_alias_name "Direct link to generate_alias_name")

The alias generated for a model is controlled by a macro called `generate_alias_name`. This macro can be overridden in a dbt project to change how dbt aliases models. This macro works similarly to the [generate\_schema\_name](https://docs.getdbt.com/docs/build/custom-schemas.md#advanced-custom-schema-configuration) macro.

To override dbt's alias name generation, create a macro named `generate_alias_name` in your own dbt project. The `generate_alias_name` macro accepts two arguments:

1. The custom alias supplied in the model config
2. The node that a custom alias is being generated for

The default implementation of `generate_alias_name` simply uses the supplied `alias` config (if present) as the model alias, otherwise falling back to the model name. This implementation looks like this:

get\_custom\_alias.sql

```jinja2
{% macro generate_alias_name(custom_alias_name=none, node=none) -%}

    {%- if custom_alias_name -%}

        {{ custom_alias_name | trim }}

    {%- elif node.version -%}

        {{ return(node.name ~ "_v" ~ (node.version | replace(".", "_"))) }}

    {%- else -%}

        {{ node.name }}

    {%- endif -%}

{%- endmacro %}
```

<!-- -->

💡 Use Jinja's whitespace control to tidy your macros!

When you're modifying macros in your project, you might notice extra white space in your code in the `target/compiled` folder.

You can remove unwanted spaces and lines with Jinja's [whitespace control](https://docs.getdbt.com/faqs/Jinja/jinja-whitespace.md) by using a minus sign. For example, use `{{- ... -}}` or `{%- ... %}` around your macro definitions (such as `{%- macro generate_schema_name(...) -%} ... {%- endmacro -%}`).

### Dispatch macro - SQL alias management for databases and dbt packages[​](#dispatch-macro---sql-alias-management-for-databases-and-dbt-packages "Direct link to Dispatch macro - SQL alias management for databases and dbt packages")

See docs on macro `dispatch`: ["Managing different global overrides across packages"](https://docs.getdbt.com/reference/dbt-jinja-functions/dispatch.md#managing-different-global-overrides-across-packages)

### Caveats[​](#caveats "Direct link to Caveats")

#### Ambiguous database identifiers[​](#ambiguous-database-identifiers "Direct link to Ambiguous database identifiers")

Using aliases, it's possible to accidentally create models with ambiguous identifiers. Given the following two models, dbt would attempt to create two views with *exactly* the same names in the database (ie. `sessions`):

models/snowplow\_sessions.sql

```sql
{{ config(alias='sessions') }}

select * from ...
```

models/sessions.sql

```sql
select * from ...
```

Whichever one of these models runs second would "win", and generally, the output of dbt would not be what you would expect. To avoid this failure mode, dbt will check if your model names and aliases are ambiguous in nature. If they are, you will be presented with an error message like this:

```text
$ dbt compile
Encountered an error:
Compilation Error
  dbt found two resources with the database representation "analytics.sessions".
  dbt cannot create two resources with identical database representations. To fix this,
  change the "schema" or "alias" configuration of one of these resources:
  - model.my_project.snowplow_sessions (models/snowplow_sessions.sql)
  - model.my_project.sessions (models/sessions.sql)
```

If these models should indeed have the same database identifier, you can work around this error by configuring a [custom schema](https://docs.getdbt.com/docs/build/custom-schemas.md) for one of the models.

#### Model versions[​](#model-versions "Direct link to Model versions")

**Related documentation:**

* [Model versions](https://docs.getdbt.com/docs/mesh/govern/model-versions.md)
* [`versions`](https://docs.getdbt.com/reference/resource-properties/versions.md#alias)

By default, dbt will create versioned models with the alias `<model_name>_v<v>`, where `<v>` is that version's unique identifier. You can customize this behavior just like for non-versioned models by configuring a custom `alias` or re-implementing the `generate_alias_name` macro.

## Related docs[​](#related-docs "Direct link to Related docs")

* [Customize dbt models database, schema, and alias](https://docs.getdbt.com/guides/customize-schema-alias.md?step=1) to learn how to customize dbt models database, schema, and alias
* [Custom schema](https://docs.getdbt.com/docs/build/custom-schemas.md) to learn how to customize dbt schema
* [Custom database](https://docs.getdbt.com/docs/build/custom-databases.md) to learn how to customize dbt database

## Was this page helpful?

YesNo

[Privacy policy](https://www.getdbt.com/cloud/privacy-policy)[Create a GitHub issue](https://github.com/dbt-labs/docs.getdbt.com/issues)

This site is protected by reCAPTCHA and the Google [Privacy Policy](https://policies.google.com/privacy) and [Terms of Service](https://policies.google.com/terms) apply.
