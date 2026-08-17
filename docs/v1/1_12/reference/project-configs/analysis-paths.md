# analysis-paths

dbt\_project.yml

```yml
analysis-paths: [directorypath]
```

## Definition

Specify a custom list of directories where [analyses](../../docs/build/analyses.md) are located.

## Default

Without specifying this config, dbt will not compile any `.sql` files as analyses.

However, the [`dbt init` command](../commands/init.md) populates this value as `analyses` ([source](https://github.com/dbt-labs/dbt-starter-project/blob/HEAD/dbt_project.yml#L15)).

Paths specified in `analysis-paths` must be relative to the location of your `dbt_project.yml` file. Avoid using absolute paths like `/Users/username/project/analyses`, as it will lead to unexpected behavior and outcomes.

* ✅ **Do**

  * Use relative path:

    ```yml
    analysis-paths: ["analyses"]
    ```

* ❌ **Don't**

  * Avoid absolute paths:

    ```yml
    analysis-paths: ["/Users/username/project/analyses"]
    ```

## Examples

### Use a subdirectory named `analyses`

This is the value populated by the [`dbt init` command](../commands/init.md).

dbt\_project.yml

```yml
analysis-paths: ["analyses"]
```

### Use a subdirectory named `custom_analyses`

dbt\_project.yml

```yml
analysis-paths: ["custom_analyses"]
```
