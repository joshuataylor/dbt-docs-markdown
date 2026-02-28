# test-paths

dbt\_project.yml

```yml
test-paths: [directorypath]
```

## Definition[​](#definition "Direct link to Definition")

Optionally specify a custom list of directories where [singular tests](https://docs.getdbt.com/docs/build/data-tests.md#singular-data-tests) and [custom generic tests](https://docs.getdbt.com/docs/build/data-tests.md#generic-data-tests) are located.

## Default[​](#default "Direct link to Default")

Without specifying this config, dbt will search for tests in the `tests` directory, i.e. `test-paths: ["tests"]`. Specifically, it will look for `.sql` files containing:

* Generic test definitions in the `tests/generic` subdirectory
* Singular tests (all other files)

<!-- -->

Paths specified in `test-paths` must be relative to the location of your `dbt_project.yml` file. Avoid using absolute paths like `/Users/username/project/test`, as it will lead to unexpected behavior and outcomes.

* ✅ **Do**

  * Use relative path:

    <!-- -->

    ```yml
    test-paths: ["test"]
    ```

* ❌ **Don't:**

  * Avoid absolute paths:

    <!-- -->

    ```yml
    test-paths: ["/Users/username/project/test"]
    ```

## Examples[​](#examples "Direct link to Examples")

### Use a subdirectory named `custom_tests` instead of `tests` for data tests[​](#use-a-subdirectory-named-custom_tests-instead-of-tests-for-data-tests "Direct link to use-a-subdirectory-named-custom_tests-instead-of-tests-for-data-tests")

dbt\_project.yml

```yml
test-paths: ["custom_tests"]
```

## Was this page helpful?

YesNo

[Privacy policy](https://www.getdbt.com/cloud/privacy-policy)[Create a GitHub issue](https://github.com/dbt-labs/docs.getdbt.com/issues)

This site is protected by reCAPTCHA and the Google [Privacy Policy](https://policies.google.com/privacy) and [Terms of Service](https://policies.google.com/terms) apply.
