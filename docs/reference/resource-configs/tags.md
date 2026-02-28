# tags

* Models
* Seeds
* Snapshots
* Saved queries
* Sources
* Exposures
* Tests

dbt\_project.yml

models/properties.yml

```yaml
models:
  - name: model_name
    config:
      tags: <string> | [<string>]
    columns:
      - name: column_name
        config:
          tags: <string> | [<string>] # changed to config in v1.10 and backported to 1.9
        data_tests:
          - test_name:
              config:
                tags: <string> | [<string>]
```

models/\<modelname>.sql

```sql
{{ config(
    tags="<string>" | ["<string>"]
) }}

select ...
```

dbt\_project.yml

seeds/properties.yml

```yaml
seeds:
  - name: seed_name
    config:
      tags: <string> | [<string>]
    columns:
      - name: column_name
        config:
          tags: <string> | [<string>] # changed to config in v1.10 and backported to 1.9
        data_tests:
          - test_name:
              config:
                tags: <string> | [<string>]
```

dbt\_project.yml

snapshots/\<filename>.sql

```sql
{% snapshot snapshot_name %}

{{ config(
    tags="<string>" | ["<string>"]
) }}

select ...

{% endsnapshot %}
```

dbt\_project.yml

models/semantic\_models.yml

```yaml
saved_queries:
  - name: saved_query_name
    config:
      tags: <string> | [<string>]
```

dbt\_project.yml

models/properties.yml

```yaml
sources:
  - name: source_name
    config:
      tags: <string> | [<string>] # changed to config in v1.10
    tables:
      - name: table_name
        config:
          tags: <string> | [<string>] # changed to config in v1.10
        columns:
          - name: column_name
            config:
              tags: <string> | [<string>] # changed to config in v1.10 and backported to 1.9
            data_tests:
              - test_name:
                  config:
                    tags: <string> | [<string>]
```

Note that for backwards compatibility, `tags` is supported as a top-level key for sources, but without the capabilities of config inheritance.

dbt\_project.yml

models/exposures.yml

```yaml
exposures:
  - name: exposure_name
    config:
      tags: <string> | [<string>] # changed to config in v1.10
```

Note that for backwards compatibility, `tags` is supported as a top-level key for exposures, but without the capabilities of config inheritance.

dbt\_project.yml

models/properties.yml

```yaml
models:
  - name: model_name
    columns:
      - name: column_name
        data_tests:
          - test_name:
              config:
                tags: <string> | [<string>]
```

tests/\<filename>.sql

```sql
{% test test_name() %}

{{ config(
    tags="<string>" | ["<string>"]
) }}

select ...

{% endtest %}
```

## Definition[​](#definition "Direct link to Definition")

Apply a tag (or list of tags) to a resource.

These tags can be used as part of the [resource selection syntax](https://docs.getdbt.com/reference/node-selection/syntax.md), when running the following commands:

* `dbt run --select tag:my_tag` — Run all models tagged with a specific tag.
* `dbt build --select tag:my_tag` — Build all resources tagged with a specific tag.
* `dbt seed --select tag:my_tag` — Seed all resources tagged with a specific tag.
* `dbt snapshot --select tag:my_tag` — Snapshot all resources tagged with a specific tag.
* `dbt test --select tag:my_tag` — Indirectly runs all tests associated with the models that are tagged.

#### Using tags with the `+` operator[​](#using-tags-with-the--operator "Direct link to using-tags-with-the--operator")

You can use the [`+` operator](https://docs.getdbt.com/reference/node-selection/graph-operators.md#the-plus-operator) to include upstream or downstream dependencies in your `tag` selection:

* `dbt run --select tag:my_tag+` — Run models tagged with `my_tag` and all their downstream dependencies.
* `dbt run --select +tag:my_tag` — Run models tagged with `my_tag` and all their upstream dependencies.
* `dbt run --select +tag:my_tag+` — Run models tagged with `my_tag`, their upstream dependencies, and their downstream dependencies.
* `dbt run --select tag:my_tag+ --exclude tag:exclude_tag` — Run models tagged with `my_tag` and their downstream dependencies, and exclude models tagged with `exclude_tag`, regardless of their dependencies.

Usage notes about tags

When using tags, consider the following:

* Each individual tag must be a string.
* Tags are additive across project hierarchy.
* Some resource types (like sources, exposures) require tags at the top level.

Refer to [usage notes](#usage-notes) for more information.

## Examples[​](#examples "Direct link to Examples")

The following examples show how to apply tags to resources in your project. You can configure tags in the `dbt_project.yml`, property files, or SQL files.

### Use tags to run parts of your project[​](#use-tags-to-run-parts-of-your-project "Direct link to Use tags to run parts of your project")

Apply tags in your `dbt_project.yml` as a single value or a string. In the following example, one of the models, the `jaffle_shop` model, is tagged with `contains_pii`.

dbt\_project.yml

```yml
models:
  jaffle_shop:
    +tags: "contains_pii"

    staging:
      +tags:
        - "hourly"

    marts:
      +tags:
        - "hourly"
        - "published"

    metrics:
      +tags:
        - "daily"
        - "published"
```

### Apply tags to models[​](#apply-tags-to-models "Direct link to Apply tags to models")

This section demonstrates applying tags to models in the `dbt_project.yml`, `schema.yml`, and SQL files.

To apply tags to a model in your `dbt_project.yml` file, you would add the following:

dbt\_project.yml

```yaml
models:
  jaffle_shop:
    +tags: finance # jaffle_shop model is tagged with 'finance'.
```

To apply tags to a model in your `models/` directory YAML property file, you would add the following using the `config` property:

models/stg\_customers.yml

```yaml
models:
  - name: stg_customers
    description: Customer data with basic cleaning and transformation applied, one row per customer.
    config:
      tags: ['santi'] # stg_customers.yml model is tagged with 'santi'.
    columns:
      - name: customer_id
        description: The unique key for each customer.
        data_tests:
          - not_null
          - unique
```

To apply tags to a model in your SQL file, you would add the following:

models/staging/stg\_payments.sql

```sql
{{ config(
    tags=["finance"] # stg_payments.sql model is tagged with 'finance'.
) }}

select ...
```

Run resources with specific tags (or exclude resources with specific tags) using the following commands:

```shell
# Run all models tagged "daily"
  dbt run --select tag:daily

# Run all models tagged "daily", except those that are tagged hourly
  dbt run --select tag:daily --exclude tag:hourly
```

### Apply tags to seeds[​](#apply-tags-to-seeds "Direct link to Apply tags to seeds")

dbt\_project.yml

```yml
seeds:
  jaffle_shop:
    utm_mappings:
      +tags: marketing
```

dbt\_project.yml

```yml
seeds:
  jaffle_shop:
    utm_mappings:
      +tags:
        - marketing
        - hourly
```

### Apply tags to saved queries[​](#apply-tags-to-saved-queries "Direct link to Apply tags to saved queries")

This following example shows how to apply a tag to a saved query in the `dbt_project.yml` file. The saved query is then tagged with `order_metrics`.

dbt\_project.yml

```yml
saved-queries:
  jaffle_shop:
    customer_order_metrics:
      +tags: order_metrics
```

Then run resources with a specific tag using the following commands:

```shell
# Run all resources tagged "order_metrics"
  dbt run --select tag:order_metrics
```

The second example shows how to apply multiple tags to a saved query in the `semantic_model.yml` file. The saved query is then tagged with `order_metrics` and `hourly`.

semantic\_model.yml

```yaml
saved_queries:
  - name: test_saved_query
    description: "{{ doc('saved_query_description') }}"
    label: Test saved query
    config:
      tags: 
        - order_metrics
        - hourly
```

Run resources with multiple tags using the following commands:

```shell
# Run all resources tagged "order_metrics" and "hourly"
  dbt build --select tag:order_metrics tag:hourly
```

## Usage notes[​](#usage-notes "Direct link to Usage notes")

### Tags must be strings[​](#tags-must-be-strings "Direct link to Tags must be strings")

Each individual tag must be a string value (for example, `marketing` or `daily`).

In the following example, `my_tag: "my_value"` is invalid because it is a key-value pair.

```yml
sources:
  - name: ecom
    schema: raw
    description: E-commerce data for the Jaffle Shop
    config:
      tags:
        my_tag: "my_value". # invalid
    tables:
      - name: raw_customers
        config:
          tags:
            my_tag: "my_value". # invalid
```

A warning is raised when the `tags` value is not a string. For example:

```text
Field config.tags: {'my_tag': 'my_value'} is not valid for source (ecom)
```

### Tags are additive[​](#tags-are-additive "Direct link to Tags are additive")

Tags accumulate hierarchically. The [earlier example](https://docs.getdbt.com/reference/resource-configs/tags.md#use-tags-to-run-parts-of-your-project) would result in:

| Model                             | Tags                                  |
| --------------------------------- | ------------------------------------- |
| models/staging/stg\_customers.sql | `contains_pii`, `hourly`              |
| models/staging/stg\_payments.sql  | `contains_pii`, `hourly`, `finance`   |
| models/marts/dim\_customers.sql   | `contains_pii`, `hourly`, `published` |
| models/metrics/daily\_metrics.sql | `contains_pii`, `daily`, `published`  |

Search table...

|                  |   |   |   |   |
| ---------------- | - | - | - | - |
| Loading table... |   |   |   |   |

### Applying tags to specific columns and tests[​](#applying-tags-to-specific-columns-and-tests "Direct link to Applying tags to specific columns and tests")

You can also apply tags to specific columns in a resource, and to tests.

models/properties.yml

```yml
models:
  - name: my_model
    columns:
      - name: column_name
        config:
          tags: ['column_level'] # changed to config in v1.10 and backported to 1.9
        data_tests:
          - unique:
              config:
                tags: ['test_level'] # changed to config in v1.10
```

In the example above, the `unique` test would be selected by either of these tags:

```bash
dbt test --select tag:column_level
dbt test --select tag:test_level
```

### Backwards compatibility for sources and exposures[​](#backwards-compatibility-for-sources-and-exposures "Direct link to Backwards compatibility for sources and exposures")

For backwards compatibility, `tags` is supported as a top-level key for sources and exposures (prior to dbt v1.10), but without the capabilities of config inheritance.

models/properties.yml

```yml
exposures:
  - name: my_exposure
    tags: ['exposure_tag'] # top-level key (legacy)
    # OR use config (v1.10+)
    config:
      tags: ['exposure_tag']

sources:
  - name: source_name
    tags: ['top_level'] # top-level key (legacy)
    # OR use config (v1.10+)
    config:
      tags: ['top_level']
    tables:
      - name: table_name
        tags: ['table_level'] # top-level key (legacy)
        # OR use config (v1.10+)
        config:
          tags: ['table_level']
        columns:
          - name: column_name
            config:
              tags: ['column_level'] # changed to config in v1.10 and backported to 1.9
```

## Was this page helpful?

YesNo

[Privacy policy](https://www.getdbt.com/cloud/privacy-policy)[Create a GitHub issue](https://github.com/dbt-labs/docs.getdbt.com/issues)

This site is protected by reCAPTCHA and the Google [Privacy Policy](https://policies.google.com/privacy) and [Terms of Service](https://policies.google.com/terms) apply.
