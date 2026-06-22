# Semantic Layer configurations

The Semantic Layer YAML spec defines every property and option for semantic models, metrics, and dimensions. Use it when you need the complete, authoritative list of what you can configure. For example, configurations when defining new objects, validating YAML, or troubleshooting config errors.

Because this reference provides information about both the *latest spec* (model-embedded) and the *legacy spec* (standalone YAML), you'll need to select the appropriate version from the version picker. [Read the build docs](https://docs.getdbt.com/docs/build/semantic-models.md) to find out which applies to your environment. To convert from the legacy spec, see [Migrate to the latest YAML spec](https://docs.getdbt.com/docs/build/latest-metrics-spec.md).

<!-- -->

Availability

The latest YAML spec is supported in the following environments:

* **dbt platform (Latest release track)**
* **dbt Fusion engine**
* **dbt Core v1.12**

## Property reference[​](#property-reference "Direct link to Property reference")

The property reference pages document each resource type in detail so you can look up allowed values, syntax, and behavior for every property in the full YAML spec. Use the version picker on each page to see *latest spec (model YAML)* or *legacy spec (standalone YAML)* content:

* [Semantic model properties](https://docs.getdbt.com/reference/semantic-model-properties.md)
* [Metric properties](https://docs.getdbt.com/reference/metric-properties.md)
* [Dimension properties](https://docs.getdbt.com/reference/dimension-properties.md) — for the latest spec, distinguishes **column-level** keys (`dimension:`, `granularity:`) from **properties inside** the `dimension:` block; use the version picker on that page for legacy standalone `dimensions:` fields.

## Where to define Semantic Layer objects[​](#where-to-define--objects "Direct link to where-to-define--objects")

| Object              | Latest spec (model YAML)                                                                                                                                                                                                                                                                                                                                                              | Legacy spec (standalone YAML)                                                                    |
| ------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------ |
| **Semantic models** | Top-level `semantic_model:` block under [models](https://docs.getdbt.com/reference/model-properties.md).                                                                                                                                                                                                                                                                              | Top-level `semantic_models:` list.                                                               |
| **Metrics**         | For metrics that only depend on the same semantic model, nest them directly under each model in `models:` (but not nested under `semantic_model`).<br /><br />For metrics that depend on metrics or dimensions from a different semantic model, define them under a top-level `metrics:` block (For example, `outside models:`). This can be in the same YAML file or a separate one. | Top-level `metrics:` key in standalone YAML.                                                     |
| **Dimensions**      | `dimension:` blocks on model columns (and optional `derived_semantics.dimensions`). Defined within a model's semantic layer configuration.                                                                                                                                                                                                                                            | `dimensions:` list on the semantic model. Defined within a model's semantic layer configuration. |

Search table...

|                  |   |   |   |   |
| ---------------- | - | - | - | - |
| Loading table... |   |   |   |   |

## Related documentation[​](#related-documentation "Direct link to Related documentation")

For the complete structure and examples, refer to these build docs:

* [Build your metrics](https://docs.getdbt.com/docs/build/build-metrics-intro.md): Conceptual overview of the Semantic Layer, metric types, and how to get started.
* [Semantic models](https://docs.getdbt.com/docs/build/semantic-models.md): How to define semantic models (on a model or standalone), structure, and examples.
* [Creating metrics](https://docs.getdbt.com/docs/build/metrics-overview.md): How to create and configure metrics; links to type-specific guides (simple, cumulative, ratio, derived, conversion).
* [Dimensions](https://docs.getdbt.com/docs/build/dimensions.md): How to define time and categorical dimensions within semantic models.
* [Migrate to the latest YAML spec](https://docs.getdbt.com/docs/build/latest-metrics-spec.md): How to migrate from the legacy metrics YAML spec to the latest spec.

## Was this page helpful?

YesNo

[Privacy policy](https://www.getdbt.com/cloud/privacy-policy)[Create a GitHub issue](https://github.com/dbt-labs/docs.getdbt.com/issues)

This site is protected by reCAPTCHA and the Google [Privacy Policy](https://policies.google.com/privacy) and [Terms of Service](https://policies.google.com/terms) apply.
