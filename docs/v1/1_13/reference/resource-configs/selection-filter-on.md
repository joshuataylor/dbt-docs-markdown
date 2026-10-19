# selection\_filter\_on

Available in v2

When you pass a selector (`--select`, `--exclude`, `--selector`) to `dbt check` or `dbt build`, dbt uses it to scope which project resources each check evaluates. `selection_filter_on` tells dbt which column in the check's output contains the resource IDs to match against the selection.

## Values

* **Default (not set):** If the query returns `unique_id`, dbt keeps only rows whose `unique_id` is in the selection. If the query does not return `unique_id`, the check runs against the whole project. Useful for aggregate checks like "the project has at least one model".
* **`none`:** The check always runs against the whole project, ignoring any selector. Use this to make whole-project behavior explicit.
* **A column name or list of column names:** dbt keeps a row if the ID in any of the named columns is in the selection. Each named column must exist in the result, or the check raises an error. Use this for checks that return relationships between resources (edges).

## When to set this config

If your check returns a `unique_id` column, you don't need to set this config.

Use `selection_filter_on` when your check returns rows with ID columns other than `unique_id`. Set it to the columns that contain resource IDs so selectors can scope rows by those columns.

The following check queries the `edges` table and returns `child_unique_id` instead of `unique_id`, so you must set `selection_filter_on`:

checks/multiple\_sources\_joined.sql

```sql
select child_unique_id, count(*) as source_parents
from {{ info_schema('edges') }}
where parent_unique_id like 'source.%'
group by child_unique_id
having count(*) > 1
```

Configure `selection_filter_on` for this check using one of the following methods:

### Project YAML file

dbt\_project.yml

```yaml
checks:
  +selection_filter_on: child_unique_id
```

### Properties YAML file

checks/\_checks.yml

```yaml
version: 2
checks:
  - name: multiple_sources_joined
    description: "Fails if any model reads directly from more than one source."
    config:
      selection_filter_on: child_unique_id
```

### SQL file config

checks/multiple\_sources\_joined.sql

```sql
{{ config(
    selection_filter_on = "child_unique_id"
) }}
```

## Related docs

* [Checks](../../docs/build/checks.md)
* [Check configurations](../check-configs.md)
* [Using selectors with checks](../../docs/build/checks.md#using-selectors-with-checks)
