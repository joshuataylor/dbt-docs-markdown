# model-paths

dbt\_project.yml

```yml
model-paths: [directorypath]
```

## Definition[​](#definition "Direct link to Definition")

Optionally specify a custom list of directories where [models](https://docs.getdbt.com/docs/build/models.md), [sources](https://docs.getdbt.com/docs/build/sources.md), and [unit tests](https://docs.getdbt.com/docs/build/unit-tests.md) are located.

## Default[​](#default "Direct link to Default")

By default, dbt will search for models and sources in the `models` directory. For example, `model-paths: ["models"]`.

<!-- -->

Paths specified in `model-paths` must be relative to the location of your `dbt_project.yml` file. Avoid using absolute paths like `/Users/username/project/models`, as it will lead to unexpected behavior and outcomes.

* ✅ **Do**

  * Use relative path:

    <!-- -->

    ```yml
    model-paths: ["models"]
    ```

* ❌ **Don't:**

  * Avoid absolute paths:

    <!-- -->

    ```yml
    model-paths: ["/Users/username/project/models"]
    ```

## Examples[​](#examples "Direct link to Examples")

### Use a subdirectory named `transformations` instead of `models`[​](#use-a-subdirectory-named-transformations-instead-of-models "Direct link to use-a-subdirectory-named-transformations-instead-of-models")

dbt\_project.yml

```yml
model-paths: ["transformations"]
```
