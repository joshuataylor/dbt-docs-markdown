# snapshot-paths

dbt\_project.yml

```yml
snapshot-paths: [directorypath]
```

Report incorrect code

## Definition

Optionally specify a custom list of directories where [snapshots](../../docs/build/snapshots.md) are located.

(Applies to dbt v1.9 and later)

In dbt v1.9+, you can co-locate your snapshots with models if they are [defined using the latest YAML syntax](../../docs/build/snapshots.md).

## Default

By default, dbt will search for snapshots in the `snapshots` directory. For example, `snapshot-paths: ["snapshots"]`.

Paths specified in `snapshot-paths` must be relative to the location of your `dbt_project.yml` file. Avoid using absolute paths like `/Users/username/project/snapshots`, as it will lead to unexpected behavior and outcomes.

* ✅ **Do**

  * Use relative path:

    ```yml
    snapshot-paths: ["snapshots"]
    ```

    Report incorrect code

* ❌ **Don't:**

  * Avoid absolute paths:

    ```yml
    snapshot-paths: ["/Users/username/project/snapshots"]
    ```

    Report incorrect code

## Examples

### Use a subdirectory named `archives` instead of `snapshots`

dbt\_project.yml

```yml
snapshot-paths: ["archives"]
```

Report incorrect code
