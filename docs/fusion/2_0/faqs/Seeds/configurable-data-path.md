# Can I store my seeds in a directory other than the \`seeds\` directory in my project?

By default, dbt expects your seed files to be located in the `seeds` subdirectory of your project.

To change this, update the [seed-paths](https://docs.getdbt.com/reference/project-configs/seed-paths.md) configuration in your `dbt_project.yml` file, like so:

dbt\_project.yml

```yml
seed-paths: ["custom_seeds"]
```
