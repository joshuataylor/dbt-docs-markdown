# About fromjson context method

The `fromjson` context method can be used to deserialize a JSON string into a Python object primitive, eg. a `dict` or `list`.

**Args**:

* `string`: The JSON string to deserialize (required)
* `default`: A default value to return if the `string` argument cannot be deserialized (optional)

### Usage:[​](#usage "Direct link to Usage:")

```text
{% set my_json_str = '{"abc": 123}' %}
{% set my_dict = fromjson(my_json_str) %}

{% do log(my_dict['abc']) %}
```

## Was this page helpful?

YesNo

[Privacy policy](https://www.getdbt.com/cloud/privacy-policy)[Create a GitHub issue](https://github.com/dbt-labs/docs.getdbt.com/issues)

This site is protected by reCAPTCHA and the Google [Privacy Policy](https://policies.google.com/privacy) and [Terms of Service](https://policies.google.com/terms) apply.
