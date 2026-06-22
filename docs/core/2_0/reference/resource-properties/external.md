# external

models/\<filename>.yml

```yml

sources:
  - name: <source_name>
    tables:
      - name: <table_name>
        external:
          location: <string>
          file_format: <string>
          row_format: <string>
          tbl_properties: <string>      
          partitions:
            - name: <column_name>
              data_type: <string>
              description: <string>
              config:
                meta: {dictionary} # changed to config in v1.10
            - ...
          <additional_property>: <additional_value>
```

## Definition[​](#definition "Direct link to Definition")

An extensible dictionary of metadata properties specific to sources that point to external tables. There are optional built-in properties, with simple type validation, that roughly correspond to the Hive external table spec. You may define and use as many additional properties as you'd like.

You may wish to define the `external` property in order to:

* Power macros that introspect [`graph.sources`](https://docs.getdbt.com/reference/dbt-jinja-functions/graph.md)
* Define metadata that you can later extract from the [manifest](https://docs.getdbt.com/reference/artifacts/manifest-json.md)

For an example of how this property can be used to power custom workflows, see the [`dbt-external-tables`](https://github.com/dbt-labs/dbt-external-tables) package.

## Was this page helpful?

YesNo

[Privacy policy](https://www.getdbt.com/cloud/privacy-policy)[Create a GitHub issue](https://github.com/dbt-labs/docs.getdbt.com/issues)

This site is protected by reCAPTCHA and the Google [Privacy Policy](https://policies.google.com/privacy) and [Terms of Service](https://policies.google.com/terms) apply.
