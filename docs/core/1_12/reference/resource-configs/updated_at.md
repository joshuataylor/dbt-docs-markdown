# updated\_at

snapshots/snapshots.yml

```yaml
snapshots:
  - name: snapshot
    relation: source('my_source', 'my_table')
    config:
      strategy: timestamp
      updated_at: column_name
```

dbt\_project.yml

```yml
snapshots:
  <resource-path>:
    +strategy: timestamp
    +updated_at: column_name
```

caution

You will get a warning if the data type of the `updated_at` column does not match the adapter-configured default.

## Description[​](#description "Direct link to Description")

A column within the results of your snapshot query that represents when the record row was last updated.

This parameter is **required if using the `timestamp` [strategy](https://docs.getdbt.com/reference/resource-configs/strategy.md)**. The `updated_at` field may support ISO date strings and unix epoch integers, depending on the data platform you use.

## Default[​](#default "Direct link to Default")

No default is provided.

## Examples[​](#examples "Direct link to Examples")

### Use a column name `updated_at`[​](#use-a-column-name-updated_at "Direct link to use-a-column-name-updated_at")

snapshots/orders\_snapshot.yml

```yaml
snapshots:
  - name: orders_snapshot
    relation: source('jaffle_shop', 'orders')
    config:
      schema: snapshots
      unique_key: id
      strategy: timestamp
      updated_at: updated_at
```

### Coalesce two columns to create a reliable `updated_at` column[​](#coalesce-two-columns-to-create-a-reliable-updated_at-column "Direct link to coalesce-two-columns-to-create-a-reliable-updated_at-column")

Consider a data source that only has an `updated_at` column filled in when a record is updated (so a `null` value indicates that the record hasn't been updated after it was created).

Since the `updated_at` configuration only takes a column name, rather than an expression, you should update your snapshot query to include the coalesced column.

1. Create an staging model to perform the transformation. In your `models/` directory, create a SQL file that configures an staging model to coalesce the `updated_at` and `created_at` columns into a new column `updated_at_for_snapshot`.

   models/staging\_orders.sql

   ```sql
   select  * coalesce (updated_at, created_at) as updated_at_for_snapshot
   from {{ source('jaffle_shop', 'orders') }}
   ```

2. Define the snapshot configuration in a YAML file. In your `snapshots/` directory, create a YAML file that defines your snapshot and references the `updated_at_for_snapshot` staging model you just created.

   snapshots/orders\_snapshot.yml

   ```yaml
   snapshots:
     - name: orders_snapshot
       relation: ref('staging_orders')
       config:
         schema: snapshots
         unique_key: id
         strategy: timestamp
         updated_at: updated_at_for_snapshot
   ```

3. Run `dbt snapshot` to execute the snapshot.

Alternatively, you can also create an ephemeral model to performs the required transformations. Then, you reference this model in your snapshot's `relation` key.

## Was this page helpful?

YesNo

[Privacy policy](https://www.getdbt.com/cloud/privacy-policy)[Create a GitHub issue](https://github.com/dbt-labs/docs.getdbt.com/issues)

This site is protected by reCAPTCHA and the Google [Privacy Policy](https://policies.google.com/privacy) and [Terms of Service](https://policies.google.com/terms) apply.
