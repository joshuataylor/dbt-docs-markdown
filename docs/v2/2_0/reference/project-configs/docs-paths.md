# docs-paths

dbt\_project.yml

```yml
docs-paths: [directorypath]
```

## Definition[​](#definition "Direct link to Definition")

Optionally specify a custom list of directories where [docs blocks](../../docs/build/documentation.md#docs-blocks) are located.

## Default[​](#default "Direct link to Default")

By default, dbt will search in all resource paths for docs blocks (for example, the combined list of [model-paths](./model-paths.md), [seed-paths](./seed-paths.md), [analysis-paths](./analysis-paths.md), [test-paths](./test-paths.md), [macro-paths](./macro-paths.md), and [snapshot-paths](./snapshot-paths.md)). If this option is configured, dbt will *only* look in the specified directory for docs blocks.

<!-- -->

Paths specified in `docs-paths` must be relative to the location of your `dbt_project.yml` file. Avoid using absolute paths like `/Users/username/project/docs`, as it will lead to unexpected behavior and outcomes.

* ✅ **Do**

  * Use relative path:

    <!-- -->

    ```yml
    docs-paths: ["docs"]
    ```

* ❌ **Don't**

  * Avoid absolute paths:

    <!-- -->

    ```yml
    docs-paths: ["/Users/username/project/docs"]
    ```

## Example[​](#example "Direct link to Example")

Use a subdirectory named `docs` for docs blocks:

dbt\_project.yml

```yml
docs-paths: ["docs"]
```

**Note:** We typically omit this configuration as we prefer dbt's default behavior.
