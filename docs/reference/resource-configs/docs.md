# docs

* Models
* Sources
* Seeds
* Snapshots
* Analyses
* Macros

You can configure `docs` behavior for many resources at once by setting in `dbt_project.yml`. You can also use the `docs` config in `properties.yaml` files, to set or override documentation behaviors for specific resources:

dbt\_project.yml

```yml
models:
  <resource-path>:
    +docs:
      show: true | false
      node_color: color_id # Use name (such as node_color: purple) or hex code with quotes (such as node_color: "#cd7f32")
```

models/schema.yml

```yml

models:
- name: model_name
  config:
    docs: # changed to config in v1.10
      show: true | false
      node_color: color_id # Use name (such as node_color: purple) or hex code with quotes (such as node_color: "#cd7f32")
```

This property is not implemented for sources.

You can use the docs property in YAML files, including the `dbt_project.yml`:

dbt\_project.yml

```yml
seeds:
  <resource-path>:
    +docs:
      show: true | false
      node_color: color_id # Use name (such as node_color: purple) or hex code with quotes (such as node_color: "#cd7f32")
```

seeds/schema.yml

```yml

seeds:
  - name: seed_name
    config:
      docs: # changed to config in v1.10
        show: true | false
        node_color: color_id # Use name (such as node_color: purple) or hex code with quotes (such as node_color: "#cd7f32")
```

You can use the docs property in YAML files, including the `dbt_project.yml`:

dbt\_project.yml

```yml
snapshots:
  <resource-path>:
    +docs:
      show: true | false
      node_color: color_id # Use name (such as node_color: purple) or hex code with quotes (such as node_color: "#cd7f32")
```

snapshots/schema.yml

```yml

snapshots:
  - name: snapshot_name
    config:
      docs: # changed to config in v1.10
        show: true | false
        node_color: color_id # Use name (such as node_color: purple) or hex code with quotes (such as node_color: "#cd7f32")
```

You can use the docs property in YAML files, *except* in `dbt_project.yml`. Refer to [Analysis properties](https://docs.getdbt.com/reference/analysis-properties.md) for more info.

analysis/schema.yml

```yml

analyses:
  - name: analysis_name
    config:
      docs: # changed to config in v1.10
        show: true | false
        node_color: color_id # Use name (such as node_color: purple) or hex code with quotes (such as node_color: "#cd7f32")
```

You can use the docs property in YAML files, *except* in `dbt_project.yml`. Refer to [Macro properties](https://docs.getdbt.com/reference/macro-properties.md) for more info.

macros/schema.yml

```yml

macros:
  - name: macro_name
    config:
      docs: # changed to config in v1.10
        show: true | false
```

Note that for backwards compatibility, `docs` is supported as a top-level key, but without the capabilities of config inheritance.

## Definition[​](#definition "Direct link to Definition")

The `docs` property can be used to provide documentation-specific configuration to models. It supports the attribute `show`, which controls whether or not nodes are shown in the auto-generated documentation website. It also supports `node_color` for models, seeds, snapshots, and analyses. Other node types are not supported.

**Note:** Hidden models will still appear in the dbt DAG visualization but will be identified as "hidden.”

## Default[​](#default "Direct link to Default")

The default value for `show` is `true`.

## Examples[​](#examples "Direct link to Examples")

### Mark a model as hidden[​](#mark-a-model-as-hidden "Direct link to Mark a model as hidden")

```yml
models:
  - name: sessions__tmp
    docs:
      show: false
```

### Mark a subfolder of models as hidden[​](#mark-a-subfolder-of-models-as-hidden "Direct link to Mark a subfolder of models as hidden")

**Note:** This can also hide dbt packages.

dbt\_project.yml

```yml
models:
  # hiding models within the staging subfolder
  tpch:
    staging:
      +materialized: view
      +docs:
        show: false
  
  # hiding a dbt package
  dbt_artifacts:
    +docs:
      show: false
```

## Custom node colors[​](#custom-node-colors "Direct link to Custom node colors")

The `docs` attribute supports `node_color` to customize the display color of some node types in the DAG within [dbt Docs](https://docs.getdbt.com/docs/build/view-documentation.md). You can define node colors in the following files and apply overrides where needed.

* `node_color` hierarchy:
  <!-- -->
  * `<example-sql-file.sql>` overrides `schema.yml` overrides `dbt_project.yml`

Note, you need to run or re-run the `dbt docs generate` command to apply and view the customized colors.

Custom node colors not applicable in Catalog

The custom `node_color` attribute isn't applicable in Catalog. Instead, Explorer provides [lenses](https://docs.getdbt.com/docs/explore/explore-projects.md#lenses), which are map layers for your DAG. Lenses help you better understand your project's contextual metadata at scale and distinguish specific models or subsets of models.

## Examples[​](#examples-1 "Direct link to Examples")

Add custom `node_colors` to models that support it within subdirectories based on hex codes or a plain color name.

![Example](/assets/images/node_color_example-80b597978b6a0f15b6db30ce0d3375ed.png)

`marts/core/fct_orders.sql` with `node_color: red` overrides `dbt_project.yml` with `node_color: gold`

`marts/core/schema.yml` with `node_color: #000000` overrides `dbt_project.yml` with `node_color: gold`

dbt\_project.yml

```yml
models:
  tpch:
    staging:
      +materialized: view
      +docs:
        node_color: "#cd7f32"

    marts:
      core:
        materialized: table
        +docs:
          node_color: "gold"
```

marts/core/schema.yml

```yml
models:
  - name: dim_customers
    description: Customer dimensions table
    docs:
      node_color: '#000000'
```

marts/core/fct\_orders.sql

```sql
{{
    config(
        materialized = 'view',
        tags=['finance'],
        docs={'node_color': 'red'}
    )
}}

with orders as (
    
    select * from {{ ref('stg_tpch_orders') }} 

),
order_item as (
    
    select * from {{ ref('order_items') }}

),
order_item_summary as (

    select 
        order_key,
        sum(gross_item_sales_amount) as gross_item_sales_amount,
        sum(item_discount_amount) as item_discount_amount,
        sum(item_tax_amount) as item_tax_amount,
        sum(net_item_sales_amount) as net_item_sales_amount
    from order_item
    group by
        1
),
final as (

    select 

        orders.order_key, 
        orders.order_date,
        orders.customer_key,
        orders.status_code,
        orders.priority_code,
        orders.clerk_name,
        orders.ship_priority,
                
        1 as order_count,                
        order_item_summary.gross_item_sales_amount,
        order_item_summary.item_discount_amount,
        order_item_summary.item_tax_amount,
        order_item_summary.net_item_sales_amount
    from
        orders
        inner join order_item_summary
            on orders.order_key = order_item_summary.order_key
)
select 
    *
from
    final

order by
    order_date
```

If a `node_color` is incompatible with dbt docs, you will see a compile error, as in the example below.

```shell
Invalid color name for docs.node_color: aweioohafio23f. It is neither a valid HTML color name nor a valid HEX code.
```

dbt\_project.yml

```yml
models:
  tpch:
    marts:
      core:
        materialized: table
        +docs:
          node_color: "aweioohafio23f"
```

## Was this page helpful?

YesNo

[Privacy policy](https://www.getdbt.com/cloud/privacy-policy)[Create a GitHub issue](https://github.com/dbt-labs/docs.getdbt.com/issues)

This site is protected by reCAPTCHA and the Google [Privacy Policy](https://policies.google.com/privacy) and [Terms of Service](https://policies.google.com/terms) apply.
