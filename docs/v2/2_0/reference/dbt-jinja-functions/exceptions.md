# About exceptions namespace

Use the `exceptions` namespace to raise your own warnings or errors from Jinja code in dbt models and macros during compilation and execution.

This is useful when your macro or model needs to check for a problem and tell dbt how to respond. For example, you might want to raise a warning or error when:

* a required input is missing
* a value isn't supported
* a dependency isn't available
* a custom validation check fails

These are errors or warnings that you choose to raise in your own Jinja code. They're different from the automatic dbt or Jinja errors, like syntax errors or undefined variables.

If you're not writing custom Jinja code and are instead trying to fix an error from dbt, refer to [Debug errors](https://docs.getdbt.com/guides/debug-errors.md).

When calling these functions during `dbt run` or `dbt run-operation`, wrap them in `{% if execute %}`. This ensures the function runs only when dbt executes your code, rather than when it parses your project.

Learn more about the [`execute`](https://docs.getdbt.com/reference/dbt-jinja-functions/execute.md) variable.

## raise\_compiler\_error[​](#raise_compiler_error "Direct link to raise_compiler_error")

The `exceptions.raise_compiler_error` method raises a compilation error with the provided message. This is typically only useful in macros or materializations when invalid arguments are provided by the calling model. Throwing an exception causes a model to fail, so use this method with care.

**Example usage**:

exceptions.sql

```sql
{% if execute %}
  {% if number < 0 or number > 100 %}
    {{ exceptions.raise_compiler_error("Invalid `number`. Got: " ~ number) }}
  {% endif %}
{% endif %}
```

<!-- -->

## raise\_fail\_fast\_error[​](#raise_fail_fast_error "Direct link to raise_fail_fast_error")

Use `exceptions.raise_fail_fast_error` to stop dbt immediately when it should not continue running.

This is commonly used when dbt detects a configuration change and `on_configuration_change` is set to `fail`.

The run fails with a `FailFast Error` and your custom message.

**Example usage**:

macros/materialized\_view\.sql

```sql
{% if on_configuration_change == 'fail' %}
  {{ exceptions.raise_fail_fast_error(
    "Configuration changes were identified and `on_configuration_change` was set to `fail` for `" ~ target_relation.render() ~ "`"
  ) }}
{% endif %}
```

## raise\_not\_implemented[​](#raise_not_implemented "Direct link to raise_not_implemented")

Use `exceptions.raise_not_implemented` to stop a dbt run when a macro or feature isn't supported by the current adapter.

This is often used by adapter and package authors when a feature hasn't been built yet.

The run fails with a `Runtime Error` and your custom message.

**Example usage**:

macros/get\_columns\_in\_relation.sql

```sql
{% macro default__get_columns_in_relation(relation) -%}
  {{ exceptions.raise_not_implemented(
    'get_columns_in_relation macro not implemented for adapter ' ~ adapter.type()) }}
{% endmacro %}
```

## warn[​](#warn "Direct link to warn")

Use the `exceptions.warn` method to raise a compiler warning with the provided message. The model will still be successful and treated as a PASS. By default, warnings will not cause dbt runs to fail. However:

* If you use the `--warn-error` flag, all warnings will be promoted to errors.
* To promote only Jinja warnings to errors (and leave other warnings alone), use `--warn-error-options`. For example, `--warn-error-options '{"error": ["JinjaLogWarning"]}'`.

Learn more about [Warnings](https://docs.getdbt.com/reference/global-configs/warnings.md).

**Example usage**:

warn.sql

```sql
{% if execute %}
  {% if number < 0 or number > 100 %}
    {% do exceptions.warn("Invalid `number`. Got: " ~ number) %}
  {% endif %}
{% endif %}
```
