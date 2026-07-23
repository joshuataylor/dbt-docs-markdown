# What if my source is in a different database to my target database?

Use the [`database` property](https://docs.getdbt.com/reference/resource-properties/database.md) to define the database that the source is in.

models/\<filename>.yml

```yml
sources:
  - name: jaffle_shop
    database: raw
    schema: jaffle_shop
    tables:
      - name: orders
      - name: customers
```
