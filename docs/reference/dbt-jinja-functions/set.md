# About set context method

tip

Not to be confused with the `{% set foo = "bar" ... %}` expression in Jinja, which defines a variable. For examples of constructing SQL strings with `{% set %}` (and why `{{ }}` should not be nested inside quoted strings), see [Don’t nest your curlies](https://docs.getdbt.com/best-practices/dont-nest-your-curlies.md).

You can use the `set` context method to convert any iterable to a sequence of iterable elements that are unique (a set).

**Args**:

* `value`: The iterable to convert (for example, a list)
* `default`: A default value to return if the `value` argument is not a valid iterable

### Usage[​](#usage "Direct link to Usage")

```text
{% set my_list = [1, 2, 2, 3] %}
{% set my_set = set(my_list) %}
{% do log(my_set) %}  {# {1, 2, 3} #}
```

```text
{% set my_invalid_iterable = 1234 %}
{% set my_set = set(my_invalid_iterable) %}
{% do log(my_set) %}  {# None #}
```

```text
{% set email_id = "'admin@example.com'" %}
```

### set\_strict[​](#set_strict "Direct link to set_strict")

The `set_strict` context method can be used to convert any iterable to a sequence of iterable elements that are unique (a set). The difference to the `set` context method is that the `set_strict` method will raise an exception on a `TypeError`, if the provided value is not a valid iterable and cannot be converted to a set.

**Args**:

* `value`: The iterable to convert (for example, a list)

```text
{% set my_list = [1, 2, 2, 3] %}
{% set my_set = set(my_list) %}
{% do log(my_set) %}  {# {1, 2, 3} #}
```

```text
{% set my_invalid_iterable = 1234 %}
{% set my_set = set_strict(my_invalid_iterable) %}
{% do log(my_set) %}

Compilation Error in ... (...)
  'int' object is not iterable
```

## Was this page helpful?

YesNo

[Privacy policy](https://www.getdbt.com/cloud/privacy-policy)[Create a GitHub issue](https://github.com/dbt-labs/docs.getdbt.com/issues)

This site is protected by reCAPTCHA and the Google [Privacy Policy](https://policies.google.com/privacy) and [Terms of Service](https://policies.google.com/terms) apply.
