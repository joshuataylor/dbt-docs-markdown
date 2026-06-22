# About as\_number filter

The `as_number` Jinja filter will coerce Jinja-compiled output into a numeric value (integer or float), or return an error if it cannot be represented as a number.

### Usage[​](#usage "Direct link to Usage")

In the example below, the `as_number` filter is used to coerce an environment variables into a numeric value to dynamically control the connection port.

profiles.yml

```yml
my_profile:
  outputs:
    dev:
      type: postgres
      port: "{{ env_var('PGPORT') | as_number }}"
```

## Was this page helpful?

YesNo

[Privacy policy](https://www.getdbt.com/cloud/privacy-policy)[Create a GitHub issue](https://github.com/dbt-labs/docs.getdbt.com/issues)

This site is protected by reCAPTCHA and the Google [Privacy Policy](https://policies.google.com/privacy) and [Terms of Service](https://policies.google.com/terms) apply.
