# About config variable

The `config` variable exists to handle end-user configuration for custom materializations. Configs like `unique_key` can be implemented using the `config` variable in your own materializations.

For example, code in the `incremental` materialization like this:

```text
{% materialization incremental, default -%}
  {%- set unique_key = config.get('unique_key') -%}
  ...
```

is responsible for handling model code that looks like this:

```text
{{
  config(
    materialized='incremental',
    unique_key='id'
  )
}}
```

Review [Model configurations](https://docs.getdbt.com/reference/model-configs.md) for examples and more information on valid arguments.

## config.get[​](#configget "Direct link to config.get")

**Args**:

* `name`: The name of the configuration variable (required)
* `default`: The default value to use if this configuration is not provided (optional)

The `config.get` function is used to get configurations for a model from the end-user. Configs defined in this way are optional, and a default value can be provided.

There are 3 cases:

1. The configuration variable exists, it is not `None`
2. The configuration variable exists, it is `None`
3. The configuration variable does not exist

Accessing custom configurations in meta

`config.get()` doesn't return values from `config.meta`. If a key exists only in `meta`, `config.get()` returns the default value and emits a warning. To access custom configurations stored under `meta`, use [`config.meta_get()`](#configmeta_get).

Example usage:

```sql
{% materialization incremental, default -%}
  -- Example w/ no default. unique_key will be None if the user does not provide this configuration
  {%- set unique_key = config.get('unique_key') -%}

  -- Example w/ alternate value. Use alternative of 'id' if 'unique_key' config is provided, but it is None
  {%- set unique_key = config.get('unique_key') or 'id' -%}

  -- Example w/ default value. Default to 'id' if the 'unique_key' config does not exist
  {%- set unique_key = config.get('unique_key', default='id') -%}

  -- For custom configs under `meta`, use config.meta_get()
  {% set my_custom_config = config.meta_get('custom_config_key') %}
  ...
```

## config.require[​](#configrequire "Direct link to config.require")

**Args**:

* `name`: The name of the configuration variable (required)

The `config.require` function is used to get configurations for a model from the end-user. Configs defined using this function are required, and failure to provide them will result in a compilation error.

Accessing custom configurations in meta

`config.require()` doesn't return values from `config.meta`. If a key exists only in `meta`, `config.require()` raises an error and emits a warning. To access required custom configurations stored under `meta`, use [`config.meta_require()`](#configmeta_require).

Example usage:

```sql
{% materialization incremental, default -%}
  {%- set unique_key = config.require('unique_key') -%}
  ...
```

## config.meta\_get[​](#configmeta_get "Direct link to config.meta_get")

This functionality is available starting in dbt Core v1.10 and in the dbt Fusion engine.

**Args**:

* `name`: The name of the configuration variable to retrieve from `meta` (required)
* `default`: The default value to use if this configuration is not provided (optional)

The `config.meta_get` function retrieves custom configurations stored under the `meta` dictionary. Unlike `config.get()`, this function exclusively checks `config.meta` and won't result in a deprecation warning.

Use this function when accessing custom configurations that you've defined under `meta` in your model or resource configuration - it's equivalent to writing `config.get('meta').get()`.

Note that `config.meta_get` is not yet supported in Python models. In the meantime, Python models should continue using `dbt.config.get("meta").get("<key>")` to access custom meta configurations. `dbt.config.get_meta("<key>")` is an alias for `dbt.config.get("meta").get("<key>")`.

Example usage:

```sql
{% materialization custom_materialization, default -%}
  -- Retrieve a custom config from meta, returns None if not found
  {%- set custom_setting = config.meta_get('custom_setting') -%}

  -- Retrieve with a default value
  {%- set custom_setting = config.meta_get('custom_setting', default='default_value') -%}
  ...
```

Example model configuration:

```yaml
models:
  - name: my_model
    config:
      meta:
        custom_setting: "my_value"
```

## config.meta\_require[​](#configmeta_require "Direct link to config.meta_require")

This functionality is available starting in dbt Core v1.10 and in the dbt Fusion engine.

**Args**:

* `name`: The name of the configuration variable to retrieve from `meta` (required)

The `config.meta_require` function retrieves custom configurations stored under the `meta` dictionary. Unlike `config.require()`, this function exclusively checks `config.meta` and won't result in deprecation warnings. If the configuration is not found, dbt raises a compilation error.

Use this function when you need to ensure a custom configuration exists under `meta`.

Note that `config.meta_require` is not yet supported in Python models.

Example usage:

```sql
{% materialization custom_materialization, default -%}
  -- Require a custom config from meta, throws error if not found
  {%- set required_setting = config.meta_require('required_setting') -%}
  ...
```

Example model configuration:

```yaml
models:
  - name: my_model
    config:
      meta:
        required_setting: "my_value"
```

## Was this page helpful?

YesNo

[Privacy policy](https://www.getdbt.com/cloud/privacy-policy)[Create a GitHub issue](https://github.com/dbt-labs/docs.getdbt.com/issues)

This site is protected by reCAPTCHA and the Google [Privacy Policy](https://policies.google.com/privacy) and [Terms of Service](https://policies.google.com/terms) apply.
