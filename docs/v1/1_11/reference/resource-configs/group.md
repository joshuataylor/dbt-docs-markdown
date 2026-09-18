# group

## Models

dbt\_project.yml

```yml
models:

  <resource-path>:
    +group: GROUP_NAME
```

Report incorrect code

models/schema.yml

```yml

models:
  - name: MODEL_NAME
    config:
      group: GROUP # changed to config in v1.10
```

Report incorrect code

models/\<modelname>.sql

```sql

{{ config(
  group='GROUP_NAME'
) }}

select ...
```

Report incorrect code

## Seeds

dbt\_project.yml

```yml
models:
  <resource-path>:
    +group: GROUP_NAME
```

Report incorrect code

seeds/properties.yml

```yml
seeds:
  - name: [SEED_NAME]
    config:
      group: GROUP_NAME # changed to config in v1.10
```

Report incorrect code

## Snapshots

dbt\_project.yml

```yml
snapshots:
  <resource-path>:
    +group: GROUP_NAME
```

Report incorrect code

(Applies to dbt v1.9 and later)

snapshots/properties.yml

```yaml

snapshots:
  - name: snapshot_name
    config:
      group: GROUP_NAME
```

Report incorrect code

snapshots/\<filename>.sql

```sql
{% snapshot snapshot_name %}

{{ config(
  group='GROUP_NAME'
) }}

select ...

{% endsnapshot %}
```

Report incorrect code

## Tests

dbt\_project.yml

```yml
data_tests:
  <resource-path>:
    +group: GROUP_NAME
```

Report incorrect code

tests/properties.yml

```yml

<resource_type>:
  - name: <resource_name>
    data_tests:
      - <test_name>:
          config:
            group: GROUP_NAME
```

Report incorrect code

tests/\<filename>.sql

```sql
{% test <testname>() %}

{{ config(
  group='GROUP_NAME'
) }}

select ...

{% endtest %}
```

Report incorrect code

tests/\<filename>.sql

```sql
{{ config(
  group='GROUP_NAME'
) }}
```

Report incorrect code

## Analyses

analyses/\<filename>.yml

```yml

analyses:
  - name: ANALYSIS_NAME
    config:
      group: GROUP_NAME # changed to config in v1.10
```

Report incorrect code

## Metrics

dbt\_project.yml

```yaml
metrics:
  <resource-path>:
    +group: GROUP_NAME
```

Report incorrect code

models/metrics.yml

```yaml

metrics:
  - name: [METRIC_NAME]
    config:
      group: GROUP_NAME
```

Report incorrect code

## Semantic models

dbt\_project.yml

```yaml
semantic-models:
  <resource-path>:
    +group: GROUP_NAME
```

Report incorrect code

(Applies to dbt v1.11 and earlier)

models/semantic\_models.yml

```yaml
semantic_models:
  - name: SEMANTIC_MODEL_NAME
    config:
      group: GROUP_NAME
```

Report incorrect code

## Saved queries

dbt\_project.yml

```yaml
saved-queries:
  <resource-path>:
    +group: GROUP_NAME
```

Report incorrect code

models/semantic\_models.yml

```yaml
saved_queries:
  - name: SAVED_QUERY_NAME
    config:
      group: GROUP_NAME
```

Report incorrect code

Note that for backwards compatibility, `group` is supported as a top-level key, but without the capabilities of config inheritance.

## Definition

An optional configuration for assigning a group to a resource. When a resource is grouped, dbt will allow it to reference private models within the same group.

For more details on reference access between resources in groups, check out [model access](../../docs/mesh/govern/model-access.md#groups).

## Examples

### Prevent a 'marketing' group model from referencing a private 'finance' group model

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

Report incorrect code

models/marketing\_model.sql

```sql
select * from {{ ref('finance_model') }}
```

Report incorrect code

```shell
$ dbt run -s marketing_model
...
dbt.exceptions.DbtReferenceError: Parsing Error
  Node model.jaffle_shop.marketing_model attempted to reference node model.jaffle_shop.finance_model, 
  which is not allowed because the referenced node is private to the finance group.
```

Report incorrect code

## Related docs

* [Model Access](../../docs/mesh/govern/model-access.md#groups)
* [Defining groups](../../docs/build/groups.md)
