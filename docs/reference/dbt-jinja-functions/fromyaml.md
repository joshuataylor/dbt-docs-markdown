# About fromyaml context method

The `fromyaml` context method can be used to deserialize a YAML string into a Python object primitive, eg. a `dict` or `list`.

**Args**:

* `string`: The YAML string to deserialize (required)
* `default`: A default value to return if the `string` argument cannot be deserialized (optional)

### Usage:[​](#usage "Direct link to Usage:")

```text
{% set my_yml_str -%}

dogs:
 - good
 - bad

{%- endset %}

{% set my_dict = fromyaml(my_yml_str) %}

{% do log(my_dict['dogs'], info=true) %}
-- ["good", "bad"]

{% do my_dict['dogs'].pop() %}
{% do log(my_dict['dogs'], info=true) %}
-- ["good"]
```

## Was this page helpful?

YesNo

[Privacy policy](https://www.getdbt.com/cloud/privacy-policy)[Create a GitHub issue](https://github.com/dbt-labs/docs.getdbt.com/issues)

This site is protected by reCAPTCHA and the Google [Privacy Policy](https://policies.google.com/privacy) and [Terms of Service](https://policies.google.com/terms) apply.
