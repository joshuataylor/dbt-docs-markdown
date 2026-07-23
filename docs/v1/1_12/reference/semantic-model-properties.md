# Semantic model properties

Semantic models define the structure that MetricFlow uses to build the semantic graph. In the *latest spec*, they can be declared as a top-level `semantic_model:` block on a [model](https://docs.getdbt.com/reference/model-properties.md). In the *legacy spec*, we used standalone YAML. For more information, refer to [Semantic models](https://docs.getdbt.com/docs/build/semantic-models.md).

<!-- -->

Availability

The latest YAML spec is supported in the following environments:

* **dbt platform (Latest release track)**
* **dbt Fusion engine**
* **dbt Core v1.12**

For more information, refer to [Migrate to the latest YAML spec](https://docs.getdbt.com/docs/build/latest-metrics-spec.md).

## Latest spec (model YAML)[​](#latest-spec-model-yaml "Direct link to Latest spec (model YAML)")

In the latest spec, a model defines a semantic model with the `semantic_model` key, and that value can be:

* **`true` or `false`**: Boolean shorthand. `true` enables a semantic model and uses the **model’s name** as the semantic model name (and other defaults where fields are unset). `false` turns off the semantic model for this model.
* A **mapping**: An object with `enabled`, optional `name`, `group`, `config`, and so on, as in the table below.

Dimensions and entities are defined with a column.

### Available semantic model properties (latest spec)[​](#available-semantic-model-properties-latest-spec "Direct link to Available semantic model properties (latest spec)")

| Property / location              | Type              | Required                              | Description                                                                                                                                                                                                                  |
| -------------------------------- | ----------------- | ------------------------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `semantic_model`                 | boolean or object | When using semantic layer for a model | `true` / `false`, or a mapping. When using a mapping, optional keys are in the following rows.                                                                                                                               |
| `semantic_model.enabled`         | boolean           | No (mapping form)                     | Defaults to `true` when `semantic_model` is a mapping. Set `false` to disable. Omit when using `semantic_model: true` or `false`.                                                                                            |
| `semantic_model.name`            | string            | No                                    | Unique name; defaults to model name. Avoid double underscores (`__`) in the name; see [Name](https://docs.getdbt.com/docs/build/semantic-models.md#name) in **Semantic models**.                                             |
| `semantic_model.group`           | string            | No                                    | Group for organization.                                                                                                                                                                                                      |
| `semantic_model.config`          | object            | No                                    | Supports [meta](https://docs.getdbt.com/reference/resource-configs/meta.md), [group](https://docs.getdbt.com/reference/resource-configs/group.md), [enabled](https://docs.getdbt.com/reference/resource-configs/enabled.md). |
| `agg_time_dimension` (top-level) | string            | Yes                                   | Default time dimension for metrics; references dimension name.                                                                                                                                                               |
| `primary_entity` (top-level)     | string            | No                                    | Primary entity name when no column has `type: primary`.                                                                                                                                                                      |
| `columns`                        | array             | Yes                                   | Column list; each can have `entity:` or `dimension:` block; time dimensions have `granularity` on the column.                                                                                                                |
| `derived_semantics` (top-level)  | object            | No                                    | Optional dimensions and entities with `expr`.                                                                                                                                                                                |
| `metrics` (top-level)            | array             | No                                    | Metrics derived from this semantic model; list is alongside (not under) `semantic_model`.                                                                                                                                    |

Search table...

|                  |   |   |   |   |
| ---------------- | - | - | - | - |
| Loading table... |   |   |   |   |

### Minimal structure example[​](#minimal-structure-example "Direct link to Minimal structure example")

```yaml
models:
  - name: my_model
    semantic_model:
      enabled: true    # explicit; defaults to true if omitted in mapping form
      name: optional_override   # optional; defaults to model name
      group: optional_group
      config:
        meta: {}
    agg_time_dimension: my_time_dimension   # top-level; references dimension name
    primary_entity: my_primary_entity       # optional; use when no column has type: primary
    columns:
      - name: my_entity_column
        entity:
          type: primary | foreign | unique | natural
          name: optional_entity_name
      - name: my_time_dimension_column
        granularity: day    # required for time dimensions
        dimension:
          type: time
          name: my_time_dimension
      - name: my_categorical_dimension_column
        dimension:
          type: categorical
          name: my_categorical_dimension
    derived_semantics:    # optional
      dimensions: []
      entities: []
    metrics:
      - name: my_simple_metric
        type: simple
        agg: count
        expr: optional_expr
```

### Example with boolean instead of mapping[​](#example-with-boolean-instead-of-mapping "Direct link to Example with boolean instead of mapping")

Same layout as previous example, but with a boolean instead of a `semantic_model:` mapping (semantic model name defaults to the model name):

```yaml
models:
  - name: my_model
    semantic_model: true
    agg_time_dimension: my_time_dimension
    primary_entity: my_primary_entity       # optional
    columns:
      - name: my_entity_column
        entity:
          type: primary | foreign | unique | natural
      - name: my_time_dimension_column
        granularity: day
        dimension:
          type: time
          name: my_time_dimension
    metrics:
      - name: my_simple_metric
        type: simple
        agg: count
```

For the latest spec (model-embedded form with top-level `semantic_model:` and `metrics:` on the model), see [Semantic models](https://docs.getdbt.com/docs/build/semantic-models.md).
