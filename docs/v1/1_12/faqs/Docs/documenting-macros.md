# How do I document macros?

To document macros, use a [properties file](../../reference/macro-properties.md) and nest the configurations under a `macros:` key

## Example

macros/properties.yml

```yml
macros:
  - name: cents_to_dollars
    description: A macro to convert cents to dollars
    arguments:
      - name: column_name
        type: column
        description: The name of the column you want to convert
      - name: precision
        type: integer
        description: Number of decimal places. Defaults to 2.
```

tip

From dbt Core v1.10, you can opt into validating the arguments you define in macro documentation using the `validate_macro_args` behavior change flag. When enabled, dbt will:

* Infer arguments from the macro and includes them in the [manifest.json](../../reference/artifacts/manifest-json.md) file if no arguments are documented.
* Raise a warning if documented argument names don't match the macro definition.
* Raise a warning if `type` fields don't follow [supported formats](../../reference/resource-properties/arguments.md#supported-types).

Learn more about [macro argument validation](../../reference/global-configs/behavior-flags/validate_macro_args.md).

## Document a custom materialization

When you create a [custom materialization](../../guides/create-new-materializations.md), dbt creates an associated macro with the following format:

```text
materialization_{materialization_name}_{adapter}
```

To document a custom materialization, use the previously mentioned format to determine the associated macro name(s) to document.

macros/properties.yml

```yaml
macros:
  - name: materialization_my_materialization_name_default
    description: A custom materialization to insert records into an append-only table and track when they were added.
  - name: materialization_my_materialization_name_xyz
    description: A custom materialization to insert records into an append-only table and track when they were added.
```
