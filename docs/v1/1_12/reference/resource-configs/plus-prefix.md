# Using the + prefix

Use the + prefix to help clarify the difference between resource paths and configs in dbt\_project.yml files.

The `+` prefix is a dbt syntax feature which helps disambiguate between [resource paths](https://docs.getdbt.com/reference/resource-configs/resource-path.md) and configs in [`dbt_project.yml` files](https://docs.getdbt.com/reference/dbt_project.yml.md).

* It is not compatible with `dbt_project.yml` files that use [`config-version`](https://docs.getdbt.com/reference/project-configs/config-version.md) 1.

* It doesn't apply to:

  <!-- -->

  * `config()` Jinja macro within a resource file
  * config property in a `.yml` file.

For example:

dbt\_project.yml

```yml
name: jaffle_shop
config-version: 2

...

models:
  +materialized: view
  jaffle_shop:
    marts:
      +materialized: table
```

Throughout this documentation, we use the `+` prefix on configuration keys in `dbt_project.yml` files.

For projects using [`config-version`](https://docs.getdbt.com/reference/project-configs/config-version.md) 2, dbt expects configuration keys to use the `+` prefix. Specifying configurations without the `+` prefix is [deprecated](https://docs.getdbt.com/reference/deprecations.md#missingplusprefixdeprecation). Folder and file names within resource configurations still do not use the `+` prefix.

The `+` prefix is especially important when you need to disambiguate between [resource paths](https://docs.getdbt.com/reference/resource-configs/resource-path.md) and configs. For example, when:

* A config accepts a dictionary as its input, such as [`persist_docs`](https://docs.getdbt.com/reference/resource-configs/persist_docs.md).
* A config shares a key with part of a resource path, such as a directory of models named `tags`.

<!-- -->

dbt has deprecated specifying configurations without [the `+` prefix](https://docs.getdbt.com/reference/dbt_project.yml#the--prefix) in `dbt_project.yml`. Only folder and file names can be specified without the `+` prefix within resource configurations in `dbt_project.yml`.

<!-- -->

This deprecation warning is only raised for the following adapters:

* Snowflake
* Databricks
* BigQuery
* Redshift

dbt\_project.yml

```yml
name: jaffle_shop
config-version: 2

...

models:
  +persist_docs: # this config is a dictionary, so needs a + prefix
    relation: true
    columns: true

  jaffle_shop:
    +schema: my_schema
    +tags: # this is the tags config
      - "hello"
    config:
      tags: # resource path (models/tags/), not the tags config
        # config: disambiguates path vs. config (required in v1.10+)
        # The below config applies to models in the models/tags/ directory.
        +materialized: view
```

**Note:** The use of the `+` prefix in `dbt_project.yml` is distinct from the use of `+` to control config merge behavior (clobber vs. add) in other config settings (specific resource `.yml` and `.sql` files). Currently, the only config which supports `+` for controlling config merge behavior is [`grants`](https://docs.getdbt.com/reference/resource-configs/grants.md#grant-config-inheritance).
