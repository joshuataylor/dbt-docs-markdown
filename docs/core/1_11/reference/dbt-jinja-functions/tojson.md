# About tojson context method

The `tojson` context method can be used to serialize a Python object primitive, eg. a `dict` or `list` to a JSON string.

**Args**:

* `value`: The value to serialize to JSON (required)
* `default`: A default value to return if the `value` argument cannot be serialized (optional)

### Usage:[​](#usage "Direct link to Usage:")

```text
{% set my_dict = {"abc": 123} %}
{% set my_json_string = tojson(my_dict) %}

{% do log(my_json_string) %}
```

## Was this page helpful?

YesNo

[Privacy policy](https://www.getdbt.com/cloud/privacy-policy)[Create a GitHub issue](https://github.com/dbt-labs/docs.getdbt.com/issues)

This site is protected by reCAPTCHA and the Google [Privacy Policy](https://policies.google.com/privacy) and [Terms of Service](https://policies.google.com/terms) apply.
