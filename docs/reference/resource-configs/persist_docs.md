# persist\_docs

* Models
* Sources
* Seeds
* Snapshots

dbt\_project.yml

```yml
models:
  <resource-path>:
    +persist_docs:
      relation: true
      columns: true
```

models/\<modelname>.sql

```sql

{{ config(
  persist_docs={"relation": true, "columns": true}
) }}

select ...
```

This config is not implemented for sources.

dbt\_project.yml

```yml
seeds:
  <resource-path>:
    +persist_docs:
      relation: true
      columns: true
```

dbt\_project.yml

```yml
snapshots:
  <resource-path>:
    +persist_docs:
      relation: true
      columns: true
```

snapshots/\<filename>.sql

```sql
{% snapshot snapshot_name %}

{{ config(
  persist_docs={"relation": true, "columns": true}
) }}

select ...

{% endsnapshot %}
```

## Definition[​](#definition "Direct link to Definition")

Optionally persist [resource descriptions](https://docs.getdbt.com/reference/resource-properties/description.md) as column and relation comments in the database. By default, documentation persistence is disabled, but it can be enabled for specific resources or groups of resources as needed.

## Support[​](#support "Direct link to Support")

The `persist_docs` config is supported on the most widely used dbt adapters:

* Postgres
* Redshift
* Snowflake
* BigQuery
* Databricks
* Apache Spark

However, some databases limit where and how descriptions can be added to database objects. Those database adapters might not support `persist_docs`, or might offer only partial support.

Some known issues and limitations:

* Databricks
* Snowflake

- Column-level comments require `file_format: delta` (or another "v2 file format").

* If a column name in a SQL model is in a mixed-case format (for example, `ca_net_ht_N`), the docs for that column will not be persisted. For the docs to persist, there are two options:

  * Define the column name in the corresponding YML file using lowercase or uppercase letters only.
  * Use the [`quote`](https://docs.getdbt.com/reference/resource-properties/columns.md#quoter) configuration in the corresponding YML file.

  See the following sample steps on how to use the `quote` field for columns in a mixed-case format.

  1. Create the following SQL and YML files:

     \<modelname>.sql

     ```sql
     {{ config(materialized='table') }}

     select 1 as "ca_net_ht_N" # note the use of double quotes for the column name
     ```

     \<modelname>.yml

     ```yml
     models:
       - name: <modelname>
         description: This is the table description

     columns:
       - name: "ca_net_ht_N"
         description: This should be the description of the column
         quote: true
     ```

  2. Run `dbt build -s models/<modelname>.sql --full-refresh`.

  3. Open the logs at `logs/dbt.log` and check the column description:

     ```log
     alter table analytics.<schema>.<modelname> alter
         "ca_net_ht_N" COMMENT $$This should be the description of the column$$;
     ```

## Usage[​](#usage "Direct link to Usage")

### Documenting columns and relations[​](#documenting-columns-and-relations "Direct link to Documenting columns and relations")

Supply a [description](https://docs.getdbt.com/reference/resource-properties/description.md) for a model:

models/schema.yml

```yml

models:
  - name: dim_customers
    description: One record per customer
    columns:
      - name: customer_id
        description: Primary key
```

Enable `persist_docs` for columns and relations in your project:

dbt\_project.yml

```yml
models:
  +persist_docs:
    relation: true
    columns: true
```

Run dbt and observe that the created relation and columns are annotated with your descriptions:

[![Relation descriptions in BigQuery](/img/reference/persist_docs_relation.png?v=2 "Relation descriptions in BigQuery")](#)Relation descriptions in BigQuery

[![Column descriptions in BigQuery](/img/reference/persist_docs_columns.png?v=2 "Column descriptions in BigQuery")](#)Column descriptions in BigQuery

## Was this page helpful?

YesNo

[Privacy policy](https://www.getdbt.com/cloud/privacy-policy)[Create a GitHub issue](https://github.com/dbt-labs/docs.getdbt.com/issues)

This site is protected by reCAPTCHA and the Google [Privacy Policy](https://policies.google.com/privacy) and [Terms of Service](https://policies.google.com/terms) apply.
