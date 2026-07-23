# About toyaml context method

The `toyaml` context method can be used to serialize a Python object primitive, eg. a `dict` or `list` to a YAML string.

**Args**:

* `value`: The value to serialize to YAML (required)
* `default`: A default value to return if the `value` argument cannot be serialized (optional)

### Usage:[​](#usage "Direct link to Usage:")

```text
{% set my_dict = {"abc": 123} %}
{% set my_yaml_string = toyaml(my_dict) %}

{% do log(my_yaml_string) %}
```
