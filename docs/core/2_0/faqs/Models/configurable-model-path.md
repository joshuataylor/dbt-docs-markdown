# Can I store my models in a directory other than the \`models\` directory in my project?

By default, dbt expects the files defining your models to be located in the `models` subdirectory of your project.

To change this, update the [model-paths](https://docs.getdbt.com/reference/project-configs/model-paths.md) configuration in your `dbt_project.yml` file, like so:

dbt\_project.yml

```yml
model-paths: ["transformations"]
```

## Was this page helpful?

YesNo

[Privacy policy](https://www.getdbt.com/cloud/privacy-policy)[Create a GitHub issue](https://github.com/dbt-labs/docs.getdbt.com/issues)

This site is protected by reCAPTCHA and the Google [Privacy Policy](https://policies.google.com/privacy) and [Terms of Service](https://policies.google.com/terms) apply.
