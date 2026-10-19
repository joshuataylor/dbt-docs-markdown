# About the info\_schema macro

Available in v2

`{{ info_schema('<view_name>') }}` is the supported way to reference [dbt Information Schema](../../docs/build/dbt-information-schema.md) tables in a [check](../../docs/build/checks.md) or with [`dbt show --inline`](../../docs/build/dbt-information-schema.md#querying-with-dbt-show).

When you pass the name of the view you want to query (for example, `{{ info_schema('models') }}`), the macro reads from a logical view layer built at parse time.

For example, to find models without a description:

```sql
select name
from {{ info_schema('models') }}
where description = ''
```

For the full list of columns available for each view, refer to [Columns available for checks](../info-schema.md#columns-available-for-checks).

note

Checks can only access parse-time project metadata. Only views whose columns are fully populated at parse time are available. Passing a view name that doesn't exist or isn't available at parse time causes the check to fail with a message listing what is available.
