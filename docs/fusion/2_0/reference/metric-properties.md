# Metric properties

Metrics define measurable quantities that you can query through the Semantic Layer. You define them in different places, depending on your dbt version:

* In a model using the *latest* YAML spec. Top-level `metrics:` list on a [model](https://docs.getdbt.com/reference/model-properties.md) that has semantic modeling enabled, alongside `semantic_model:` and `columns:`. Available in the dbt platform **Latest** release track and the dbt Fusion engine.
* In the standalone *legacy* YAML spec. Refer to [Creating metrics](https://docs.getdbt.com/docs/build/metrics-overview.md) for more information.

<!-- -->

Availability

The latest YAML spec is supported in the following environments:

* **dbt platform (Latest release track)**
* **dbt Fusion engine**
* **dbt Core v1.12**

For more information, refer to [Migrate to the latest YAML spec](https://docs.getdbt.com/docs/build/latest-metrics-spec.md).

## Latest spec (model YAML)[​](#latest-spec-model-yaml "Direct link to Latest spec (model YAML)")

In the latest YAML spec, you can define metrics on a model that has semantic modeling enabled. Add a top-level `metrics` list alongside `semantic_model` and `columns` (metrics are not nested under `semantic_model`). Type-specific settings are top-level keys on each metric.

### Available metric properties (latest spec)[​](#available-metric-properties-latest-spec "Direct link to Available metric properties (latest spec)")

| Property    | Type   | Required | Description                                                                                                                                                                                                                                                                                      |
| ----------- | ------ | -------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| name        | string | Yes      | Unique metric name. Use lowercase letters, numbers, and underscores.                                                                                                                                                                                                                             |
| type        | string | Yes      | One of: `simple`, `cumulative`, `ratio`, `derived`, `conversion`.                                                                                                                                                                                                                                |
| description | string | No       | Documentation for the metric.                                                                                                                                                                                                                                                                    |
| label       | string | No       | Display name in downstream tools.                                                                                                                                                                                                                                                                |
| filter      | string | No       | MetricFlow filter expression (dimensions, entities, or other metrics).                                                                                                                                                                                                                           |
| config      | object | No       | Supports [meta](https://docs.getdbt.com/reference/resource-configs/meta.md), [group](https://docs.getdbt.com/reference/resource-configs/group.md), [tags](https://docs.getdbt.com/reference/resource-configs/tags.md), [enabled](https://docs.getdbt.com/reference/resource-configs/enabled.md). |

Search table...

|                  |   |   |   |   |
| ---------------- | - | - | - | - |
| Loading table... |   |   |   |   |

### Properties by metric type (latest spec)[​](#properties-by-metric-type-latest-spec "Direct link to Properties by metric type (latest spec)")

| Metric type                                                    | Key properties                                                                                                                  |
| -------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------- |
| All                                                            | name, type, description, label, hidden, filter, config                                                                          |
| [Simple](https://docs.getdbt.com/docs/build/simple.md)         | agg, expr, time\_granularity, agg\_time\_dimension, join\_to\_timespine, fill\_nulls\_with; optionally non\_additive\_dimension |
| [Derived](https://docs.getdbt.com/docs/build/derived.md)       | expr, input\_metrics (each with optional alias, filter, offset\_window)                                                         |
| [Ratio](https://docs.getdbt.com/docs/build/ratio.md)           | numerator, denominator (each a metric name or a dict with name, filter, alias)                                                  |
| [Conversion](https://docs.getdbt.com/docs/build/conversion.md) | entity, calculation, base\_metric, conversion\_metric, window; optional constant\_properties                                    |
| [Cumulative](https://docs.getdbt.com/docs/build/cumulative.md) | input\_metric, window, grain\_to\_date, period\_agg                                                                             |

Search table...

|                  |   |   |   |   |
| ---------------- | - | - | - | - |
| Loading table... |   |   |   |   |

**Cross-model metrics:** Metrics under a model's `metrics:` list can only reference that semantic model. Metrics that depend on other semantic models (for example, cross-model cumulative, ratio, derived, or conversion) go in a top-level `metrics:` block (outside `models:`). This can live in the same YAML file or a separate file.

**Note:** For the legacy spec, all metrics were defined in standalone YAML; there was no model-level `metrics:` list.

For the latest spec, refer to [Semantic models](https://docs.getdbt.com/docs/build/semantic-models.md). For metric types, `type_params`, and more examples, refer to [Creating metrics](https://docs.getdbt.com/docs/build/metrics-overview.md).

## Was this page helpful?

YesNo

[Privacy policy](https://www.getdbt.com/cloud/privacy-policy)[Create a GitHub issue](https://github.com/dbt-labs/docs.getdbt.com/issues)

This site is protected by reCAPTCHA and the Google [Privacy Policy](https://policies.google.com/privacy) and [Terms of Service](https://policies.google.com/terms) apply.
