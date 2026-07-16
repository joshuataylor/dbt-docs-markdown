# Macro argument validation

| validate\_macro\_args      | dbt **Latest** | dbt Core |
| -------------------------- | -------------- | -------- |
| Introduced                 | 2025.03        | 1.10.0   |
| Matured (default → `true`) | Sep 1, 2026    | 1.12.0   |
| Removed                    | —              | —        |

Search table...

|                  |   |   |   |   |
| ---------------- | - | - | - | - |
| Loading table... |   |   |   |   |

<br />

dbt validates macro arguments using the `validate_macro_args` flag. Starting in dbt Core v1.12, this flag defaults to `true`.

In the past, dbt didn't enforce a standard vocabulary for the [`type`](https://docs.getdbt.com/reference/resource-properties/arguments.md#type) field on macro arguments in YAML. Because of this, the `type` field was used for documentation only, and dbt didn't check that:

* the argument names matched those in your macro
* the argument types were valid or consistent with the macro's Jinja definition

Here's an example of a documented macro:

macros/filename.yml

```yaml
macros:
  - name: <macro name>
    arguments:
      - name: <arg name>
        type: <string>
```

When you set the `validate_macro_args` flag to `true`, dbt will:

* Validate macro arguments during project parsing.
* Check that all argument names in your YAML match those in the macro definition.
* Raise warnings if the names or types don't match.
* Validate that the [`type` values follow the supported format](https://docs.getdbt.com/reference/resource-properties/arguments.md#supported-types).
* If no arguments are documented in the YAML, infer them from the macro and include them in the [`manifest.json` file](https://docs.getdbt.com/reference/artifacts/manifest-json.md).

 When does validation occur?

Macro argument validation runs during project parsing, not during macro execution. Any dbt command that parses the project will trigger validation if you enable the `validate_macro_args` flag.

* In dbt Core:

  <!-- -->

  * Validation runs as part of parsing for most commands (`parse`, `build`, `run`, `test`, `seed`, `snapshot`, `compile`).
  * With a full parse, dbt validates all macros.
  * With partial parsing (the default), dbt validates only macros affected by changed files.
  * Use `--no-partial-parse` to force validation of all macros.

## Impact[​](#impact "Direct link to Impact")

On its own, the flag emits warnings and builds continue. However, these warnings use the force-handled path and respect `--warn-error`, so projects with `--warn-error` set will see build failures at parse time.

This affects projects where the `arguments:` listed in a macro's YAML patch no longer match the macro's actual Jinja signature. For those projects, every command fails at parse time until you either update the YAML arguments to match the macro or remove the `arguments:` block entirely.

 Recommended actions

If `InvalidMacroAnnotation` warnings are appearing at parse time (or causing parse failures with `--warn-error` set), check each log line — it names the macro and the specific mismatch. In the `macros:` YAML block for each affected macro, align the `arguments:` entries with the `{% macro name(args) %}` declaration in the `.sql` file, or remove the `arguments:` block entirely.

To silence the warnings without fixing the YAML, use `warn_error_options`:

dbt\_project.yml

```yaml
flags:
  warn_error_options:
    silence:
      - InvalidMacroAnnotation
```

To opt out of this behavior, set the flag to `false`:

dbt\_project.yml

```yaml
flags:
  validate_macro_args: false
```
