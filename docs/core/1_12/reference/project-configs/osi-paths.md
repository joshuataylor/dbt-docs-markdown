# osi-paths

💡Did you know\...

Available from dbt v

<!-- -->

1.12

<!-- -->

or with the

<!-- -->

[dbt "Latest" release track](https://docs.getdbt.com/docs/dbt-versions/dbt-release-tracks.md).

dbt\_project.yml

```yml
osi-paths: [directorypath]
```

## Definition[​](#definition "Direct link to Definition")

Optionally specify a custom list of directories where [Open Semantic Interchange (OSI) semantic layer documents](https://docs.getdbt.com/docs/build/osi-semantic-models.md) are located.

## Default[​](#default "Direct link to Default")

By default, dbt will search for OSI documents in the `OSI` directory, for example, `osi-paths: ["OSI"]`.

<!-- -->

Paths specified in `osi-paths` must be relative to the location of your `dbt_project.yml` file. Avoid using absolute paths like `/Users/username/project/OSI`, as it will lead to unexpected behavior and outcomes.

* ✅ **Do**

  * Use relative path:

    <!-- -->

    ```yml
    osi-paths: ["OSI"]
    ```

* ❌ **Don't:**

  * Avoid absolute paths:

    <!-- -->

    ```yml
    osi-paths: ["/Users/username/project/OSI"]
    ```

## Examples[​](#examples "Direct link to Examples")

Use a subdirectory named `semantic_interchange` instead of `OSI`:

dbt\_project.yml

```yml
osi-paths: ["semantic_interchange"]
```

Use multiple directories to organize your OSI documents:

dbt\_project.yml

```yml
osi-paths: ["OSI", "external_osi"]
```

## Was this page helpful?

YesNo

[Privacy policy](https://www.getdbt.com/cloud/privacy-policy)[Create a GitHub issue](https://github.com/dbt-labs/docs.getdbt.com/issues)

This site is protected by reCAPTCHA and the Google [Privacy Policy](https://policies.google.com/privacy) and [Terms of Service](https://policies.google.com/terms) apply.
