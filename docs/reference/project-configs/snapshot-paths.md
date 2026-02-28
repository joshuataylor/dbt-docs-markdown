# snapshot-paths

dbt\_project.yml

```yml
snapshot-paths: [directorypath]
```

## Definition[​](#definition "Direct link to Definition")

Optionally specify a custom list of directories where [snapshots](https://docs.getdbt.com/docs/build/snapshots.md) are located.

<!-- -->

## Default[​](#default "Direct link to Default")

By default, dbt will search for snapshots in the `snapshots` directory. For example, `snapshot-paths: ["snapshots"]`.

<!-- -->

Paths specified in `snapshot-paths` must be relative to the location of your `dbt_project.yml` file. Avoid using absolute paths like `/Users/username/project/snapshots`, as it will lead to unexpected behavior and outcomes.

* ✅ **Do**

  * Use relative path:

    <!-- -->

    ```yml
    snapshot-paths: ["snapshots"]
    ```

* ❌ **Don't:**

  * Avoid absolute paths:

    <!-- -->

    ```yml
    snapshot-paths: ["/Users/username/project/snapshots"]
    ```

## Examples[​](#examples "Direct link to Examples")

### Use a subdirectory named `archives` instead of `snapshots`[​](#use-a-subdirectory-named-archives-instead-of-snapshots "Direct link to use-a-subdirectory-named-archives-instead-of-snapshots")

dbt\_project.yml

```yml
snapshot-paths: ["archives"]
```

## Was this page helpful?

YesNo

[Privacy policy](https://www.getdbt.com/cloud/privacy-policy)[Create a GitHub issue](https://github.com/dbt-labs/docs.getdbt.com/issues)

This site is protected by reCAPTCHA and the Google [Privacy Policy](https://policies.google.com/privacy) and [Terms of Service](https://policies.google.com/terms) apply.
