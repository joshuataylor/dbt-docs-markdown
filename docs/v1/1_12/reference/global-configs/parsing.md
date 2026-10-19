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

(Applies to dbt v1.12 and later)

dbt v1 flag

The v2 parser flag only applies to dbt v1.12 and higher 1.x versions. If you're already on v2, the flag has no impact.

The `use_v2_parser` flag delegates parsing to the v2 parser. This is an opt-in flag.

The v2 parser is the Rust-based parser from dbt v2. It's significantly faster than the v1 Python parser, especially on larger projects, where it can be 5–10× quicker. Enabling it can speed up your development workflow and cut down on job startup times. Because it delegates to the parser used in v2.0, it's also a low-risk way to test compatibility with v2 from within dbt v1.12.

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
