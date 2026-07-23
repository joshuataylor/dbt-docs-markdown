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

dbt Core flag

The v2 parser flag is only applies to dbt Core v1.12 or higher. If you're already on v2, the flag has no impact.

The `use_v2_parser` flag delegates parsing to the v2 parser. This is an opt-in flag.

The v2 parser is the Rust-based parser from the dbt Fusion engine. It's significantly faster than the v1 Python parser, especially on larger projects, where it can be 5–10× quicker. Enabling it can speed up your development workflow and cut down on job startup times. Because it delegates to the parser used in v2.0, it's also a low-risk way to test compatibility with v2 from within dbt Core v1.12.

You can enable the v2 parser in three ways:

* CLI flag: `--use-v2-parser`
* Environment variable: `DBT_ENGINE_USE_V2_PARSER=true`
* `dbt_project.yml` under `flags:`:

dbt\_project.yml

```yaml
flags:
  use_v2_parser: true
```

Note: Partial parsing is disabled when `--use-v2-parser` is set. Any stale `partial_parse.msgpack` from a prior run is automatically removed.

Because the flag only affects project parsing, the fastest way to check v2 parse compatibility is with `dbt parse`. You can also use `--use-v2-parser` with any other command.

Usage

```bash
# Test v2 parser compatibility without running models (recommended)
dbt parse --use-v2-parser

# Or use it with any command
dbt run --use-v2-parser
```

Plugin authors

`get_nodes` plugin hooks are not supported when `--use-v2-parser` is enabled.
