# About doc function

The `doc` function is used to reference docs blocks in the description field of schema.yml files. It is analogous to the `ref` function. For more information, consult the [Documentation guide](https://docs.getdbt.com/docs/explore/build-and-view-your-docs.md).

Usage:

orders.md

```jinja2

{% docs orders %}

# docs
- go
- here
 
{% enddocs %}
```

schema.yml

```yaml

models:
  - name: orders
    description: "{{ doc('orders') }}"
```

## Was this page helpful?

YesNo

[Privacy policy](https://www.getdbt.com/cloud/privacy-policy)[Create a GitHub issue](https://github.com/dbt-labs/docs.getdbt.com/issues)

This site is protected by reCAPTCHA and the Google [Privacy Policy](https://policies.google.com/privacy) and [Terms of Service](https://policies.google.com/terms) apply.
