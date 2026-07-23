# Semantic model properties

Semantic models define the structure that MetricFlow uses to build the semantic graph. In the *latest spec*, they can be declared as a top-level `semantic_model:` block on a [model](https://docs.getdbt.com/reference/model-properties.md). In the *legacy spec*, we used standalone YAML. For more information, refer to [Semantic models](https://docs.getdbt.com/docs/build/semantic-models.md).

<!-- -->

Availability

The latest YAML spec is supported in the following environments:

* **dbt platform (Latest release track)**
* **dbt Fusion engine**
* **dbt Core v1.12**

For more information, refer to [Migrate to the latest YAML spec](https://docs.getdbt.com/docs/build/latest-metrics-spec.md).

<!-- -->

## Legacy spec[​](#legacy-spec "Direct link to Legacy spec")

Semantic models are defined in a top-level `semantic_models:` list in standalone YAML, with `model`, `defaults`, `entities`, `dimensions`, and `measures`.

### Available semantic model properties (legacy spec)[​](#available-semantic-model-properties-legacy-spec "Direct link to Available semantic model properties (legacy spec)")

| Property        | Type   | Required | Description                                                                                                                                                                                                                  |
| --------------- | ------ | -------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| name            | string | Yes      | Unique name for the semantic model. Avoid double underscores (`__`).                                                                                                                                                         |
| description     | string | No       | Documentation for the semantic model.                                                                                                                                                                                        |
| model           | string | Yes      | The dbt model reference (for example, `ref('my_model')`).                                                                                                                                                                    |
| defaults        | object | Yes      | Defaults; typically `agg_time_dimension`.                                                                                                                                                                                    |
| entities        | array  | Yes      | Join keys and type (primary, foreign, unique); each with `name`, `type`, optional `expr`.                                                                                                                                    |
| primary\_entity | string | No       | Name of the primary entity if not declared on an entity.                                                                                                                                                                     |
| dimensions      | array  | Yes      | List of [dimension](https://docs.getdbt.com/reference/dimension-properties.md) definitions (time or categorical).                                                                                                            |
| measures        | array  | No       | List of measures (simple aggregations).                                                                                                                                                                                      |
| label           | string | No       | Display name in downstream tools.                                                                                                                                                                                            |
| config          | object | No       | Supports [meta](https://docs.getdbt.com/reference/resource-configs/meta.md), [group](https://docs.getdbt.com/reference/resource-configs/group.md), [enabled](https://docs.getdbt.com/reference/resource-configs/enabled.md). |

Search table...

|                  |   |   |   |   |
| ---------------- | - | - | - | - |
| Loading table... |   |   |   |   |

### Full structure[​](#full-structure "Direct link to Full structure")

```yaml
semantic_models:
  - name: <unique_name>
    description: <string>
    model: "{{ ref('my_model') }}"
    defaults:
      agg_time_dimension: <time_dimension_name>
    entities:
      - name: <entity_name>
        type: primary | foreign | unique
        expr: <optional_sql_expr>
    dimensions:
      - name: <dimension_name>
        type: time | categorical
        # ... see dimension-properties
    measures:
      - name: <measure_name>
        agg: sum | count | count_distinct | avg | min | max | ...
        expr: <column_or_expr>
    label: <display_name>
    config:
      meta: {}
      group: <string>
      enabled: true | false
```

For the latest spec (model-embedded form with top-level `semantic_model:` and `metrics:` on the model), see [Semantic models](https://docs.getdbt.com/docs/build/semantic-models.md).
