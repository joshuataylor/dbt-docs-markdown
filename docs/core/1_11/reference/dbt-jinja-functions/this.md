# about this

`this` is the database representation of the current model. It is useful when:

* Defining a `where` statement within [incremental models](https://docs.getdbt.com/docs/build/incremental-models.md)
* Using [pre or post hooks](https://docs.getdbt.com/reference/resource-configs/pre-hook-post-hook.md)

`this` is a [Relation](https://docs.getdbt.com/reference/dbt-classes.md#relation), and as such, properties such as `{{ this.database }}` and `{{ this.schema }}` compile as expected.

* Note — Prior to dbt v1.6,
  <!-- -->
  returns `request` as the result of `{{ ref.identifier }}`.

`this` can be thought of as equivalent to `ref('<the_current_model>')`, and is a neat way to avoid circular dependencies.

## Examples[​](#examples "Direct link to Examples")

### Configuring incremental models[​](#configuring-incremental-models "Direct link to Configuring incremental models")

models/stg\_events.sql

```sql
{{ config(materialized='incremental') }}

select
    *,
    my_slow_function(my_column)

from raw_app_data.events

{% if is_incremental() %}
  where event_time > (select max(event_time) from {{ this }})
{% endif %}
```

## Was this page helpful?

YesNo

[Privacy policy](https://www.getdbt.com/cloud/privacy-policy)[Create a GitHub issue](https://github.com/dbt-labs/docs.getdbt.com/issues)

This site is protected by reCAPTCHA and the Google [Privacy Policy](https://policies.google.com/privacy) and [Terms of Service](https://policies.google.com/terms) apply.
