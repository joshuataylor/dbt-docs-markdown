# osi-paths

💡Did you know\...

Available from dbt v1.12 or with the [dbt "Latest" release track](../../docs/dbt-versions/dbt-release-tracks.md).

dbt\_project.yml

```yml
osi-paths: [directorypath]
```

## Definition

Optionally specify a custom list of directories where [Apache Ossie semantic layer documents](../../docs/build/ossie-semantic-models.md) are located.

## Default

By default, dbt will search for Ossie documents in the `OSI` directory, for example, `osi-paths: ["OSI"]`.

Paths specified in `osi-paths` must be relative to the location of your `dbt_project.yml` file. Avoid using absolute paths like `/Users/username/project/OSI`, as it will lead to unexpected behavior and outcomes.

* ✅ **Do**

  * Use relative path:

    ```yml
    osi-paths: ["OSI"]
    ```

* ❌ **Don't:**

  * Avoid absolute paths:

    ```yml
    osi-paths: ["/Users/username/project/OSI"]
    ```

## Examples

Use a subdirectory named `semantic_interchange` instead of `OSI`:

dbt\_project.yml

```yml
osi-paths: ["semantic_interchange"]
```

Use multiple directories to organize your Ossie documents:

dbt\_project.yml

```yml
osi-paths: ["OSI", "external_osi"]
```
