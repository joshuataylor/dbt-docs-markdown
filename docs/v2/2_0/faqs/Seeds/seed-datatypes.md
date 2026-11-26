# How do I set a datatype for a column in my seed?

dbt will infer the datatype for each column based on the data in your CSV.

(Applies to dbt v2.0 and later)

Inference for dates and timestamps differs between v1 and v2. Refer to [Column type inference](../../docs/build/seeds.md#column-type-inference).

You can also explicitly set a datatype using the `column_types` [configuration](../../reference/resource-configs/column_types.md) like so:

dbt\_project.yml

```yml
seeds:
  jaffle_shop: # you must include the project name
    warehouse_locations:
      +column_types:
        zipcode: varchar(5)
```
