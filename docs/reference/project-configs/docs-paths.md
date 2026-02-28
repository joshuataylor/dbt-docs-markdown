# docs-paths

dbt\_project.yml

```yml
docs-paths: [directorypath]
```

## Definition[​](#definition "Direct link to Definition")

Optionally specify a custom list of directories where [docs blocks](https://docs.getdbt.com/docs/build/documentation.md#docs-blocks) are located.

## Default[​](#default "Direct link to Default")

<!-- -->

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

## Was this page helpful?

YesNo

[Privacy policy](https://www.getdbt.com/cloud/privacy-policy)[Create a GitHub issue](https://github.com/dbt-labs/docs.getdbt.com/issues)

This site is protected by reCAPTCHA and the Google [Privacy Policy](https://policies.google.com/privacy) and [Terms of Service](https://policies.google.com/terms) apply.
