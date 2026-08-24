# enabled

## Models

dbt\_project.yml

```yml
models:
  <resource-path>:
    +enabled: true | false
```

models/\<modelname>.sql

```sql

{{ config(
  enabled=true | false
) }}

select ...
```

## Seeds

dbt\_project.yml

```yml
seeds:
  <resource-path>:
    +enabled: true | false
```

## Snapshots

dbt\_project.yml

```yml
snapshots:
  <resource-path>:
    +enabled: true | false
```

(Applies to dbt v1.9 and later)

snapshots/snapshot\_name.yml

```yaml

snapshots:
  - name: snapshot_name
    config:
      enabled: true | false
```

snapshots/\<filename>.sql

```sql
# Configuring in a SQL file is a legacy method and not recommended. Use the property file instead.

{% snapshot snapshot_name %}

{{ config(
  enabled=true | false
) }}

select ...

{% endsnapshot %}
```

## Tests

dbt\_project.yml

```yml
data_tests:
  <resource-path>:
    +enabled: true | false
```

tests/\<filename>.sql

```sql
{% test <testname>() %}

{{ config(
  enabled=true | false
) }}

select ...

{% endtest %}
```

tests/\<filename>.sql

```sql
{{ config(
  enabled=true | false
) }}
```

## Unit tests

Did you know\...

Available from dbt v1.8 or with the [dbt "Latest" release track](../../docs/dbt-versions/dbt-release-tracks.md).

dbt\_project.yml

```yml
unit_tests:
  <resource-path>:
    +enabled: true | false
```

models/\<filename>.yml

```yaml
unit_tests:
  - name: [<test-name>]
    config:
      enabled: true | false
```

## Sources

dbt\_project.yml

```yaml
sources:
  <resource-path>:
    +enabled: true | false
```

models/properties.yml

```yaml

sources:
  - name: [<source-name>]
    config:
      enabled: true | false
    tables:
      - name: [<source-table-name>]
        config:
          enabled: true | false
```

## Analyses

analyses/\<filename>.yml

```yaml
analyses:
  - name: <analysis_name>
    config:
      enabled: true | false
```

To configure analyses at the project level, set the [`require_corrected_analysis_fqns`](../global-configs/behavior-flags/require_corrected_analysis_fqns.md) flag to `true` in your `dbt_project.yml`.

dbt\_project.yml

```yaml
flags:
  require_corrected_analysis_fqns: true

analyses:
  +enabled: true | false
```

## Metrics

dbt\_project.yml

```yaml
metrics:
  <resource-path>:
    +enabled: true | false
```

models/metrics.yml

```yaml

metrics:
  - name: [<metric-name>]
    config:
      enabled: true | false
```

## Exposures

dbt\_project.yml

```yaml
exposures:
  <resource-path>:
    +enabled: true | false
```

models/exposures.yml

```yaml

exposures:
  - name: [<exposure-name>]
    config:
      enabled: true | false
```

## Semantic models

dbt\_project.yml

```yaml
semantic-models:
  <resource-path>:
    +enabled: true | false
```

(Applies to dbt v1.12 and later)

models/file\_name.yml

```yaml
models:
  - name: model_name
    semantic_model:
      enabled: true | false # Required under 'semantic_model'
```

## Saved queries

dbt\_project.yml

```yaml
saved-queries:
  <resource-path>:
    +enabled: true | false
```

models/semantic\_models.yml

```yaml
saved_queries:
  - name: [<saved_query_name>]
    config:
      enabled: true | false
```

## Definition

An optional configuration for enabling or disabling a resource.

* Default: true

When a resource is disabled, dbt will not consider it as part of your project. Note that this can cause compilation errors.

If you instead want to exclude a model from a particular run, consider using the `--exclude` parameter as part of the [model selection syntax](../node-selection/syntax.md)

If you are disabling models because they are no longer being used, but you want to version control their SQL, consider making them an [analysis](../../docs/build/analyses.md) instead.

## Examples

### Disable a model in a package in order to use your own version of the model.

This could be useful if you want to change the logic of a model in a package. For example, if you need to change the logic in the `segment_web_page_views` from the `segment` package ([original model](https://github.com/dbt-labs/segment/blob/a8ff2f892b009a69ec36c3061a87e437f0b0ea93/models/base/segment_web_page_views.sql)):

1. Add a model named `segment_web_page_views` (the same name) to your own project.
2. To avoid a compilation error due to duplicate models, disable the segment package's version of the model like so:

dbt\_project.yml

```yml
models:
  segment:
    base:
      segment_web_page_views:
        +enabled: false
```
