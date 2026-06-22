# Can I store my data tests in a directory other than the \`tests\` directory in my project?

By default, dbt expects your singular data test files to be located in the `tests` subdirectory of your project, and generic data test definitions to be located in `tests/generic` or `macros`.

To change this, update the [test-paths](https://docs.getdbt.com/reference/project-configs/test-paths.md) configuration in your `dbt_project.yml` file, like so:

dbt\_project.yml

```yml
test-paths: ["my_cool_tests"]
```

Then, you can define generic data tests in `my_cool_tests/generic/`, and singular data tests everywhere else in `my_cool_tests/`.

## Was this page helpful?

YesNo

[Privacy policy](https://www.getdbt.com/cloud/privacy-policy)[Create a GitHub issue](https://github.com/dbt-labs/docs.getdbt.com/issues)

This site is protected by reCAPTCHA and the Google [Privacy Policy](https://policies.google.com/privacy) and [Terms of Service](https://policies.google.com/terms) apply.
