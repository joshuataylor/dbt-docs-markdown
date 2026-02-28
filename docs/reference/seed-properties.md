# Seed properties

Seed properties can be declared in `.yml` files under a `seed` key.

We recommend that you put them in the `seeds/` directory. You can name these files `whatever_you_want.yml`, and nest them arbitrarily deeply in subfolders within that directory.

seeds/\<filename>.yml

```yml

seeds:
  - name: <string>
    description: <markdown_string>
    config:
      <seed_config>: <config_value>
      docs:
        show: true | false
        node_color: <color_id> # Use name (such as node_color: purple) or hex code with quotes (such as node_color: "#cd7f32")
    data_tests:
      - <test>
      - ... # declare additional tests
    columns:
      - name: <column name>
        description: <markdown_string>
        quote: true | false
        data_tests:
          - <test>
          - ... # declare additional tests
        config:
          meta: {<dictionary>}
          tags: [<string>]

      - name: ... # declare properties of additional columns

  - name: ... # declare properties of additional seeds
```

## Was this page helpful?

YesNo

[Privacy policy](https://www.getdbt.com/cloud/privacy-policy)[Create a GitHub issue](https://github.com/dbt-labs/docs.getdbt.com/issues)

This site is protected by reCAPTCHA and the Google [Privacy Policy](https://policies.google.com/privacy) and [Terms of Service](https://policies.google.com/terms) apply.
