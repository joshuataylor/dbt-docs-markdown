# Function configurations

💡Did you know\...

Available from dbt v

<!-- -->

1.11

<!-- -->

or with the

<!-- -->

[dbt "Latest" release track](https://docs.getdbt.com/docs/dbt-versions/cloud-release-tracks.md).

## Available configurations[​](#available-configurations "Direct link to Available configurations")

### Function-specific configurations[​](#function-specific-configurations "Direct link to Function-specific configurations")

Resource-specific configurations are applicable to only one dbt resource type rather than multiple resource types. You can define these settings in the project file (`dbt_project.yml`), a property file (`models/properties.yml` for models, similarly for other resources), or within the resource’s file using the `{{ config() }}` macro.<br />

The following resource-specific configurations are only available to <!-- -->Functions:

* Project YAML file
* Properties YAML file

dbt\_project.yml

```yml
functions:
  <resource-path>:
    # Function-specific configs are defined in the properties YAML file
    # See functions/schema.yml examples below
```

functions/schema.yml

```yaml

functions:
  - name: [<function-name>]
    config:
      type: scalar  # optional, defaults to scalar. Eventually will include aggregate | table
      volatility: deterministic | stable | non-deterministic # optional
      runtime_version: <string> # required for Python UDFs
      entry_point: <string> # required for Python UDFs
      # Standard configs that apply to functions
      database: <string>
      schema: <string>
      alias: <string>
      tags: <string> | [<string>]
      meta: {<dictionary>}
```

### General configurations[​](#general-configurations "Direct link to General configurations")

General configurations provide broader operational settings applicable across multiple resource types. Like resource-specific configurations, these can also be set in the project file, property files, or within resource-specific files.

Database, schema, and alias configuration

Functions support `database`, `schema`, and `alias` configurations just like models. These determine where the function is created in your warehouse. The function will use the standard dbt configuration precedence (specific config > project config > target profile defaults).

* Project YAML file
* Properties YAML file

dbt\_project.yml

```yaml
functions:
  <resource-path>:
    +enabled: true | false
    +tags: <string> | [<string>]
    +database: <string>
    +schema: <string>
    +alias: <string>
    +meta: {<dictionary>}
```

functions/schema.yml

```yaml

functions:
  - name: [<function-name>]
    config:
      enabled: true | false
      tags: <string> | [<string>]
      database: <string>
      schema: <string>
      alias: <string>
      meta: {<dictionary>}
```

## Configuring functions[​](#configuring-functions "Direct link to Configuring functions")

Functions are configured in YAML files, either in `dbt_project.yml` or within an individual function's YAML properties file. The function body is defined in a SQL file in the `functions/` directory.

Function configurations, like model configurations, are applied hierarchically. For more info, refer to [config inheritance](https://docs.getdbt.com/reference/define-configs.md#config-inheritance).

Functions respect the same name-generation macros as models: [`generate_database_name`](https://docs.getdbt.com/docs/build/custom-databases.md), [`generate_schema_name`](https://docs.getdbt.com/docs/build/custom-schemas.md#how-does-dbt-generate-a-models-schema-name), and [`generate_alias_name`](https://docs.getdbt.com/docs/build/custom-aliases.md).

### Examples[​](#examples "Direct link to Examples")

#### Apply the `schema` configuration to all functions[​](#apply-the-schema-configuration-to-all-functions "Direct link to apply-the-schema-configuration-to-all-functions")

To apply a configuration to all functions, including those in any installed [packages](https://docs.getdbt.com/docs/build/packages.md), nest the configuration directly under the `functions` key:

dbt\_project.yml

```yml

functions:
  +schema: udf_schema
```

#### Apply the `schema` configuration to all functions in your project[​](#apply-the-schema-configuration-to-all-functions-in-your-project "Direct link to apply-the-schema-configuration-to-all-functions-in-your-project")

To apply a configuration to all functions in your project only (i.e. *excluding* any functions in installed packages), provide your [project name](https://docs.getdbt.com/reference/project-configs/name.md) as part of the resource path.

For a project named `jaffle_shop`:

dbt\_project.yml

```yml

functions:
  jaffle_shop:
    +schema: udf_schema
```

Similarly, you can use the name of an installed package to configure functions in that package.

#### Apply the `schema` configuration to one function only[​](#apply-the-schema-configuration-to-one-function-only "Direct link to apply-the-schema-configuration-to-one-function-only")

To apply a configuration to one function only in a properties file, specify the configuration in the function's `config` block:

functions/schema.yml

```yml

functions:
  - name: is_positive_int
    config:
      schema: udf_schema
```

To apply a configuration to one function only in `dbt_project.yml`, provide the full resource path (including the project name and subdirectories). For a project named `jaffle_shop`, with a function file at `functions/is_positive_int.sql`:

dbt\_project.yml

```yml
functions:
  jaffle_shop:
    is_positive_int:
      +schema: udf_schema
```

## Example function configuration[​](#example-function-configuration "Direct link to Example function configuration")

The following example shows how to configure functions in a project named `jaffle_shop` that has two function files:

* `functions/is_positive_int.sql`
* `functions/marketing/clean_url.sql`

dbt\_project.yml

```yml
name: jaffle_shop
...
functions:
  jaffle_shop:
    +enabled: true
    +schema: udf_schema
    # This configures functions/is_positive_int.sql
    is_positive_int:
      +tags: ['validation']
    marketing:
      +schema: marketing_udfs # this will take precedence
```

functions/schema.yml

```yml

functions:
  - name: is_positive_int
    description: Determines if a string represents a positive integer
    config:
      type: scalar
      volatility: deterministic
      database: analytics
      schema: udf_schema
    arguments:
      - name: a_string
        data_type: string
        description: The string to check
    returns:
      data_type: boolean
      description: Returns true if the string represents a positive integer
```

## Was this page helpful?

YesNo

[Privacy policy](https://www.getdbt.com/cloud/privacy-policy)[Create a GitHub issue](https://github.com/dbt-labs/docs.getdbt.com/issues)

This site is protected by reCAPTCHA and the Google [Privacy Policy](https://policies.google.com/privacy) and [Terms of Service](https://policies.google.com/terms) apply.
