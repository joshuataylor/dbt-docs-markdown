# model\_name

models/\<schema>.yml

```yml

models:
  - name: model_name
```

## Definition

The name of the model you are declaring properties for. Must match the *filename* of a model — including case sensitivity. Any mismatched casing can prevent dbt from applying configurations correctly and may affect metadata in Catalog.

## Default

This is a **required property**, no default exists.
