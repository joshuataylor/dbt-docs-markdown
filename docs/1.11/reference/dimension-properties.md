# Dimension properties

Dimensions are non-aggregatable expressions that define how metrics can be grouped or sliced. They are always defined within a [semantic model](https://docs.getdbt.com/reference/semantic-model-properties.md). See [Dimensions](https://docs.getdbt.com/docs/build/dimensions.md) for concepts and examples.

<!-- -->

Availability

The latest YAML spec is supported in the following environments:

* **dbt platform (Latest release track)**
* **dbt Fusion engine**
* **dbt Core v1.12**

For more information, refer to [Migrate to the latest YAML spec](https://docs.getdbt.com/docs/build/latest-metrics-spec.md).

<!-- -->

## Legacy spec (standalone semantic model)[​](#legacy-spec-standalone-semantic-model "Direct link to Legacy spec (standalone semantic model)")

Dimensions are defined in a top-level `dimensions:` list on the semantic model.

### Available dimension properties (legacy spec)[​](#available-dimension-properties-legacy-spec "Direct link to Available dimension properties (legacy spec)")

| Property      | Type    | Required        | Description                                                                                                                    |
| ------------- | ------- | --------------- | ------------------------------------------------------------------------------------------------------------------------------ |
| name          | string  | Yes             | Unique within the semantic model. Displayed in downstream tools; can act as alias when `expr` differs.                         |
| type          | string  | Yes             | `time` or `categorical`.                                                                                                       |
| type\_params  | object  | Yes (time only) | For time dimensions (for example, `time_granularity`, `is_primary`, `time_partitioning_granularity`). Omitted for categorical. |
| is\_partition | boolean | No              | Whether this dimension is a partition dimension for the model.                                                                 |
| description   | string  | No              | Documentation for the dimension.                                                                                               |
| expr          | string  | No              | Column or SQL expression. Defaults to the dimension name if omitted.                                                           |
| label         | string  | No              | Display value in downstream tools.                                                                                             |
| meta          | object  | No              | Metadata key-value pairs.                                                                                                      |

Search table...

|                  |   |   |   |   |
| ---------------- | - | - | - | - |
| Loading table... |   |   |   |   |

### Full structure (standalone semantic model, legacy spec)[​](#full-structure-standalone-semantic-model-legacy-spec "Direct link to Full structure (standalone semantic model, legacy spec)")

```yaml
dimensions:
  - name: <dimension_name>       # Required
    type: time | categorical    # Required
    type_params:                 # Required for time
      time_granularity: day | week | month | quarter | year
      is_primary: true | false
    is_partition: true | false   # Optional
    description: <string>        # Optional
    expr: <column_or_sql>        # Optional, defaults to name
    label: <display_name>        # Optional
    meta: {}                     # Optional
```

For the latest spec (column-level and derived dimensions), see [Dimensions](https://docs.getdbt.com/docs/build/dimensions.md).

## Was this page helpful?

YesNo

[Privacy policy](https://www.getdbt.com/cloud/privacy-policy)[Create a GitHub issue](https://github.com/dbt-labs/docs.getdbt.com/issues)

This site is protected by reCAPTCHA and the Google [Privacy Policy](https://policies.google.com/privacy) and [Terms of Service](https://policies.google.com/terms) apply.
