# Parsing

### Partial Parsing[​](#partial-parsing "Direct link to Partial Parsing")

The `PARTIAL_PARSE` flag can turn partial parsing on or off in your project. See [the docs on parsing](https://docs.getdbt.com/reference/parsing.md#partial-parsing) for more details.

dbt\_project.yml

```yaml

flags:
  partial_parse: true
```

Usage

```text
dbt run --no-partial-parse
```

### Static parser[​](#static-parser "Direct link to Static parser")

The `STATIC_PARSER` config can enable or disable the use of the static parser. See [the docs on parsing](https://docs.getdbt.com/reference/parsing.md#static-parser) for more details.

profiles.yml

```yaml

config:
  static_parser: true
```

### Experimental parser[​](#experimental-parser "Direct link to Experimental parser")

Not currently in use.

## Was this page helpful?

YesNo

[Privacy policy](https://www.getdbt.com/cloud/privacy-policy)[Create a GitHub issue](https://github.com/dbt-labs/docs.getdbt.com/issues)

This site is protected by reCAPTCHA and the Google [Privacy Policy](https://policies.google.com/privacy) and [Terms of Service](https://policies.google.com/terms) apply.
