# check\_cols

(Applies to dbt v1.9 and later)

snapshots/\<filename>.yml

```yml
snapshots:
- name: snapshot_name
  relation: source('my_source', 'my_table')
  config:
    schema: string
    unique_key: column_name_or_expression
    strategy: check
    check_cols:
      - column_name
```

dbt\_project.yml

```yml
snapshots:
  <resource-path>:
    +strategy: check
    +check_cols: [column_name] | all
```

## Description

A list of columns within the results of your snapshot query to check for changes.

Alternatively, use all columns using the `all` value (however this may be less performant).

This parameter is **required if using the `check` [strategy](./strategy.md)**.

## Default

No default is provided.

## Examples

### Check a list of columns for changes

(Applies to dbt v1.9 and later)

snapshots/orders\_snapshot\_check.yml

```yaml
snapshots:
  - name: orders_snapshot_check
    relation: source('jaffle_shop', 'orders')
    config:
      schema: snapshots
      unique_key: id
      strategy: check
      check_cols:
        - status
        - is_cancelled
```

To select from this snapshot in a downstream model: `select * from {{ ref('orders_snapshot_check') }}`

### Check all columns for changes

(Applies to dbt v1.9 and later)

orders\_snapshot\_check.yml

```yaml
snapshots:
  - name: orders_snapshot_check
    relation: source('jaffle_shop', 'orders')
    config:
      schema: snapshots
      unique_key: id
      strategy: check
      check_cols: all
```

To select from this snapshot in a downstream model: `select * from {{ ref('orders_snapshot_check') }}`
