# About config property

* Models
* Seeds
* Snapshots
* Tests
* Unit tests
* Sources
* Metrics
* Exposures
* Semantic models
* Saved queries

models/\<filename>.yml

```yml

models:
  - name: <model_name>
    config:
      <model_config>: <config_value>
      ...
```

seeds/\<filename>.yml

```yml

seeds:
  - name: <seed_name>
    config:
      <seed_config>: <config_value>
      ...
```

snapshots/\<filename>.yml

```yml

snapshots:
  - name: <snapshot_name>
    config:
      <snapshot_config>: <config_value>
      ...
```

\<resource\_path>/\<filename>.yml

```yml

<resource_type>:
  - name: <resource_name>
    data_tests:
      - <test_name>:
          arguments: # available in v1.10.5 and higher. Older versions can set the <argument_name> as the top-level property.
            <argument_name>: <argument_value>
          config:
            <test_config>: <config-value>
            ...

    columns:
      - name: <column_name>
        data_tests:
          - <test_name>
          - <test_name>:
              arguments: # available in v1.10.5 and higher. Older versions can set the <argument_name> as the top-level property.
                <argument_name>: <argument_value>
              config:
                <test_config>: <config-value>
                ...
```

💡Did you know\...

Available from dbt v

<!-- -->

1.8

<!-- -->

or with the

<!-- -->

[dbt "Latest" release track](https://docs.getdbt.com/docs/dbt-versions/cloud-release-tracks.md).

models/\<filename>.yml

```yml
unit_tests:
  - name: <test-name>
    config:
      enabled: true | false
      meta: {dictionary}
      tags: <string>
```

models/\<filename>.yml

```yml

sources:
  - name: <source_name>
    config:
      <source_config>: <config_value>
    tables:
      - name: <table_name>
        config:
          <source_config>: <config_value>
```

models/\<filename>.yml

```yml

metrics:
  - name: <metric_name>
    config:
      enabled: true | false
      group: <string>
      meta: {dictionary}
```

models/\<filename>.yml

```yml

exposures:
  - name: <exposure_name>
    config:
      enabled: true | false
      meta: {dictionary}
```

models/\<filename>.yml

```yml

saved-queries:
  - name: <saved_query_name>
    config:
      cache: 
        enabled: true | false
      enabled: true | false
      group: <string>
      meta: {dictionary}
      schema: <string>
    exports:
      - name: <export_name>
        config:
          export_as: view | table 
          alias: <string>
          schema: <string>
```

## Definition[​](#definition "Direct link to Definition")

The `config` property allows you to configure resources at the same time you're defining properties in YAML files.

## Was this page helpful?

YesNo

[Privacy policy](https://www.getdbt.com/cloud/privacy-policy)[Create a GitHub issue](https://github.com/dbt-labs/docs.getdbt.com/issues)

This site is protected by reCAPTCHA and the Google [Privacy Policy](https://policies.google.com/privacy) and [Terms of Service](https://policies.google.com/terms) apply.
