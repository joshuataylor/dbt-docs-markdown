# Check configurations

Available in v2

## Related documentation

* [Checks](../docs/build/checks.md)
* [Check properties](./check-properties.md)
* [`dbt check` command](./commands/check.md)

## Available configurations

### Project YAML file

dbt\_project.yml

```yaml
checks:
  <resource-path>:
    +severity: error | warn
    +enabled: true | false
    +selection_filter_on: column_name | [column_names] | none
    +tags: string | [string]
    +meta: {dictionary}
```

Report incorrect code

### Properties YAML file

checks/\_checks.yml

```yaml
version: 2

checks:
  - name: check-name
    config:
      severity: error | warn
      enabled: true | false
      selection_filter_on: column_name | [column_names] | none
      tags: string | [string]
      meta: {dictionary}
```

Report incorrect code

### SQL config

checks/\<check\_name>.sql

```sql
{{ config(
    severity = "error" | "warn",
    enabled = true | false,
    selection_filter_on = "column_name" | ["column_names"] | "none",
    tags = ["string"],
    meta = {"key": "value"}
) }}

select ...
from {{ info_schema('models') }}
where ...
```

Report incorrect code

## Examples

The following examples show common ways to configure checks.

### Warn on check failure

You can use `severity: warn` when rolling out a new rule gradually. Issues are logged but the build does not fail.

checks/\_public\_models\_have\_owners.yml

```yaml
checks:
  - name: public_models_have_owners
    config:
      severity: warn  # default is error
```

Report incorrect code

### Filter by a specific column

The `edges` table has no `unique_id` column, so checks that query it won't return one. When you use `--select`, dbt looks for a `unique_id` column to scope results and finds none, so the check runs against the whole project regardless of the selector. Set `selection_filter_on` to the columns that contain resource IDs so `--select` scopes rows by those columns. For example, the `multiple_sources_joined` check aggregates by `child_unique_id`, so only that column needs to be set:

checks/\_multiple\_sources\_joined.yml

```yaml
checks:
  - name: multiple_sources_joined
    config:
      selection_filter_on: child_unique_id
```

Report incorrect code
