# snapshot\_name

snapshots/\<filename>.yml

```yaml
snapshots:
  - name: snapshot_name
    relation: source('my_source', 'my_table')
    config:
      schema: string
      database: string
      unique_key: column_name_or_expression
      strategy: timestamp | check
      updated_at: column_name  # Required if strategy is 'timestamp'
```

## Description[​](#description "Direct link to Description")

The name of a snapshot, which is used when selecting from a snapshot using the [`ref` function](https://docs.getdbt.com/reference/dbt-jinja-functions/ref.md)

This name must not conflict with the name of any other "refable" resource (models, seeds, other snapshots) defined in this project or package.

The name does not need to match the file name. As a result, snapshot filenames do not need to be unique.

## Examples[​](#examples "Direct link to Examples")

### Name a snapshot `order_snapshot`[​](#name-a-snapshot-order_snapshot "Direct link to name-a-snapshot-order_snapshot")

snapshots/order\_snapshot.yml

```yaml
snapshots:
  - name: order_snapshot
    relation: source('my_source', 'my_table')
    config:
      schema: string
      database: string
      unique_key: column_name_or_expression
      strategy: timestamp | check
      updated_at: column_name  # Required if strategy is 'timestamp'
```

To select from this snapshot in a downstream model:

```sql
select * from {{ ref('orders_snapshot') }}
```

## Was this page helpful?

YesNo

[Privacy policy](https://www.getdbt.com/cloud/privacy-policy)[Create a GitHub issue](https://github.com/dbt-labs/docs.getdbt.com/issues)

This site is protected by reCAPTCHA and the Google [Privacy Policy](https://policies.google.com/privacy) and [Terms of Service](https://policies.google.com/terms) apply.
