# columns

* Models
* Sources
* Seeds
* Snapshots
* Analyses

models/\<filename>.yml

```yml

models:
  - name: <model_name>
    columns:
      - name: <column_name>
        data_type: <string>
        description: <markdown_string>
        quote: true | false
        data_tests: ...
        config:
          tags: ...
          meta: ...
      - name: <another_column>
        ...
```

models/\<filename>.yml

```yml

sources:
  - name: <source_name>
    tables:
    - name: <table_name>
      columns:
        - name: <column_name>
          description: <markdown_string>
          data_type: <string>
          quote: true | false
          data_tests: ...
          config:
            tags: ...
            meta: ...
        - name: <another_column>
          ...
```

seeds/\<filename>.yml

```yml

seeds:
  - name: <seed_name>
    columns:
      - name: <column_name>
        description: <markdown_string>
        data_type: <string>
        quote: true | false
        data_tests: ...
        config:
          tags: ...
          meta: ...
      - name: <another_column>
            ...
```

snapshots/\<filename>.yml

```yml

snapshots:
  - name: <snapshot_name>
    columns:
      - name: <column_name>
        description: <markdown_string>
        data_type: <string>
        quote: true | false
        data_tests: ...
        config:
          tags: ...
          meta: ...
      - name: <another_column>
```

analyses/\<filename>.yml

```yml

analyses:
  - name: <analysis_name>
    columns:
      - name: <column_name>
        description: <markdown_string>
        data_type: <string>
      - name: <another_column>
```

Columns are not resources in and of themselves. Instead, they are child properties of another resource type. They can define sub-properties that are similar to properties defined at the resource level:

* `tags`
* `meta`
* `data_tests`
* `description`

Because columns are not resources, their `tags` and `meta` properties are not true configurations even when nested under a `config` block. They do not inherit the `tags` or `meta` values of their parent resources. However, you can select a generic test, defined on a column, using tags applied to its column or top-level resource; see [test selection examples](https://docs.getdbt.com/reference/node-selection/test-selection-examples.md#run-tests-on-tagged-columns).

Columns may optionally define a `data_type`, which is necessary for:

* Enforcing a model [contract](https://docs.getdbt.com/reference/resource-configs/contract.md)
* Use in other packages or plugins, such as the [`external`](https://docs.getdbt.com/reference/resource-properties/external.md) property of sources and [`dbt-external-tables`](https://hub.getdbt.com/dbt-labs/dbt_external_tables/latest/)

### `quote`[​](#quote "Direct link to quote")

The `quote` field can be used to enable or disable quoting for column names.

* Models
* Sources
* Seeds
* Snapshots
* Analyses

models/schema.yml

```yml

models:
  - name: model_name
    columns:
      - name: column_name
        quote: true | false
```

models/schema.yml

```yml

sources:
  - name: source_name
    tables:
      - name: table_name
        columns:
          - name: column_name
            quote: true | false
```

seeds/schema.yml

```yml

seeds:
  - name: seed_name
    columns:
      - name: column_name
        quote: true | false
```

snapshots/schema.yml

```yml

snapshots:
  - name: snapshot_name
    columns:
      - name: column_name
        quote: true | false
```

analysis/schema.yml

```yml

analyses:
  - name: analysis_name
    columns:
      - name: column_name
        quote: true | false
```

### Default[​](#default "Direct link to Default")

The default quoting value is `false`

### Explanation[​](#explanation "Direct link to Explanation")

This is particularly relevant to those using Snowflake, where quoting can be particularly fickle.

This property is useful when:

* A source table has a column that needs to be quoted to be selected, for example, to preserve column casing
* A seed was created with `quote_columns: true` ([docs](https://docs.getdbt.com/reference/resource-configs/quote_columns.md)) on Snowflake
* A model uses quotes in the SQL, potentially to work around the use of reserved words

```sql
select user_group as "group"
```

Without setting `quote: true`:

* [Data tests](https://docs.getdbt.com/docs/build/data-tests.md) applied to this column may fail due to invalid SQL
* Documentation may not render correctly, e.g. `group` and `"group"` may not be matched as the same column name.

### Example[​](#example "Direct link to Example")

#### Add data tests to a quoted column in a source table[​](#add-data-tests-to-a-quoted-column-in-a-source-table "Direct link to Add data tests to a quoted column in a source table")

This is especially relevant if using Snowflake:

```yml

sources:
  - name: stripe
    tables:
      - name: payment
        columns:
          - name: orderID
            quote: true
            data_tests:
              - not_null
```

Without `quote: true`, the following error will occur:

```text
$ dbt test -s source:stripe.*
Running with dbt=0.16.1
Found 7 models, 22 tests, 0 snapshots, 0 analyses, 130 macros, 0 operations, 0 seed files, 4 sources

13:33:37 | Concurrency: 4 threads (target='learn')
13:33:37 |
13:33:37 | 1 of 1 START test source_not_null_stripe_payment_order_id............ [RUN]
13:33:39 | 1 of 1 ERROR source_not_null_stripe_payment_order_id................. [ERROR in 1.89s]
13:33:39 |
13:33:39 | Finished running 1 tests in 6.43s.

Completed with 1 error and 0 warnings:

Database Error in test source_not_null_stripe_payment_order_id (models/staging/stripe/src_stripe.yml)
  000904 (42000): SQL compilation error: error line 3 at position 6
  invalid identifier 'ORDERID'
  compiled SQL at target/compiled/jaffle_shop/schema_test/source_not_null_stripe_payment_orderID.sql
```

This is because dbt is trying to run:

```sql
select count(*)
from raw.stripe.payment
where orderID is null
```

Instead of:

```sql
select count(*)
from raw.stripe.payment
where "orderID" is null
```

## Was this page helpful?

YesNo

[Privacy policy](https://www.getdbt.com/cloud/privacy-policy)[Create a GitHub issue](https://github.com/dbt-labs/docs.getdbt.com/issues)

This site is protected by reCAPTCHA and the Google [Privacy Policy](https://policies.google.com/privacy) and [Terms of Service](https://policies.google.com/terms) apply.
