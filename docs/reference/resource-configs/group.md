# group

* Models
* Seeds
* Snapshots
* Tests
* Analyses
* Metrics
* Semantic models
* Saved queries

dbt\_project.yml

```yml
models:

  <resource-path>:
    +group: GROUP_NAME
```

models/schema.yml

```yml

models:
  - name: MODEL_NAME
    config:
      group: GROUP # changed to config in v1.10
```

models/\<modelname>.sql

```sql

{{ config(
  group='GROUP_NAME'
) }}

select ...
```

dbt\_project.yml

```yml
models:
  <resource-path>:
    +group: GROUP_NAME
```

seeds/properties.yml

```yml
seeds:
  - name: [SEED_NAME]
    config:
      group: GROUP_NAME # changed to config in v1.10
```

dbt\_project.yml

```yml
snapshots:
  <resource-path>:
    +group: GROUP_NAME
```

snapshots/\<filename>.sql

```sql
{% snapshot snapshot_name %}

{{ config(
  group='GROUP_NAME'
) }}

select ...

{% endsnapshot %}
```

dbt\_project.yml

```yml
data_tests:
  <resource-path>:
    +group: GROUP_NAME
```

tests/properties.yml

```yml

<resource_type>:
  - name: <resource_name>
    data_tests:
      - <test_name>:
          config:
            group: GROUP_NAME
```

tests/\<filename>.sql

```sql
{% test <testname>() %}

{{ config(
  group='GROUP_NAME'
) }}

select ...

{% endtest %}
```

tests/\<filename>.sql

```sql
{{ config(
  group='GROUP_NAME'
) }}
```

analyses/\<filename>.yml

```yml

analyses:
  - name: ANALYSIS_NAME
    config:
      group: GROUP_NAME # changed to config in v1.10
```

dbt\_project.yml

```yaml
metrics:
  <resource-path>:
    +group: GROUP_NAME
```

models/metrics.yml

```yaml

metrics:
  - name: [METRIC_NAME]
    config:
      group: GROUP_NAME
```

dbt\_project.yml

```yaml
semantic-models:
  <resource-path>:
    +group: GROUP_NAME
```

dbt\_project.yml

```yaml
saved-queries:
  <resource-path>:
    +group: GROUP_NAME
```

models/semantic\_models.yml

```yaml
saved_queries:
  - name: SAVED_QUERY_NAME
    config:
      group: GROUP_NAME
```

Note that for backwards compatibility, `group` is supported as a top-level key, but without the capabilities of config inheritance.

## Definition[​](#definition "Direct link to Definition")

An optional configuration for assigning a group to a resource. When a resource is grouped, dbt will allow it to reference private models within the same group.

For more details on reference access between resources in groups, check out [model access](https://docs.getdbt.com/docs/mesh/govern/model-access.md#groups).

## Examples[​](#examples "Direct link to Examples")

### Prevent a 'marketing' group model from referencing a private 'finance' group model[​](#prevent-a-marketing-group-model-from-referencing-a-private-finance-group-model "Direct link to Prevent a 'marketing' group model from referencing a private 'finance' group model")

This is useful if you want to prevent other groups from building on top of models that are rapidly changing, experimental, or otherwise internal to a group or team.

models/schema.yml

```yml
models:
  - name: finance_model
    config:
      group: finance # changed to config in v1.10
      access: private # changed to config in v1.10
  - name: marketing_model
    config:
      group: marketing # changed to config in v1.10
```

models/marketing\_model.sql

```sql
select * from {{ ref('finance_model') }}
```

```shell
$ dbt run -s marketing_model
...
dbt.exceptions.DbtReferenceError: Parsing Error
  Node model.jaffle_shop.marketing_model attempted to reference node model.jaffle_shop.finance_model, 
  which is not allowed because the referenced node is private to the finance group.
```

## Related docs[​](#related-docs "Direct link to Related docs")

* [Model Access](https://docs.getdbt.com/docs/mesh/govern/model-access.md#groups)
* [Defining groups](https://docs.getdbt.com/docs/build/groups.md)

## Was this page helpful?

YesNo

[Privacy policy](https://www.getdbt.com/cloud/privacy-policy)[Create a GitHub issue](https://github.com/dbt-labs/docs.getdbt.com/issues)

This site is protected by reCAPTCHA and the Google [Privacy Policy](https://policies.google.com/privacy) and [Terms of Service](https://policies.google.com/terms) apply.
