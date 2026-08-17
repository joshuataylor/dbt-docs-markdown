# macro-paths

dbt\_project.yml

```yml
macro-paths: [directorypath]
```

## Definition

Optionally specify a custom list of directories where [macros](../../docs/build/jinja-macros.md#macros) are located. Note that you cannot co-locate models and macros.

## Default

By default, dbt will search for macros in a directory named `macros`. For example, `macro-paths: ["macros"]`.

Paths specified in `macro-paths` must be relative to the location of your `dbt_project.yml` file. Avoid using absolute paths like `/Users/username/project/macros`, as it will lead to unexpected behavior and outcomes.

* ✅ **Do**

  * Use relative path:

    ```yml
    macro-paths: ["macros"]
    ```

* ❌ **Don't:**

  * Avoid absolute paths:

    ```yml
    macro-paths: ["/Users/username/project/macros"]
    ```

## Examples

### Use a subdirectory named `custom_macros` instead of `macros`

dbt\_project.yml

```yml
macro-paths: ["custom_macros"]
```
