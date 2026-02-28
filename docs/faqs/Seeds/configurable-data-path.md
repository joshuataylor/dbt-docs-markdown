# Can I store my seeds in a directory other than the \`seeds\` directory in my project?

By default, dbt expects your seed files to be located in the `seeds` subdirectory of your project.

To change this, update the [seed-paths](https://docs.getdbt.com/reference/project-configs/seed-paths.md) configuration in your `dbt_project.yml` file, like so:

dbt\_project.yml

```yml
seed-paths: ["custom_seeds"]
```

## Was this page helpful?

YesNo

[Privacy policy](https://www.getdbt.com/cloud/privacy-policy)[Create a GitHub issue](https://github.com/dbt-labs/docs.getdbt.com/issues)

This site is protected by reCAPTCHA and the Google [Privacy Policy](https://policies.google.com/privacy) and [Terms of Service](https://policies.google.com/terms) apply.
