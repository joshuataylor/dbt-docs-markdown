# Model properties

Models properties can be declared in `.yml` files in your `models/` directory (as defined by the [`model-paths` config](./project-configs/model-paths.md)).

You can name these files `whatever_you_want.yml`, and nest them arbitrarily deeply in subfolders within the `models/` directory.

<!-- -->

Availability

The latest YAML spec is supported in the following environments:

* **dbt platform (Latest release track)**
* **dbt Fusion engine**
* **dbt Core v1.12**

For more information, refer to [Migrate to the latest YAML spec](../docs/build/latest-metrics-spec.md).

## Available top-level model properties[​](#available-top-level-model-properties "Direct link to Available top-level model properties")

| Property                                                                                       | Type         | Required | Description                                                                                                                                                                                                                                  |
| ---------------------------------------------------------------------------------------------- | ------------ | -------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| [name](./resource-properties/model_name.md)                    | string       | Yes      | The model name (must match the model filename).                                                                                                                                                                                              |
| [description](./resource-properties/description.md)            | string       | No       | Documentation for the model.                                                                                                                                                                                                                 |
| [columns](./resource-properties/columns.md)                    | array        | No       | List of column definitions.                                                                                                                                                                                                                  |
| [config](./resource-properties/config.md)                      | object       | No       | Model configuration (materialization, tags, etc.).                                                                                                                                                                                           |
| [constraints](./resource-properties/constraints.md)            | array        | No       | Model-level constraints (primary key, foreign key, etc.).                                                                                                                                                                                    |
| [data\_tests](./resource-properties/data-tests.md)             | array        | No       | Model-level data tests.                                                                                                                                                                                                                      |
| tests                                                                                          | array        | No       | Legacy alias for data\_tests.                                                                                                                                                                                                                |
| [versions](./resource-properties/versions.md)                  | array        | No       | Model version definitions.                                                                                                                                                                                                                   |
| [latest\_version](./resource-properties/latest_version.md)     | string/float | No       | The latest version of the model.                                                                                                                                                                                                             |
| [deprecation\_date](./resource-properties/deprecation_date.md) | string       | No       | Date when the model is deprecated.                                                                                                                                                                                                           |
| [access](./resource-configs/access.md)                         | string       | No       | Access level: private, protected, or public. Supported at the top-level for backwards compatibility only.                                                                                                                                    |
| [time\_spine](../docs/build/metricflow-time-spine.md)                     | object       | No       | Time spine configuration for the Semantic Layer.                                                                                                                                                                                             |
| [semantic\_model](./semantic-model-properties.md)              | object       | No       | *Latest YAML spec only.* Enable semantic model configuration for the Semantic Layer with `enabled: true`. For other properties, refer to [Semantic model properties](./semantic-model-properties.md).        |
| [metrics](./metric-properties.md)                              | array        | No       | *Latest YAML spec only.* Metrics derived from this semantic model; list is alongside (not under) `semantic_model` and `columns`. For other properties, refer to [Metric properties](./metric-properties.md). |

Search table...

|                  |   |   |   |   |
| ---------------- | - | - | - | - |
| Loading table... |   |   |   |   |

### Example file (legacy)[​](#example-file-legacy "Direct link to Example file (legacy)")

The following legacy YAML spec example does not include Semantic Layer properties. Select **dbt platform Latest** from the version menu to see an example latest YAML spec.

models/\<filename>.yml

```yml

models:
  # Model name must match the filename of a model -- including case sensitivity
  - name: model_name
    description: <markdown_string>
    latest_version: <version_identifier>
    deprecation_date: <YAML_DateTime>
    config:
      <model_config>: <config_value>
      docs:
        show: true | false
        node_color: <color_id> # Use name (such as node_color: purple) or hex code with quotes (such as node_color: "#cd7f32")
      access: private | protected | public
    constraints:
      - <constraint>
    data_tests:
      - <test>
      - ... # declare additional data tests
    columns:
      - name: <column_name> # required
        description: <markdown_string>
        quote: true | false
        constraints:
          - <constraint>
        data_tests:
          - <test>
          - ... # declare additional data tests
        config:
          meta: {<dictionary>}
          tags: [<string>]
        
        # only required in conjunction with time_spine key
        granularity: <any supported time granularity> 

      - name: ... # declare properties of additional columns

    time_spine:
      standard_granularity_column: <column_name>

    versions:
      - v: <version_identifier> # required
        defined_in: <definition_file_name>
        description: <markdown_string>
        constraints:
          - <constraint>
        config:
          <model_config>: <config_value>
          docs:
            show: true | false
          access: private | protected | public
        data_tests:
          - <test>
          - ... # declare additional data tests
        columns:
          # include/exclude columns from the top-level model properties
          - include: <include_value>
            exclude: <exclude_list>
          # specify additional columns
          - name: <column_name> # required
            quote: true | false
            constraints:
              - <constraint>
            data_tests:
              - <test>
              - ... # declare additional data tests
            tags: [<string>]
        - v: ... # declare additional versions
```
