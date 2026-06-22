# About the doc function

You can specify documentation text in a docs block, then use the `doc()` Jinja function in description fields as a way to reuse the same text in multiple places. You can only use the `doc()` Jinja function in properties YAML files for resources with description properties. For example, models, model columns, sources, source tables, source columns, and so on.

The `doc()` Jinja function, which is analogous to `ref()`, looks up the named docs block (for example, `{% docs orders %} ... {% enddocs %}` in a `docs.md` file) and returns its rendered content. For more information, refer to the [Documentation guide](https://docs.getdbt.com/docs/explore/build-and-view-your-docs.md).

## Usage[​](#usage "Direct link to Usage")

In dbt, column descriptions can be defined directly in a model's properties YAML file. These descriptions are written as plain text and are associated with a specific column.

### Manually adding descriptions[​](#manually-adding-descriptions "Direct link to Manually adding descriptions")

models/orders.yml

```yaml
models:
  - name: orders
    columns:
      - name: order_total_cents
        description: "Total order amount in cents. This value is always a positive integer and excludes taxes and shipping."
```

When you run `dbt docs generate` (dbt Core v1 only) and view the docs site, this text appears exactly as written but only for the `order_total_cents` column of the `orders` model.

### Reusing descriptions with doc()[​](#reusing-descriptions-with-doc "Direct link to Reusing descriptions with doc()")

To avoid repeating the same description across multiple models or columns, dbt lets you define documentation separately and reference it using the `doc()` function.

With this approach, documentation is written once in a markdown file using a named docs block, and then reused wherever needed in your project. This helps ensure consistency and makes it easier to maintain shared definitions over time.

First, define a reusable documentation block in a `docs.md`:

models/docs/example.md

```text
{% docs order_total_cents %}
Total order amount in cents. This value is always a positive integer and excludes taxes and shipping.
{% enddocs %}
```

This defines a docs block named `order_total_cents`. Reference it with `doc('order_total_cents')` in description fields wherever you need the same text.

Next, reference this documentation in your properties YAML file:

models/orders.yml

```yaml
models:
  - name: orders
    columns:
      - name: order_total_cents
        description: "{{ doc('order_total_cents') }}"
```

When you run `dbt docs generate` (dbt Core v1 only), dbt resolves the `doc()` reference by looking up the corresponding docs block and injecting its content into the generated documentation.

As a result, the column description displays the text defined in the markdown file, rather than inline YAML.

## Avoid duplicate names[​](#avoid-duplicate-names "Direct link to Avoid duplicate names")

Docs block names must be unique within your project. If you define multiple `{% docs %}` blocks with the same name, dbt cannot reliably determine which block to use when `doc('DOCS_BLOCK_NAME')` is called.

In dbt Core v1.11 and later, duplicate docs block names are not allowed. If duplicates are found, dbt reports the conflicting files and raises a compilation error. Rename one block so each docs block name is unique.

models/docs/example.md

```text
{% docs order_total_cents %}
Total order amount in cents. This value is always a positive integer and excludes taxes and shipping.
{% enddocs %}
 
{% docs order_total_cents %}
Total order amount in cents, excluding taxes and shipping fees.
{% enddocs %}
```

```shell
dbt parse
09:55:55  Running with dbt=1.11.8
09:55:55  Registered adapter: duckdb=1.10.1
09:55:56  Encountered an error:
Compilation Error
  dbt found two docs with the name "order_total_cents".
  
  Since these resources have the same name, dbt will be unable to find the correct resource
  when looking for doc("order_total_cents").
  
  To fix this, change the name of one of these resources:
  - doc.orders.order_total_cents (models/docs/example.md)
  - doc.orders.order_total_cents (models/docs/example.md)
```

## Was this page helpful?

YesNo

[Privacy policy](https://www.getdbt.com/cloud/privacy-policy)[Create a GitHub issue](https://github.com/dbt-labs/docs.getdbt.com/issues)

This site is protected by reCAPTCHA and the Google [Privacy Policy](https://policies.google.com/privacy) and [Terms of Service](https://policies.google.com/terms) apply.
