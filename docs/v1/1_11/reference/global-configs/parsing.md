# Parsing

### Partial Parsing

The `PARTIAL_PARSE` flag can turn partial parsing on or off in your project. See [the docs on parsing](../parsing.md#partial-parsing) for more details.

dbt\_project.yml

```yaml

flags:
  partial_parse: true
```

Usage

```text
dbt run --no-partial-parse
```

### Static parser

The `STATIC_PARSER` config can enable or disable the use of the static parser. See [the docs on parsing](../parsing.md#static-parser) for more details.

profiles.yml

```yaml

config:
  static_parser: true
```

### Opt-in v2 parser
