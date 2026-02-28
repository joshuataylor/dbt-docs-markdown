# access

models/\<schema>.yml

```yml

models:
  - name: model_name
    config:
      access: private | protected | public # changed to config in v1.10
```

You can apply `access` modifiers in config files, including the `dbt_project.yml`, or to models one-by-one in `properties.yml`. Applying `access` configs to a subfolder modifies the default for all models in that subfolder, so make sure you intend for this behavior. When setting individual model access, a group or subfolder might contain a variety of access levels, so when you designate a model with `access: public` make sure you intend for this behavior.

Note that for backwards compatibility, `access` is supported as a top-level key, but without the capabilities of config inheritance.

There are multiple approaches to configuring access:

* In `properties.yml` using the older method:

  models/properties\_my\_public\_model.yml

  ```yml

  models:
    - name: my_public_model
      config:
        access: public # Older method, still supported
          # changed to config in v1.10
      
  ```

* In `properties.yml` using the new method (for v1.7 or higher). Use either the older method or the new method, but not both for the same model:

  models/properties\_my\_public\_model.yml

  ```yml

  models:
    - name: my_public_model
      config:
        access: public
      
  ```

* In `dbt_project.yml`:

  dbt\_project.yml

  ```yml
  models:
    my_project_name:
      subfolder_name:
        +group: my_group
        +access: private  # sets default for all models in this subfolder
  ```

* In the `my_public_model.sql` file:

  models/my\_public\_model.sql

  ```sql
  -- models/my_public_model.sql

  {{ config(access = "public") }}

  select ...
  ```

After you define `access`, rerun a production job to apply the change.

## Definition[​](#definition "Direct link to Definition")

The access level of the model you are declaring properties for.

Some models (not all) are designed to be referenced through the [ref](https://docs.getdbt.com/reference/dbt-jinja-functions/ref.md) function across [groups](https://docs.getdbt.com/docs/build/groups.md).

| Access    | Referenceable by                                                                          |
| --------- | ----------------------------------------------------------------------------------------- |
| private   | Same group                                                                                |
| protected | Same project/package                                                                      |
| public    | Any group, package, or project. When defined, rerun a production job to apply the change. |

Search table...

|                  |   |   |   |   |
| ---------------- | - | - | - | - |
| Loading table... |   |   |   |   |

If you try to reference a model outside of its supported access, you will see an error:

```shell
dbt run -s marketing_model
...
dbt.exceptions.DbtReferenceError: Parsing Error
  Node model.jaffle_shop.marketing_model attempted to reference node model.jaffle_shop.finance_model, 
  which is not allowed because the referenced node is private to the finance group.
```

## Default[​](#default "Direct link to Default")

By default, all models are "protected." This means that other models in the same project can reference them.

## Related docs[​](#related-docs "Direct link to Related docs")

* [Model Access](https://docs.getdbt.com/docs/mesh/govern/model-access.md#groups)
* [Group configuration](https://docs.getdbt.com/reference/resource-configs/group.md)

## Was this page helpful?

YesNo

[Privacy policy](https://www.getdbt.com/cloud/privacy-policy)[Create a GitHub issue](https://github.com/dbt-labs/docs.getdbt.com/issues)

This site is protected by reCAPTCHA and the Google [Privacy Policy](https://policies.google.com/privacy) and [Terms of Service](https://policies.google.com/terms) apply.
