# Add groups to your DAG

A group is a collection of nodes within a dbt DAG. Groups are named, and every group has an `owner`. They enable intentional collaboration within and across teams by restricting [access to private](https://docs.getdbt.com/reference/resource-configs/access.md) models.

Group members may include models, tests, seeds, snapshots, analyses, and metrics. (Not included: sources and exposures.) Each node may belong to only one group.

### Declaring a group[​](#declaring-a-group "Direct link to Declaring a group")

<!-- -->

Groups are defined in `.yml` files, nested under a `groups:` key. In version 1.10 and higher, you can add a `description` and a `meta` config to add more information about the group.

<!-- -->

models/marts/finance/finance.yml

```yaml
groups:
  - name: finance
    description: "All finance-related models owned by the Finance team." # optional
    owner:
      # 'name' or 'email' is required; additional properties will no longer be allowed in a future release
      email: finance@jaffleshop.com
    config:
      meta: # optional
        data_owner: Finance team
        cost_center: finance
        data_classification: sensitive
```

#### Centrally defining a group[​](#centrally-defining-a-group "Direct link to Centrally defining a group")

To centrally define a group in your project, there are two options:

* Create one `_groups.yml` file in the root of the `models` directory.

* Create one `_groups.yml` file in the root of a `groups` directory. For this option, you also need to configure [`model-paths`](https://docs.getdbt.com/reference/project-configs/model-paths.md) in the `dbt_project.yml` file:

  ```yml
  model-paths: ["models", "groups"]
  ```

### Group properties[​](#group-properties "Direct link to Group properties")

The following properties are available when defining a group:

| Property      | Required | Description                                                                                                                                                                                        |
| ------------- | -------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `name`        | Required | A unique name for the group within the project. Used to assign resources via the [`group` config](https://docs.getdbt.com/reference/resource-configs/group.md).                                    |
| `owner`       | Required | Identifies who is responsible for the group. Must include either `name:` or `email:`.                                                                                                              |
| `description` | Optional | A human readable description of the group's purpose. Supports markdown and the [`doc` Jinja function](https://docs.getdbt.com/reference/dbt-jinja-functions/doc.md). Supported in v1.10 and later. |
| `config.meta` | Optional | A dictionary of arbitrary key/value metadata about the group. Useful for storing information such as cost centers, data classifications, or team contact details.                                  |

Search table...

|                  |   |   |   |   |
| ---------------- | - | - | - | - |
| Loading table... |   |   |   |   |

models/marts/finance/finance.yml

```yaml
groups:
  - name: finance
    description: "All models owned by the Finance team."
    owner:
      email: finance@jaffleshop.com
    config:
      meta:
        data_owner: Finance team
        cost_center: finance
        data_classification: sensitive
```

### Adding a model to a group[​](#adding-a-model-to-a-group "Direct link to Adding a model to a group")

Use the `group` configuration to add one or more models to a group.

* Project-level
* Model-level
* In-file

dbt\_project.yml

```yml
models:
  marts:
    finance:
      +group: finance
```

models/schema.yml

```yml
models:
  - name: model_name
    config:
      group: finance
```

models/model\_name.sql

```sql
{{ config(group = 'finance') }}

select ...
```

### Referencing a model in a group[​](#referencing-a-model-in-a-group "Direct link to Referencing a model in a group")

By default, all models within a group have the `protected` [access modifier](https://docs.getdbt.com/reference/resource-configs/access.md). This means they can be referenced by downstream resources in *any* group in the same project, using the [`ref`](https://docs.getdbt.com/reference/dbt-jinja-functions/ref.md) function. If a grouped model's `access` property is set to `private`, only resources within its group can reference it.

models/schema.yml

```yml
models:
  - name: finance_private_model
    config:
      access: private # changed to config in v1.10
      group: finance

  # in a different group!
  - name: marketing_model
    config:
      group: marketing
```

models/marketing\_model.sql

```sql
select * from {{ ref('finance_private_model') }}
```

```shell
$ dbt run -s marketing_model
...
dbt.exceptions.DbtReferenceError: Parsing Error
  Node model.jaffle_shop.marketing_model attempted to reference node model.jaffle_shop.finance_private_model, 
  which is not allowed because the referenced node is private to the finance group.
```

## Related docs[​](#related-docs "Direct link to Related docs")

* [Model Access](https://docs.getdbt.com/docs/mesh/govern/model-access.md#groups)
* [Group configuration](https://docs.getdbt.com/reference/resource-configs/group.md)
* [Group selection](https://docs.getdbt.com/reference/node-selection/methods.md#group)

## Was this page helpful?

YesNo

[Privacy policy](https://www.getdbt.com/cloud/privacy-policy)[Create a GitHub issue](https://github.com/dbt-labs/docs.getdbt.com/issues)

This site is protected by reCAPTCHA and the Google [Privacy Policy](https://policies.google.com/privacy) and [Terms of Service](https://policies.google.com/terms) apply.
