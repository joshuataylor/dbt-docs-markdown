# Can I store my models in a directory other than the \`models\` directory in my project?

By default, dbt expects the files defining your models to be located in the `models` subdirectory of your project.

To change this, update the [model-paths](https://docs.getdbt.com/reference/project-configs/model-paths.md) configuration in your `dbt_project.yml` file, like so:

dbt\_project.yml

```yml
model-paths: ["transformations"]
```
