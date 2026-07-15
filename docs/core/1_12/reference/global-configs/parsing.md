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

dbt Core flag

The v2 parser flag is only supported on dbt Core v1.12 or higher. If you're already on Fusion, the flag has no impact.

The `use_v2_parser` flag delegates parsing to the Fusion parser instead of dbt Core's own parser. This is an opt-in flag — it changes no behavior unless explicitly set.

The v2 parser is the Rust-based parser from the dbt Fusion engine. It's significantly faster than dbt Core's Python parser, especially on larger projects, where it can be 5–10× quicker. Enabling it can speed up your development workflow and cut down on job startup times. Because it delegates to the Fusion parser used in v2.0, it's also a low-risk way to test Fusion compatibility from within dbt Core v1.12.

When enabled, dbt hands parsing off to the Fusion parser, loads the `manifest.json` it produces, and skips dbt Core's own parser entirely.

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

* **Partial parsing**: Partial parsing is disabled when `--use-v2-parser` is set. Any stale `partial_parse.msgpack` from a prior run is automatically removed.
* **`write_manifest`**: `write_manifest` does not work in this mode because the Fusion parser's artifacts (`manifest.json` and `semantic_manifest.json`) are canonical and dbt Core does not re-serialize or overwrite them.
* **Artifacts in `target/`**: When `write_json` is enabled, the handoff `manifest.json` (and `semantic_manifest.json` if present) is copied into your project's `target/` directory.

Because the flag only changes *how* your project is parsed, the lightest way to check Fusion parser compatibility is `dbt parse`. You can also pass `--use-v2-parser` with any other command.

Usage

```bash
# Test Fusion parser compatibility without running models (recommended)
dbt parse --use-v2-parser

# Or use it with any command
dbt run --use-v2-parser
```

When the v2 parser fails, dbt surfaces these exceptions to make failures easier to diagnose:

| Exception                  | Cause                                                                                                    |
| -------------------------- | -------------------------------------------------------------------------------------------------------- |
| `FusionParserError`        | The parser exited with a non-zero code, produced no manifest output, or couldn't be found.               |
| `FusionParserSchemaError`  | dbt couldn't load the `manifest.json` the parser produced (for example, it was malformed or unreadable). |
| `FusionParserVersionError` | The parser's manifest uses an incompatible schema version. Make sure you're on dbt Core v1.12 or higher. |

Search table...

|                  |   |   |   |   |
| ---------------- | - | - | - | - |
| Loading table... |   |   |   |   |

Plugin authors

`get_nodes` plugin hooks are not supported when `--use-v2-parser` is enabled.
