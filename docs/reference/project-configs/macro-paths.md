# macro-paths

dbt\_project.yml

```yml
macro-paths: [directorypath]
```

## Definition[​](#definition "Direct link to Definition")

Optionally specify a custom list of directories where [macros](https://docs.getdbt.com/docs/build/jinja-macros.md#macros) are located. Note that you cannot co-locate models and macros.

## Default[​](#default "Direct link to Default")

By default, dbt will search for macros in a directory named `macros`. For example, `macro-paths: ["macros"]`.

<!-- -->

Paths specified in `macro-paths` must be relative to the location of your `dbt_project.yml` file. Avoid using absolute paths like `/Users/username/project/macros`, as it will lead to unexpected behavior and outcomes.

* ✅ **Do**

  * Use relative path:

    <!-- -->

    ```yml
    macro-paths: ["macros"]
    ```

* ❌ **Don't:**

  * Avoid absolute paths:

    <!-- -->

    ```yml
    macro-paths: ["/Users/username/project/macros"]
    ```

## Examples[​](#examples "Direct link to Examples")

### Use a subdirectory named `custom_macros` instead of `macros`[​](#use-a-subdirectory-named-custom_macros-instead-of-macros "Direct link to use-a-subdirectory-named-custom_macros-instead-of-macros")

dbt\_project.yml

```yml
macro-paths: ["custom_macros"]
```

## Was this page helpful?

YesNo

[Privacy policy](https://www.getdbt.com/cloud/privacy-policy)[Create a GitHub issue](https://github.com/dbt-labs/docs.getdbt.com/issues)

This site is protected by reCAPTCHA and the Google [Privacy Policy](https://policies.google.com/privacy) and [Terms of Service](https://policies.google.com/terms) apply.
