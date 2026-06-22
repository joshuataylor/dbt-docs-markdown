# analysis-paths

dbt\_project.yml

```yml
analysis-paths: [directorypath]
```

## Definition[​](#definition "Direct link to Definition")

Specify a custom list of directories where [analyses](https://docs.getdbt.com/docs/build/analyses.md) are located.

## Default[​](#default "Direct link to Default")

Without specifying this config, dbt will not compile any `.sql` files as analyses.

However, the [`dbt init` command](https://docs.getdbt.com/reference/commands/init.md) populates this value as `analyses` ([source](https://github.com/dbt-labs/dbt-starter-project/blob/HEAD/dbt_project.yml#L15)).

<!-- -->

Paths specified in `analysis-paths` must be relative to the location of your `dbt_project.yml` file. Avoid using absolute paths like `/Users/username/project/analyses`, as it will lead to unexpected behavior and outcomes.

* ✅ **Do**

  * Use relative path:

    <!-- -->

    ```yml
    analysis-paths: ["analyses"]
    ```

* ❌ **Don't**

  * Avoid absolute paths:

    <!-- -->

    ```yml
    analysis-paths: ["/Users/username/project/analyses"]
    ```

## Examples[​](#examples "Direct link to Examples")

### Use a subdirectory named `analyses`[​](#use-a-subdirectory-named-analyses "Direct link to use-a-subdirectory-named-analyses")

This is the value populated by the [`dbt init` command](https://docs.getdbt.com/reference/commands/init.md).

dbt\_project.yml

```yml
analysis-paths: ["analyses"]
```

### Use a subdirectory named `custom_analyses`[​](#use-a-subdirectory-named-custom_analyses "Direct link to use-a-subdirectory-named-custom_analyses")

dbt\_project.yml

```yml
analysis-paths: ["custom_analyses"]
```

## Was this page helpful?

YesNo

[Privacy policy](https://www.getdbt.com/cloud/privacy-policy)[Create a GitHub issue](https://github.com/dbt-labs/docs.getdbt.com/issues)

This site is protected by reCAPTCHA and the Google [Privacy Policy](https://policies.google.com/privacy) and [Terms of Service](https://policies.google.com/terms) apply.
