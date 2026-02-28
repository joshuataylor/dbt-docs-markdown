# delimiter

💡Did you know\...

Available from dbt v

<!-- -->

1.7

<!-- -->

or with the

<!-- -->

[dbt "Latest" release track](https://docs.getdbt.com/docs/dbt-versions/cloud-release-tracks.md).

## Definition[​](#definition "Direct link to Definition")

You can use this optional seed configuration to customize how you separate values in a [seed](https://docs.getdbt.com/docs/build/seeds.md) by providing the one-character string.

* The delimiter defaults to a comma when not specified.
* Explicitly set the `delimiter` configuration value if you want seed files to use a different delimiter, such as "|" or ";".

## Usage[​](#usage "Direct link to Usage")

Specify a delimiter in your `dbt_project.yml` file to customize the global separator for all seed values:

dbt\_project.yml

```yml
seeds:
  <project_name>:
     +delimiter: "|" # default project delimiter for seeds will be "|"
    <seed_subdirectory>:
      +delimiter: "," # delimiter for seeds in seed_subdirectory will be ","
```

Or use a custom delimiter to override the values for a specific seed:

seeds/properties.yml

```yml

seeds:
  - name: <seed_name>
    config: 
      delimiter: "|"
```

## Examples[​](#examples "Direct link to Examples")

For a project with:

* `name: jaffle_shop` in the `dbt_project.yml` file
* `seed-paths: ["seeds"]` in the `dbt_project.yml` file

### Use a custom delimiter to override global values[​](#use-a-custom-delimiter-to-override-global-values "Direct link to Use a custom delimiter to override global values")

You can set a default behavior for all seeds with an exception for one seed, `seed_a`, which uses a comma:

dbt\_project.yml

```yml
seeds:
  jaffle_shop: 
    +delimiter: "|" # default delimiter for seeds in jaffle_shop project will be "|"
    seed_a:
      +delimiter: "," # delimiter for seed_a will be ","
```

Your corresponding seed files would be formatted like this:

seeds/my\_seed.csv

```text
col_a|col_b|col_c
1|2|3
4|5|6
...
```

seeds/seed\_a.csv

```text
name,id
luna,1
doug,2
...
```

Or you can configure custom behavior for one seed. The `country_codes` uses the ";" delimiter:

seeds/properties.yml

```yml

seeds:
  - name: country_codes
    config:
      delimiter: ";"
```

The `country_codes` seed file would be formatted like this:

seeds/country\_codes.csv

```text
country_code;country_name
US;United States
CA;Canada
GB;United Kingdom
...
```

## Was this page helpful?

YesNo

[Privacy policy](https://www.getdbt.com/cloud/privacy-policy)[Create a GitHub issue](https://github.com/dbt-labs/docs.getdbt.com/issues)

This site is protected by reCAPTCHA and the Google [Privacy Policy](https://policies.google.com/privacy) and [Terms of Service](https://policies.google.com/terms) apply.
