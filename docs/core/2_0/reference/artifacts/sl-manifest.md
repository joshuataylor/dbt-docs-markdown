# Semantic manifest

**Produced by:** Any command that parses your project. This includes all commands *except* [`deps`](https://docs.getdbt.com/reference/commands/deps.md), [`clean`](https://docs.getdbt.com/reference/commands/clean.md), [`debug`](https://docs.getdbt.com/reference/commands/debug.md), and [`init`](https://docs.getdbt.com/reference/commands/init.md).

dbt creates an [artifact](https://docs.getdbt.com/reference/artifacts/dbt-artifacts.md) file called the *Semantic Manifest* (`semantic_manifest.json`), which MetricFlow requires to build and run metric queries properly for the dbt Semantic Layer. This artifact contains comprehensive information about your dbt Semantic Layer. It is an internal file that acts as the integration point with MetricFlow.

By using the semantic manifest produced by dbt Core, MetricFlow will instantiate a data flow plan and generate SQL from Semantic Layer query requests. It's a valuable reference that you can use to understand the structure and details of your data models.

Similar to the [`manifest.json` file](https://docs.getdbt.com/reference/artifacts/manifest-json.md), the `semantic_manifest.json` file also lives in the [target directory](https://docs.getdbt.com/reference/global-configs/json-artifacts.md) of your dbt project where dbt stores various artifacts (such as compiled models and tests) generated during the execution of your project.

There are two reasons why `semantic_manifest.json` exists alongside `manifest.json`:

* Deserialization: `dbt-core` and MetricFlow use different libraries for handling data serialization.
* Efficiency and performance: MetricFlow and the dbt Semantic Layer need specific semantic details from the manifest. By trimming down the information printed into `semantic_manifest.json`, the process becomes more efficient and enables faster data handling between `dbt-core` and MetricFlow.

## Top-level keys[​](#top-level-keys "Direct link to Top-level keys")

Top-level keys for the semantic manifest are:

* `semantic_models` — Starting points of data with entities and dimensions, and correspond to models in your dbt project.
* `metrics` — Functions combining entities, dimensions, and so on to define quantitative indicators.
* `project_configuration` — Contains information around your project configurations
* `saved_queries` — Saves commonly used queries in MetricFlow

<!-- -->

### Example[​](#example "Direct link to Example")

target/semantic\_manifest.json

```json
{
    "semantic_models": [
        {
            "name": "semantic model name",
            "defaults": null,
            "description": "semantic model description",
            "node_relation": {
                "alias": "model alias",
                "schema_name": "model schema",
                "database": "model db",
                "relation_name": "Fully qualified relation name"
            },
            "entities": ["entities in the semantic model"],
            "measures": ["measures in the semantic model"],
            "dimensions": ["dimensions in the semantic model" ],
        }
    ],
    "metrics": [
        {
            "name": "name of the metric",
            "description": "metric description",
            "type": "metric type",
            "type_params": {
                "measure": {
                    "name": "name for measure",
                    "filter": "filter for measure",
                    "alias": "alias for measure"
                },
                "numerator": null,
                "denominator": null,
                "expr": null,
                "window": null,
                "grain_to_date": null,
                "metrics": ["metrics used in defining the metric. this is used in derived metrics"],
                "input_measures": []
            },
            "filter": null,
            "metadata": null
        }
    ],
    "project_configuration": {
        "time_spine_table_configurations": [
            {
                "location": "fully qualified table name for timespine",
                "column_name": "date column",
                "grain": "day"
            }
        ],
        "metadata": null,
        "dsi_package_version": {}
    },
    "saved_queries": [
        {
            "name": "name of the saved query",
            "query_params": {
                "metrics": [
                    "metrics used in the saved query"
                ],
                "group_by": [
                    "TimeDimension('model_primary_key__date_column', 'day')",
                    "Dimension('model_primary_key__metric_one')",
                    "Dimension('model__dimension')"
                ],
                "where": null
            },
            "description": "Description of the saved query",
            "metadata": null,
            "label": null,
            "exports": [
                {
                    "name": "saved_query_name",
                    "config": {
                        "export_as": "view",
                        "schema_name": null,
                        "alias": null
                    }
                }
            ]
        }
    ]
}
```

## OSI document[​](#osi-document "Direct link to OSI document")

**Produced by:** Any command that parses your project (same as the semantic manifest).

Starting in dbt Core v1.12, dbt also writes an `osi_document.json` file to your `target/` directory alongside `semantic_manifest.json` at parse time. This file represents your project's Semantic Layer in the [Open Semantic Interchange (OSI)](https://github.com/open-semantic-interchange) format, a vendor-agnostic schema for describing semantic models and metrics.

The OSI document is generated by converting the full `PydanticSemanticManifest` to the OSI format. Not all dbt semantic layer constructs have an OSI equivalent, so dbt emits warnings (event code `I078`) when elements are dropped or degraded during conversion:

| Warning                     | Cause                                                                                        |
| --------------------------- | -------------------------------------------------------------------------------------------- |
| `CONVERSION_METRIC_DROPPED` | Conversion metrics cannot be represented in OSI and are excluded.                            |
| `PRIVATE_METRIC_DROPPED`    | Private metrics are not included in OSI output.                                              |
| `NATURAL_ENTITY_DROPPED`    | Natural entities have no OSI equivalent and are excluded.                                    |
| `CUMULATIVE_SEMANTICS_LOSS` | The metric is included, but its cumulative window and grain semantics cannot be represented. |

Search table...

|                  |   |   |   |   |
| ---------------- | - | - | - | - |
| Loading table... |   |   |   |   |

If the semantic manifest fails validation, dbt logs an `error` level `SemanticValidationFailure` and skips writing `osi_document.json` for that invocation.

## Related docs[​](#related-docs "Direct link to Related docs")

* [Semantic Layer API](https://docs.getdbt.com/docs/dbt-apis/sl-api-overview.md)
* [About dbt artifacts](https://docs.getdbt.com/reference/artifacts/dbt-artifacts.md)

## Was this page helpful?

YesNo

[Privacy policy](https://www.getdbt.com/cloud/privacy-policy)[Create a GitHub issue](https://github.com/dbt-labs/docs.getdbt.com/issues)

This site is protected by reCAPTCHA and the Google [Privacy Policy](https://policies.google.com/privacy) and [Terms of Service](https://policies.google.com/terms) apply.
