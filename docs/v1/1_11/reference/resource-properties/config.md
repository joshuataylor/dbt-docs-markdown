# About config property

## Models

models/\<filename>.yml

```yml

models:
  - name: <model_name>
    config:
      <model_config>: <config_value>
      ...
```

## Seeds

seeds/\<filename>.yml

```yml

seeds:
  - name: <seed_name>
    config:
      <seed_config>: <config_value>
      ...
```

## Snapshots

snapshots/\<filename>.yml

```yml

snapshots:
  - name: <snapshot_name>
    config:
      <snapshot_config>: <config_value>
      ...
```

## Tests

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

## Unit tests

Did you know\...

Available from dbt v1.8 or with the [dbt "Latest" release track](../../docs/dbt-versions/dbt-release-tracks.md).

models/\<filename>.yml

```yml
unit_tests:
  - name: <test-name>
    config:
      enabled: true | false
      meta: {dictionary}
      tags: <string>
```

## Sources

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

## Metrics

models/\<filename>.yml

```yml

metrics:
  - name: <metric_name>
    config:
      enabled: true | false
      group: <string>
      meta: {dictionary}
```

## Exposures

models/\<filename>.yml

```yml

exposures:
  - name: <exposure_name>
    config:
      enabled: true | false
      meta: {dictionary}
```

## Semantic models

(Applies to dbt v1.11 and earlier)

models/\<filename>.yml

```yml

semantic_models:
  - name: <semantic_model_name>
    config:
      enabled: true | false
      group: <string>
      meta: {dictionary}
```

## Saved queries

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

## Definition

The `config` property allows you to configure resources at the same time you're defining properties in YAML files.
