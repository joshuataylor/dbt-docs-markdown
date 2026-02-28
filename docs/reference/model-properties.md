# Model properties

Models properties can be declared in `.yml` files in your `models/` directory (as defined by the [`model-paths` config](https://docs.getdbt.com/reference/project-configs/model-paths.md)).

You can name these files `whatever_you_want.yml`, and nest them arbitrarily deeply in subfolders within the `models/` directory.

## Available top-level model properties[​](#available-top-level-model-properties "Direct link to Available top-level model properties")

| Property                                                                                       | Type         | Required | Description                                                                                               |
| ---------------------------------------------------------------------------------------------- | ------------ | -------- | --------------------------------------------------------------------------------------------------------- |
| [name](https://docs.getdbt.com/reference/resource-properties/model_name.md)                    | string       | Yes      | The model name (must match the model filename).                                                           |
| [description](https://docs.getdbt.com/reference/resource-properties/description.md)            | string       | No       | Documentation for the model.                                                                              |
| [columns](https://docs.getdbt.com/reference/resource-properties/columns.md)                    | array        | No       | List of column definitions.                                                                               |
| [config](https://docs.getdbt.com/reference/resource-properties/config.md)                      | object       | No       | Model configuration (materialization, tags, etc.).                                                        |
| [constraints](https://docs.getdbt.com/reference/resource-properties/constraints.md)            | array        | No       | Model-level constraints (primary key, foreign key, etc.).                                                 |
| [data\_tests](https://docs.getdbt.com/reference/resource-properties/data-tests.md)             | array        | No       | Model-level data tests.                                                                                   |
| tests                                                                                          | array        | No       | Legacy alias for data\_tests.                                                                             |
| [versions](https://docs.getdbt.com/reference/resource-properties/versions.md)                  | array        | No       | Model version definitions.                                                                                |
| [latest\_version](https://docs.getdbt.com/reference/resource-properties/latest_version.md)     | string/float | No       | The latest version of the model.                                                                          |
| [deprecation\_date](https://docs.getdbt.com/reference/resource-properties/deprecation_date.md) | string       | No       | Date when the model is deprecated.                                                                        |
| [access](https://docs.getdbt.com/reference/resource-configs/access.md)                         | string       | No       | Access level: private, protected, or public. Supported at the top-level for backwards compatibility only. |
| [time\_spine](https://docs.getdbt.com/docs/build/metricflow-time-spine.md)                     | object       | No       | Time spine configuration for semantic layer.                                                              |

Search table...

|                  |   |   |   |   |
| ---------------- | - | - | - | - |
| Loading table... |   |   |   |   |

### Example file[​](#example-file "Direct link to Example file")

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

## Was this page helpful?

YesNo

[Privacy policy](https://www.getdbt.com/cloud/privacy-policy)[Create a GitHub issue](https://github.com/dbt-labs/docs.getdbt.com/issues)

This site is protected by reCAPTCHA and the Google [Privacy Policy](https://policies.google.com/privacy) and [Terms of Service](https://policies.google.com/terms) apply.
