# About zip context method

The `zip` context method can be used to return an iterator of tuples, where the i-th tuple contains the i-th element from each of the argument iterables. For more information, see [Python docs](https://docs.python.org/3/library/functions.html#zip).

**Args**:

* `*args`: Any number of iterables
* `default`: A default value to return if `*args` is not iterable

### Usage[​](#usage "Direct link to Usage")

```text
{% set my_list_a = [1, 2] %}
{% set my_list_b = ['alice', 'bob'] %}
{% set my_zip = zip(my_list_a, my_list_b) | list %}
{% do log(my_zip) %}  {# [(1, 'alice'), (2, 'bob')] #}
```

```text
{% set my_list_a = 12 %}
{% set my_list_b = ['alice', 'bob'] %}
{% set my_zip = zip(my_list_a, my_list_b, default = []) | list %}
{% do log(my_zip) %}  {# [] #}
```

### zip\_strict[​](#zip_strict "Direct link to zip_strict")

The `zip_strict` context method can be used to used to return an iterator of tuples, just like `zip`. The difference to the `zip` context method is that the `zip_strict` method will raise an exception on a `TypeError`, if one of the provided values is not a valid iterable.

**Args**:

* `value`: The iterable to convert (e.g. a list)

```text
{% set my_list_a = [1, 2] %}
{% set my_list_b = ['alice', 'bob'] %}
{% set my_zip = zip_strict(my_list_a, my_list_b) | list %}
{% do log(my_zip) %}  {# [(1, 'alice'), (2, 'bob')] #}
```

```text
{% set my_list_a = 12 %}
{% set my_list_b = ['alice', 'bob'] %}
{% set my_zip = zip_strict(my_list_a, my_list_b) %}

Compilation Error in ... (...)
  'int' object is not iterable
```

## Was this page helpful?

YesNo

[Privacy policy](https://www.getdbt.com/cloud/privacy-policy)[Create a GitHub issue](https://github.com/dbt-labs/docs.getdbt.com/issues)

This site is protected by reCAPTCHA and the Google [Privacy Policy](https://policies.google.com/privacy) and [Terms of Service](https://policies.google.com/terms) apply.
