# What model configurations exist?

You can also configure:

* [tags](https://docs.getdbt.com/reference/resource-configs/tags.md) to support easy categorization and graph selection
* [custom schemas](https://docs.getdbt.com/reference/resource-properties/schema.md) to split your models across multiple schemas
* [aliases](https://docs.getdbt.com/reference/resource-configs/alias.md) if your view/table name should differ from the filename
* Snippets of SQL to run at the start or end of a model, known as [hooks](https://docs.getdbt.com/docs/build/hooks-operations.md)
* Warehouse-specific configurations for performance (e.g. `sort` and `dist` keys on Redshift, `partitions` on BigQuery)

Check out the docs on [model configurations](https://docs.getdbt.com/reference/model-configs.md) to learn more.
