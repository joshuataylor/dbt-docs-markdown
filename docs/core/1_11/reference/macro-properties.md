# Macro properties

You can declare macro properties and configs in `.yml` files in your project. Macro properties<!-- --> are "special properties" in that you can't configure them in the `dbt_project.yml` file or using `config()` blocks. Refer to [Configs and properties](https://docs.getdbt.com/reference/define-properties#which-properties-are-not-also-configs) for more info.

Macros support a `config` block. You can define `meta` and `docs` within `config`.

You can name these files `whatever_you_want.yml` and nest them arbitrarily deep in sub-folders.

macros/\<filename>.yml

```yml

macros:
  - name: <macro name>
    description: <markdown_string>
    config:
      docs:
        show: true | false
      meta: {<dictionary>}
    arguments:
      - name: <arg name>
        type: <string>
        description: <markdown_string>
      - ... # declare properties of additional arguments

  - name: ... # declare properties of additional macros
```

## Example[​](#example "Direct link to Example")

macros/schema.yml

```yaml
macros:
  - name: cents_to_dollars
    description: Converts a numeric column from cents to dollars.
    config:
      docs:
        show: true
      meta:
        owner: analytics
    arguments:
      - name: column_name
        type: column
        description: The column to convert
      - name: precision
        type: integer
        description: Number of decimal places. Defaults to 2.
```

## Was this page helpful?

YesNo

[Privacy policy](https://www.getdbt.com/cloud/privacy-policy)[Create a GitHub issue](https://github.com/dbt-labs/docs.getdbt.com/issues)

This site is protected by reCAPTCHA and the Google [Privacy Policy](https://policies.google.com/privacy) and [Terms of Service](https://policies.google.com/terms) apply.
