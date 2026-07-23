# How do I set a datatype for a column in my seed?

dbt will infer the datatype for each column based on the data in your CSV.

You can also explicitly set a datatype using the `column_types` [configuration](https://docs.getdbt.com/reference/resource-configs/column_types.md) like so:

dbt\_project.yml

```yml
seeds:
  jaffle_shop: # you must include the project name
    warehouse_locations:
      +column_types:
        zipcode: varchar(5)
```
