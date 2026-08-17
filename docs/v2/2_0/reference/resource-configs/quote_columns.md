# quote\_columns

## Definition

An optional seed configuration, used to determine whether column names in the seed file should be quoted when the table is created.

* When `True`, dbt will quote the column names defined in the seed file when building a table for the seed, preserving casing.
* When `False`, dbt will not quote the column names defined in the seed file.
* When not set, it will vary by adapter whether or not column names are quoted.

## Usage

### Globally quote all seed columns

dbt\_project.yml

```yml
seeds:
  +quote_columns: true
```

### Only quote seeds in the `seeds/mappings` directory.

For a project with:

* `name: jaffle_shop` in the `dbt_project.yml` file
* `seed-paths: ["seeds"]` in the `dbt_project.yml` file

dbt\_project.yml

```yml
seeds:
  jaffle_shop:
    mappings:
      +quote_columns: true
```

Or:

seeds/properties.yml

```yml

seeds:
  - name: mappings
    config:
      quote_columns: true
```

## Recommended configuration

* Explicitly set this value if using seed files.
* Apply the configuration globally rather than to individual projects/seeds.
* Set `quote_columns: false` *unless* your column names include a special character or casing needs to be preserved. In that case, consider renaming your seed columns (this will simplify code downstream)
