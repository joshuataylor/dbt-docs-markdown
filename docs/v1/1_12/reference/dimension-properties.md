# Dimension properties

Dimensions are non-aggregatable expressions that define how metrics can be grouped or sliced. They are always defined within a [semantic model](https://docs.getdbt.com/reference/semantic-model-properties.md). See [Dimensions](https://docs.getdbt.com/docs/build/dimensions.md) for concepts and examples.

<!-- -->

Availability

The latest YAML spec is supported in the following environments:

* **dbt platform (Latest release track)**
* **dbt Fusion engine**
* **dbt Core v1.12**

For more information, refer to [Migrate to the latest YAML spec](https://docs.getdbt.com/docs/build/latest-metrics-spec.md).

## Latest spec (model YAML)[​](#latest-spec-model-yaml "Direct link to Latest spec (model YAML)")

Dimensions are defined at the column level.

### Column-level placement (latest spec)[​](#column-level-placement-latest-spec "Direct link to Column-level placement (latest spec)")

Add these keys to a **column** under the semantic model’s `columns:` list — they are not nested inside the `dimension:` object.

| Location       | Type               | Required                    | Description                                                                                                                                                                                                                                                                                             |
| -------------- | ------------------ | --------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `dimension:`   | block or shorthand | Yes (to define a dimension) | Attaches a dimension to the column. Use a mapping (`dimension:` with `type`, etc.) or the shorthand `dimension: categorical` / `dimension: time`.                                                                                                                                                       |
| `granularity:` | string             | Yes for **time** dimensions | Time grain of the underlying column data (for example, `day`, `week`, `month`). Place `granularity:` on the column next to `dimension:`, not inside the `dimension:` block. For **categorical** dimensions, `granularity` is not meaningful; if it appears in YAML, validation should surface an error. |

Search table...

|                  |   |   |   |   |
| ---------------- | - | - | - | - |
| Loading table... |   |   |   |   |

### Properties inside the `dimension:` block (latest spec)[​](#properties-inside-the-dimension-block-latest-spec "Direct link to properties-inside-the-dimension-block-latest-spec")

| Property          | Type    | Required | Description                                                                                                            |
| ----------------- | ------- | -------- | ---------------------------------------------------------------------------------------------------------------------- |
| `type`            | string  | Yes      | `time` or `categorical`.                                                                                               |
| `name`            | string  | No       | Unique within the semantic model; defaults to column name.                                                             |
| `description`     | string  | No       | Documentation for the dimension.                                                                                       |
| `label`           | string  | No       | Display value in downstream tools.                                                                                     |
| `is_partition`    | boolean | No       | Whether this dimension is a partition dimension for the model (supported for time and categorical dimensions in YAML). |
| `config`          | object  | No       | Metadata and config.                                                                                                   |
| `validity_params` | object  | No       | For **time** dimensions: SCD-style validity (for example, `is_start`, `is_end`).                                       |

Search table...

|                  |   |   |   |   |
| ---------------- | - | - | - | - |
| Loading table... |   |   |   |   |

**Derived dimensions:** To define dimensions with an `expr` that is not tied to a single column, use the semantic model’s optional `derived_semantics.dimensions` list. That structure is part of the [semantic model](https://docs.getdbt.com/reference/semantic-model-properties.md) configuration (alongside `columns:`), not a property nested under a column’s `dimension:` block. See [Semantic models](https://docs.getdbt.com/docs/build/semantic-models.md) and [Dimensions](https://docs.getdbt.com/docs/build/dimensions.md) for examples.

* **Column-level:** Under the model's `columns:` list, each column can have a `dimension:` block with *time* or *categorical* type, and optional `name`, `description`, `label`, `is_partition`, `config`.
* **Time dimensions:** The column must also have a top-level `granularity:` (for example, `day`).
* **Validity (SCD):** Time dimensions can specify `validity_params` (for example, `is_start`, `is_end`).

For concepts and usage patterns, refer to [Dimensions](https://docs.getdbt.com/docs/build/dimensions.md). For the latest spec, refer to [Semantic models](https://docs.getdbt.com/docs/build/semantic-models.md).
