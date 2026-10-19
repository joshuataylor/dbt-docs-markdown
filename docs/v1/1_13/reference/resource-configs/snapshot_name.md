# snapshot\_name

(Applies to dbt v1.9 and later)

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

## Description

The name of a snapshot, which is used when selecting from a snapshot using the [`ref` function](../dbt-jinja-functions/ref.md)

This name must not conflict with the name of any other "refable" resource (models, seeds, other snapshots) defined in this project or package.

The name does not need to match the file name. As a result, snapshot filenames do not need to be unique.

## Examples

### Name a snapshot `order_snapshot`

(Applies to dbt v1.9 and later)

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
