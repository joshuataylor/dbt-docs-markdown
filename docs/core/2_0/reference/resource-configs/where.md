# where

### Definition[​](#definition "Direct link to Definition")

Filter the resource being tested (model, source, seed, or snapshot).

The `where` condition is templated into the test query by replacing the resource reference with a subquery. For instance, a `not_null` test may look like:

```sql
select *
from my_model
where my_column is null
```

If the `where` config is set to `where date_column = current_date`, then the test query will be updated to:

```sql
select *
from (select * from my_model where date_column = current_date) dbt_subquery
where my_column is null
```

### Examples[​](#examples "Direct link to Examples")

* Specific test
* One-off test
* Generic test block
* Project level

Configure a specific instance of a generic (schema) test:

models/\<filename>.yml

```yaml

models:
  - name: large_table
    columns:
      - name: my_column
        data_tests:
          - accepted_values:
              arguments: # available in v1.10.5 and higher. Older versions can set the <argument_name> as the top-level property.
                values: ["a", "b", "c"]
              config:
                where: "date_column = current_date"
      - name: other_column
        data_tests:
          - not_null:
              config: 
                where: "date_column < current_date"
```

This config is ignored for one-off tests.

Set the default for all instances of a generic (schema) test, by setting the config inside its test block (definition):

macros/\<filename>.sql

```sql
{% test <testname>(model, column_name) %}

{{ config(where = "date_column = current_date") }}

select ...

{% endtest %}
```

Set the default for all tests in a package or project:

dbt\_project.yml

```yaml
data_tests:
  +where: "date_column = current_date"
  
  <package_name>:
    +where: >
        date_column = current_date
        and another_column is not null
```

### Custom logic[​](#custom-logic "Direct link to Custom logic")

The rendering context for the `where` config is the same as for all configurations defined in `.yml` files. You have access to `{{ var() }}` and `{{ env_var() }}`, but you **do not** have access to custom macros for setting this config. If you do want to use custom macros to template out the `where` filter for certain tests, there is a workaround.

dbt defines a `get_where_subquery` macro.

dbt replaces `{{ model }}` in generic test definitions with `{{ get_where_subquery(relation) }}`, where `relation` is a `ref()` or `source()` for the resource being tested. The default implementation of this macro returns:

* `{{ relation }}` when the `where` config is not defined (`ref()` or `source()`)
* `(select * from {{ relation }} where {{ where }}) dbt_subquery` when the `where` config is defined

You can override this behavior by:

* Defining a custom `get_where_subquery` in your root project
* Defining a custom `<adapter>__get_where_subquery` [dispatch candidate](https://docs.getdbt.com/reference/dbt-jinja-functions/dispatch.md) in your package or adapter plugin

Within this macro definition, you can reference whatever custom macros you want, based on static inputs from the configuration. At simplest, this enables you to DRY up code that you'd otherwise need to repeat across many different `.yml` files. Because the `get_where_subquery` macro is resolved at runtime, your custom macros can also include [fetching the results of introspective database queries](https://docs.getdbt.com/reference/dbt-jinja-functions/run_query.md).

#### Example[​](#example "Direct link to Example")

Filter your test to the past N days of data, using dbt's cross-platform [`dateadd()`](https://docs.getdbt.com/reference/dbt-jinja-functions/cross-database-macros.md#dateadd) utility macro. You can set the number of days in the placeholder string.

models/config.yml

```yml
models:
  - name: my_model
    columns:
      - name: id
        data_tests:
          - unique:
              config:
                where: "date_column > __3_days_ago__"  # placeholder string for static config
```

macros/custom\_get\_where\_subquery.sql

```sql
{% macro get_where_subquery(relation) -%}
    {% set where = config.get('where') %}
    {% if where %}
        {% if "_days_ago__" in where %}
            {# replace placeholder string with result of custom macro #}
            {% set where = replace_days_ago(where) %}
        {% endif %}
        {%- set filtered -%}
            (select * from {{ relation }} where {{ where }}) dbt_subquery
        {%- endset -%}
        {% do return(filtered) %}
    {%- else -%}
        {% do return(relation) %}
    {%- endif -%}
{%- endmacro %}

{% macro replace_days_ago(where_string) %}
    {# Use regex to search the pattern for the number days #}
    {# Default to 3 days when no number found #}
    {% set re = modules.re %}
    {% set days = 3 %}
    {% set pattern = '__(\d+)_days_ago__' %}
    {% set match = re.search(pattern, where_string) %}
    {% if match %}
        {% set days = match.group(1) | int %}        
    {% endif %}
    {% set n_days_ago = dbt.dateadd('day', -days, current_timestamp()) %}
    {% set result = re.sub(pattern, n_days_ago, where_string) %}
    {{ return(result) }}
{% endmacro %}
```

## Was this page helpful?

YesNo

[Privacy policy](https://www.getdbt.com/cloud/privacy-policy)[Create a GitHub issue](https://github.com/dbt-labs/docs.getdbt.com/issues)

This site is protected by reCAPTCHA and the Google [Privacy Policy](https://policies.google.com/privacy) and [Terms of Service](https://policies.google.com/terms) apply.
