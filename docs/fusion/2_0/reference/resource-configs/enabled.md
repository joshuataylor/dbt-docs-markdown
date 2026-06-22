# enabled

* Models
* Seeds
* Snapshots
* Tests
* Unit tests
* Sources
* Analyses
* Metrics
* Exposures
* Semantic models
* Saved queries

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

dbt\_project.yml

```yml
seeds:
  <resource-path>:
    +enabled: true | false
```

dbt\_project.yml

```yml
snapshots:
  <resource-path>:
    +enabled: true | false
```

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

💡Did you know\...

Available from dbt v

<!-- -->

1.8

<!-- -->

or with the

<!-- -->

[dbt "Latest" release track](https://docs.getdbt.com/docs/dbt-versions/dbt-release-tracks.md).

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

analyses/\<filename>.yml

```yaml
analyses:
  - name: <analysis_name>
    config:
      enabled: true | false
```

To configure analyses at the project level, set the [`require_corrected_analysis_fqns`](https://docs.getdbt.com/reference/global-configs/behavior-flag-introduction.md#project-level-configuration-for-analyses) flag to `true` in your `dbt_project.yml`.

dbt\_project.yml

```yaml
flags:
  require_corrected_analysis_fqns: true

analyses:
  +enabled: true | false
```

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

dbt\_project.yml

```yaml
semantic-models:
  <resource-path>:
    +enabled: true | false
```

models/file\_name.yml

```yaml
models:
  - name: model_name
    semantic_model:
      enabled: true | false # Required under 'semantic_model'
```

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

## Definition[​](#definition "Direct link to Definition")

An optional configuration for enabling or disabling a resource.

* Default: true

When a resource is disabled, dbt will not consider it as part of your project. Note that this can cause compilation errors.

If you instead want to exclude a model from a particular run, consider using the `--exclude` parameter as part of the [model selection syntax](https://docs.getdbt.com/reference/node-selection/syntax.md)

If you are disabling models because they are no longer being used, but you want to version control their SQL, consider making them an [analysis](https://docs.getdbt.com/docs/build/analyses.md) instead.

## Examples[​](#examples "Direct link to Examples")

### Disable a model in a package in order to use your own version of the model.[​](#disable-a-model-in-a-package-in-order-to-use-your-own-version-of-the-model "Direct link to Disable a model in a package in order to use your own version of the model.")

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

### Disable Semantic Layer resources from a package[​](#disable-semantic-layer-resources-from-a-package "Direct link to Disable Semantic Layer resources from a package")

Some packages may define Semantic Layer resources (semantic models, metrics, saved queries) using an older specification that isn’t compatible with the dbt Fusion engine.

To use these packages with Fusion while keeping your own semantic layer definitions, disable the package’s semantic layer resources in the relevant YAML file.

dbt\_project.yml

```yml
# Disable the package's time spine model (if it conflicts with yours)
models:
  ad_reporting:
    semantic_models:
      metricflow_time_spine:
        +enabled: false

# Disable all semantic layer resources from the package
semantic-models:
  ad_reporting:
    +enabled: false

metrics:
  ad_reporting:
    +enabled: false

saved-queries:
  ad_reporting:
    +enabled: false
```

## Was this page helpful?

YesNo

[Privacy policy](https://www.getdbt.com/cloud/privacy-policy)[Create a GitHub issue](https://github.com/dbt-labs/docs.getdbt.com/issues)

This site is protected by reCAPTCHA and the Google [Privacy Policy](https://policies.google.com/privacy) and [Terms of Service](https://policies.google.com/terms) apply.
