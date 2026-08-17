# Metric properties

Metrics define measurable quantities that you can query through the Semantic Layer. You define them in different places, depending on your dbt version:

* In a model using the *latest* YAML spec. Top-level `metrics:` list on a [model](./model-properties.md) that has semantic modeling enabled, alongside `semantic_model:` and `columns:`. Available in the dbt platform **Latest** release track and the dbt Fusion engine.
* In the standalone *legacy* YAML spec. Refer to [Creating metrics](../docs/build/metrics-overview.md) for more information.

Availability

The latest YAML spec is supported in the following environments:

* **dbt platform (Latest release track)**
* **dbt Fusion engine**
* **dbt Core v1.12**

For more information, refer to [Migrate to the latest YAML spec](../docs/build/latest-metrics-spec.md).

(Applies to dbt v1.11 and earlier)

## Legacy spec (standalone YAML)

Metrics are defined in a top-level `metrics:` key in standalone YAML. Type-specific settings go under `type_params`.

### Available metric properties (legacy spec)

| Property     | Type   | Required | Description                                                                                                                                                                                                                                                                                      |
| ------------ | ------ | -------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| name         | string | Yes      | Unique metric name. Use lowercase letters, numbers, and underscores.                                                                                                                                                                                                                             |
| type         | string | Yes      | One of: `simple`, `cumulative`, `ratio`, `derived`, `conversion`.                                                                                                                                                                                                                                |
| type\_params | object | Yes      | Type-specific parameters; structure depends on `type`. See the type-specific list below.                                                                                                                                                                                                         |
| description  | string | No       | Documentation for the metric.                                                                                                                                                                                                                                                                    |
| label        | string | Yes      | Display name in downstream tools.                                                                                                                                                                                                                                                                |
| filter       | string | No       | MetricFlow filter expression (dimensions, entities, or other metrics).                                                                                                                                                                                                                           |
| config       | object | No       | Supports [meta](./resource-configs/meta.md), [group](./resource-configs/group.md), [tags](./resource-configs/tags.md), [enabled](./resource-configs/enabled.md). |

### Type-specific parameters (legacy spec)

The following parameters apply by metric type under `type_params`:

* **[Simple](../docs/build/simple.md)**: `agg` (required), `expr`, `percentile`, `percentile_type`, `non_additive_dimension`, `agg_time_dimension`, `join_to_timespine`, `fill_nulls_with`
* **[Cumulative](../docs/build/cumulative.md)**: `input_metric` (required), `window`, `grain_to_date`, `period_agg`
* **[Derived](../docs/build/derived.md)**: `expr` (required), `input_metrics` (required)
* **[Ratio](../docs/build/ratio.md)**: `numerator` (required), `denominator` (required)
* **[Conversion](../docs/build/conversion.md)**: `entity` (required), `calculation` (required), `base_metric` (required), `conversion_metric` (required), `window`, `constant_properties`

For full `type_params` and examples per type, see [Creating metrics](../docs/build/metrics-overview.md), [Simple metrics](../docs/build/simple.md), [Cumulative metrics](../docs/build/cumulative.md), [Ratio metrics](../docs/build/ratio.md), [Derived metrics](../docs/build/derived.md), and [Conversion metrics](../docs/build/conversion.md).
