# Can I store my snapshots in a directory other than the \`snapshot\` directory in my project?

By default, dbt expects your snapshot files to be located in the `snapshots` subdirectory of your project.

To change this, update the [snapshot-paths](https://docs.getdbt.com/reference/project-configs/snapshot-paths.md) configuration in your `dbt_project.yml` file, like so:

dbt\_project.yml

```yml
snapshot-paths: ["snapshots"]
```

Note that you cannot co-locate snapshots and models in the same directory.

## Was this page helpful?

YesNo

[Privacy policy](https://www.getdbt.com/cloud/privacy-policy)[Create a GitHub issue](https://github.com/dbt-labs/docs.getdbt.com/issues)

This site is protected by reCAPTCHA and the Google [Privacy Policy](https://policies.google.com/privacy) and [Terms of Service](https://policies.google.com/terms) apply.
