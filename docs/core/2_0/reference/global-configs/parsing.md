# Parsing

### Partial Parsing[​](#partial-parsing "Direct link to Partial Parsing")

Fusion and partial parsing

Fusion job runs no longer support the `--partial-parse` and `--no-partial-parse` CLI flags. If you pass them (for example, from a dbt Core command or script), dbt logs deprecation warning `dbt1700`. Remove these flags from your Fusion job commands. For more information, refer to [Deprecated flags](https://docs.getdbt.com/docs/dbt-versions/core-upgrade/upgrading-to-v2.md#deprecated-flags) in the guide to upgrading to the dbt Fusion engine.

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

### Opt-in v2 parser[​](#opt-in-v2-parser "Direct link to Opt-in v2 parser")

<!-- -->

### Experimental parser[​](#experimental-parser "Direct link to Experimental parser")

Not currently in use.
