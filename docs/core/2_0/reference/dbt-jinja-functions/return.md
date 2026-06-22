# About return function

**Args**:

* `data`: The data to return to the caller

The `return` function can be used in macros to return data to the caller. The type of the data (dict, list, int, etc) will be preserved through the `return` call. You can use the `return` function in the following ways within your macros: as an expression or as a statement.

* Expression — Use an expression when the goal is to output a string from the macro.
* Statement with a `do` tag — Use a statement with a `do` tag to execute the return function without generating an output string. This is particularly useful when you want to perform actions without necessarily inserting their results directly into the template.

In the following example, `{{ return([1,2,3]) }}` acts as an *expression* that directly outputs a string, making it suitable for directly inserting returned values into SQL code.

macros/get\_data.sql

```sql
{% macro get_data() %}

  {{ return([1,2,3]) }}
  
{% endmacro %}
```

Alternatively, you can use a statement with a [do](https://jinja.palletsprojects.com/en/3.0.x/extensions/#expression-statement) tag (or expression-statements) to execute the return function without generating an output string.

In the following example ,`{% do return([1,2,3]) %}` acts as a *statement* that executes the return action but does not output a string:

macros/get\_data.sql

```sql
{% macro get_data() %}

  {% do return([1,2,3]) %}
  
{% endmacro %}
```

models/my\_model.sql

```sql

select
  -- getdata() returns a list!
  {% for i in get_data() %}
    {{ i }}
    {%- if not loop.last %},{% endif -%}
  {% endfor %}
```

## Was this page helpful?

YesNo

[Privacy policy](https://www.getdbt.com/cloud/privacy-policy)[Create a GitHub issue](https://github.com/dbt-labs/docs.getdbt.com/issues)

This site is protected by reCAPTCHA and the Google [Privacy Policy](https://policies.google.com/privacy) and [Terms of Service](https://policies.google.com/terms) apply.
