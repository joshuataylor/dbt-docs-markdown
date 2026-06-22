# Parsing

### Partial Parsing[​](#partial-parsing "Direct link to Partial Parsing")

<!-- -->

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

The `use_v2_parser` flag delegates parsing to the Fusion parser instead of dbt Core's own parser. This is an opt-in flag — it changes no behavior unless explicitly set.

You can enable the v2 parser in three ways:

* CLI flag: `--use-v2-parser`
* Environment variable: `DBT_ENGINE_USE_V2_PARSER=true`
* `dbt_project.yml` under `flags:`:

dbt\_project.yml

```yaml
flags:
  use_v2_parser: true
```

Other behaviors to know about include:

* **Partial parsing**: Partial parsing is disabled when `--use-v2-parser` is set. Any stale `partial_parse.msgpack` from a prior run is automatically removed on entry.
* **`write_manifest`**: `write_manifest` does not work in this mode because the Fusion parser's artifacts (`manifest.json` and `semantic_manifest.json`) are canonical and dbt Core does not re-serialize or overwrite them.
* **Artifacts in `target/`**: When `write_json` is enabled, the handoff `manifest.json` (and `semantic_manifest.json` if present) is copied into your project's `target/` directory.

Usage

```bash
dbt run --use-v2-parser
```

When the v2 parser fails, dbt surfaces these exceptions to make failures easier to diagnose:

| Exception                  | Cause                                                                                                 |
| -------------------------- | ----------------------------------------------------------------------------------------------------- |
| `FusionParserError`        | The parser exited with a non-zero code or produced no output                                          |
| `FusionParserSchemaError`  | The output could not be parsed as valid JSON. Please check your JSON and try again.                   |
| `FusionParserVersionError` | The output manifest uses an incompatible schema version. Make sure you're using Core v1.12 or higher. |

Search table...

|                  |   |   |   |   |
| ---------------- | - | - | - | - |
| Loading table... |   |   |   |   |

Plugin authors

`get_nodes` plugin hooks are not supported when `--use-v2-parser` is enabled.

### Experimental parser[​](#experimental-parser "Direct link to Experimental parser")

Not currently in use.
