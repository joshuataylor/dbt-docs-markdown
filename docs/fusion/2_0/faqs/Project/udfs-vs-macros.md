# When should I use a UDF instead of a macro?

Both user-defined functions (UDFs) and macros let you reuse logic across your dbt project, but they work in fundamentally different ways. Here's when to use each:

#### Use UDFs when:[​](#use-udfs-when "Direct link to Use UDFs when:")

 You need logic accessible outside dbt

UDFs are created in your warehouse and can be used by BI tools, data science notebooks, SQL clients, or any other tool that connects to your warehouse. Macros only work within dbt.

 You want to standardize warehouse-native functions

UDFs let you create reusable warehouse functions for data validation, custom formatting, or business-specific calculations that need to be consistent across all your data tools. Once created, they become part of your warehouse's function catalog.

 You want dbt to manage the function lifecycle

dbt manages UDFs as part of your DAG execution, ensuring they're created before models that reference them. You can version control UDF definitions alongside your models, test changes in development environments, and deploy them together through CI/CD pipelines.

 Jinja compiles at creation time, not on each function call

You can use Jinja (loops, conditionals, macros, `ref`, `source`, `var`) inside a UDF configuration. dbt resolves that Jinja **when the UDF is created**, and the resulting SQL body is what gets stored in your warehouse.

Jinja influences the function when it’s created, whereas arguments influence it when it runs in the warehouse:

* ✅
  <!-- -->
  **Allowed:** Jinja that depends on project or build-time state — for example, `var(“can_do_things”)`, static `ref(‘orders’)`, or environment-specific logic. These are all evaluated once at creation time.
* ❌
  <!-- -->
  **Not allowed:** Jinja that depends on **function arguments** passed at runtime. The compiler can’t see those, so dynamic `ref(ref_name)` or conditional Jinja based on argument values won’t work.

 You need Python logic that runs in your warehouse

A Python UDF creates a Python function directly within your data warehouse, which you can invoke using SQL.<br /><!-- -->This makes it easier to apply complex transformations, calculations, or logic that would be difficult or verbose to express in SQL.

Python UDFs support conditionals and looping within the function logic itself (using Python syntax), and execute at runtime, not at compile time like macros. Python UDFs are currently supported in Snowflake and BigQuery.

#### Use macros when:[​](#use-macros-when "Direct link to Use macros when:")

 You need to generate SQL at compile time

Macros generate SQL dynamically **before** it's sent to the warehouse (at compile time). This is essential for:

* Building different SQL for different warehouses
* Generating repetitive SQL patterns (like creating dozens of similar columns)
* Creating entire model definitions or DDL statements
* Dynamically referencing models based on project structure

UDFs execute **at query runtime** in the warehouse. While they can use Jinja templating in their definitions, they don't generate new SQL queries—they're pre-defined functions that get called by your SQL.

Expanding UDFs

Currently, SQL and Python UDFs are supported. Java and Scala UDFs are planned for future releases.

 You want to generate DDL or DML statements

Currently, SQL and Python UDFs are supported. Java and Scala UDFs are planned for future releases.

 You need to adapt SQL across different warehouses

Macros can use Jinja conditional logic to generate warehouse-specific SQL (see [cross-database macros](https://docs.getdbt.com/reference/dbt-jinja-functions/cross-database-macros.md)), making your dbt project portable across platforms.

UDFs are warehouse-specific objects. Even though UDFs can include Jinja templating in their definitions, each warehouse has different syntax for creating functions, different supported data types, and different SQL dialects. You would need to define separate UDF files for each warehouse you support.

 Your logic needs access to dbt context

Both macros and UDFs can use Jinja, which means they can access dbt context variables like `{{ ref() }},` `{{ source() }}`, environment variables, and project configurations. You can even call a macro from within a UDF (and vice versa) to combine dynamic SQL generation with runtime execution.

However, the difference between the two is *when* the logic runs:

* Macros run at compile time, generating SQL before it’s sent to the warehouse.
* UDFs run inside the warehouse at query time.

 You want to avoid creating warehouse objects

Macros don't create anything in your warehouse; they just generate SQL at compile time. UDFs create actual function objects in your warehouse that need to be managed.

#### Can I use both together?[​](#can-i-use-both-together "Direct link to Can I use both together?")

Yes! You can use a macro to call a UDF or call a macro from within a UDF, combining the benefits of both. So the following example shows how to use a macro to define default values for arguments alongside your logic, for your UDF

```sql
{% macro cents_to_dollars(column_name, scale=2) %}
  {{ function('cents_to_dollars') }}({{ column_name }}, {{scale}})
{% endmacro %}
```

#### Related documentation[​](#related-documentation "Direct link to Related documentation")

* [User-defined functions](https://docs.getdbt.com/docs/build/udfs.md)
* [Jinja macros](https://docs.getdbt.com/docs/build/jinja-macros.md)

## Was this page helpful?

YesNo

[Privacy policy](https://www.getdbt.com/cloud/privacy-policy)[Create a GitHub issue](https://github.com/dbt-labs/docs.getdbt.com/issues)

This site is protected by reCAPTCHA and the Google [Privacy Policy](https://policies.google.com/privacy) and [Terms of Service](https://policies.google.com/terms) apply.
