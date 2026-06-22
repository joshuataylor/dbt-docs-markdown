# check\_cols

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

## Description[​](#description "Direct link to Description")

A list of columns within the results of your snapshot query to check for changes.

Alternatively, use all columns using the `all` value (however this may be less performant).

This parameter is **required if using the `check` [strategy](https://docs.getdbt.com/reference/resource-configs/strategy.md)**.

## Default[​](#default "Direct link to Default")

No default is provided.

## Examples[​](#examples "Direct link to Examples")

### Check a list of columns for changes[​](#check-a-list-of-columns-for-changes "Direct link to Check a list of columns for changes")

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

### Check all columns for changes[​](#check-all-columns-for-changes "Direct link to Check all columns for changes")

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
