# Jinja and macros

## Related reference docs[​](#related-reference-docs "Direct link to Related reference docs")

* [Jinja Template Designer Documentation](https://jinja.palletsprojects.com/page/templates/) (external link)
* [dbt Jinja context](https://docs.getdbt.com/reference/dbt-jinja-functions-context-variables.md)
* [Macro properties](https://docs.getdbt.com/reference/macro-properties.md)

## Overview[​](#overview "Direct link to Overview")

In dbt, you can combine SQL with [Jinja](https://jinja.palletsprojects.com), a templating language.

Using Jinja turns your dbt project into a programming environment for SQL, giving you the ability to do things that aren't normally possible in SQL. It's important to note that Jinja itself isn't a programming language; instead, it acts as a tool to enhance and extend the capabilities of SQL within your dbt projects.

For example, with Jinja, you can:

* Use control structures (for example, `if` statements and `for` loops) in SQL

* Use [environment variables](https://docs.getdbt.com/reference/dbt-jinja-functions/env_var.md) in your dbt project for production deployments

* Change the way your project builds based on the current target.

* Operate on the results of one query to generate another query, for example:

  <!-- -->

  * Return a list of payment methods, to create a subtotal column per payment method (pivot)
  * Return a list of columns in two relations, and select them in the same order to make it easier to union them together

* Abstract snippets of SQL into reusable [**macros**](#macros) — these are analogous to functions in most programming languages.

If you've used the [`{{ ref() }}` function](https://docs.getdbt.com/reference/dbt-jinja-functions/ref.md), you're already using Jinja!

Jinja can be used in any SQL in a dbt project, including [models](https://docs.getdbt.com/docs/build/sql-models.md), [analyses](https://docs.getdbt.com/docs/build/analyses.md), [data tests](https://docs.getdbt.com/docs/build/data-tests.md), and even [hooks](https://docs.getdbt.com/docs/build/hooks-operations.md).

Ready to get started with Jinja and macros?

Check out the [tutorial on using Jinja](https://docs.getdbt.com/guides/using-jinja.md) for a step-by-step example of using Jinja in a model, and turning it into a macro!

## Getting started[​](#getting-started "Direct link to Getting started")

### Jinja[​](#jinja "Direct link to Jinja")

Here's an example of a dbt model that leverages Jinja:

/models/order\_payment\_method\_amounts.sql

```sql
{% set payment_methods = ["bank_transfer", "credit_card", "gift_card"] %}

select
    order_id,
    {% for payment_method in payment_methods %}
    sum(case when payment_method = '{{payment_method}}' then amount end) as {{payment_method}}_amount,
    {% endfor %}
    sum(amount) as total_amount
from app_data.payments
group by 1
```

This query will get compiled to:

/models/order\_payment\_method\_amounts.sql

```sql
select
    order_id,
    sum(case when payment_method = 'bank_transfer' then amount end) as bank_transfer_amount,
    sum(case when payment_method = 'credit_card' then amount end) as credit_card_amount,
    sum(case when payment_method = 'gift_card' then amount end) as gift_card_amount,
    sum(amount) as total_amount
from app_data.payments
group by 1
```

You can recognize Jinja based on the delimiters the language uses, which we refer to as "curlies":

* **Expressions `{{ ... }}`**: Expressions are used when you want to output a string. You can use expressions to reference [variables](https://docs.getdbt.com/reference/dbt-jinja-functions/var.md) and call [macros](https://docs.getdbt.com/docs/build/jinja-macros.md#macros).
* **Statements `{% ... %}`**: Statements don't output a string. They are used for control flow, for example, to set up `for` loops and `if` statements, to [set](https://jinja.palletsprojects.com/en/3.1.x/templates/#assignments) or [modify](https://jinja.palletsprojects.com/en/3.1.x/templates/#expression-statement) variables, or to define macros.
* **Comments `{# ... #}`**: Jinja comments are used to prevent the text within the comment from executing or outputing a string. Don't use `--` for comment.

When used in a dbt model, your Jinja needs to compile to a valid query. To check what SQL your Jinja compiles to:

* **Using dbt:** Click the compile button to see the compiled SQL in the Compiled SQL pane
* **Using dbt Core:** Run `dbt compile` from the command line. Then open the compiled SQL file in the `target/compiled/{project name}/` directory. Use a split screen in your code editor to keep both files open at once.

### Macros[​](#macros "Direct link to Macros")

[Macros](https://docs.getdbt.com/docs/build/jinja-macros.md) in Jinja are pieces of code that can be reused multiple times – they are analogous to "functions" in other programming languages, and are extremely useful if you find yourself repeating code across multiple models. Macros are defined in `.sql` files, typically in your `macros` directory ([docs](https://docs.getdbt.com/reference/project-configs/macro-paths.md)).

Macro files can contain one or more macros — here's an example:

macros/cents\_to\_dollars.sql

```sql

{% macro cents_to_dollars(column_name, scale=2) %}
    ({{ column_name }} / 100)::numeric(16, {{ scale }})
{% endmacro %}
```

A model which uses this macro might look like:

models/stg\_payments.sql

```sql
select
  id as payment_id,
  {{ cents_to_dollars('amount') }} as amount_usd,
  ...
from app_data.payments
```

This would be *compiled* to:

target/compiled/models/stg\_payments.sql

```sql
select
  id as payment_id,
  (amount / 100)::numeric(16, 2) as amount_usd,
  ...
from app_data.payments
```

<!-- -->

💡 Use Jinja's whitespace control to tidy your macros!

When you're modifying macros in your project, you might notice extra white space in your code in the `target/compiled` folder.

You can remove unwanted spaces and lines with Jinja's [whitespace control](https://docs.getdbt.com/faqs/Jinja/jinja-whitespace.md) by using a minus sign. For example, use `{{- ... -}}` or `{%- ... %}` around your macro definitions (such as `{%- macro generate_schema_name(...) -%} ... {%- endmacro -%}`).

### Using a macro from a package[​](#using-a-macro-from-a-package "Direct link to Using a macro from a package")

A number of useful macros have also been grouped together into [packages](https://docs.getdbt.com/docs/build/packages.md) — our most popular package is [dbt-utils](https://hub.getdbt.com/dbt-labs/dbt_utils/latest/).

After installing a package into your project, you can use any of the macros in your own project — make sure you qualify the macro by prefixing it with the [package name](https://docs.getdbt.com/reference/dbt-jinja-functions/project_name.md):

```sql

select
  field_1,
  field_2,
  field_3,
  field_4,
  field_5,
  count(*)
from my_table
{{ dbt_utils.dimensions(5) }}
```

You can also qualify a macro in your own project by prefixing it with your [package name](https://docs.getdbt.com/reference/dbt-jinja-functions/project_name.md) (this is mainly useful for package authors).

## FAQs[​](#faqs "Direct link to FAQs")

What parts of Jinja are dbt-specific?

There are certain expressions that are specific to dbt — these are documented in the [Jinja function reference](https://docs.getdbt.com/reference/dbt-jinja-functions-context-variables.md) section of these docs. Further, docs blocks, snapshots, and materializations are custom Jinja *blocks* that exist only in dbt.

Which docs should I use when writing Jinja or creating a macro?

If you are stuck with a Jinja issue, it can get confusing where to check for more information. We recommend you check (in order):

1. [Jinja's Template Designer Docs](https://jinja.palletsprojects.com/page/templates/): This is the best reference for most of the Jinja you'll use
2. [Our Jinja function reference](https://docs.getdbt.com/reference/dbt-jinja-functions-context-variables.md): This documents any additional functionality we've added to Jinja in dbt.
3. [Agate's table docs](https://agate.readthedocs.io/page/api/table.html): If you're operating on the result of a query, dbt will pass it back to you as an agate table. This means that the methods you call on the table belong to the Agate library rather than Jinja or dbt.

Why do I need to quote column names in Jinja?

In the [macro example](https://docs.getdbt.com/docs/build/jinja-macros.md#macros) we passed the column name `amount` quotes:

```sql
{{ cents_to_dollars('amount') }} as amount_usd
```

We have to use quotes to pass the *string* `'amount'` to the macro.

Without the quotes, the Jinja parser will look for a variable named `amount`. Since this doesn't exist, it will compile to nothing.

Quoting in Jinja can take a while to get used to! The rule is that you're within a Jinja expression or statement (i.e. within `{% ... %}` or `{{ ... }}`), you'll need to use quotes for any arguments that are strings.

Single and double quotes are equivalent in Jinja – just make sure you match them appropriately.

And if you do need to pass a variable as an argument, make sure you [don't nest your curlies](https://docs.getdbt.com/best-practices/dont-nest-your-curlies.md).

My compiled SQL has a lot of spaces and new lines, how can I get rid of it?

This is known as "whitespace control".

Use a minus sign (`-`, e.g. `{{- ... -}}`, `{%- ... %}`, `{#- ... -#}`) at the start or end of a block to strip whitespace before or after the block (more docs [here](https://jinja.palletsprojects.com/page/templates/#whitespace-control)). Check out the [tutorial on using Jinja](https://docs.getdbt.com/guides/using-jinja.md#use-whitespace-control-to-tidy-up-compiled-code) for an example.

Take caution: it's easy to fall down a rabbit hole when it comes to whitespace control!

How do I debug my Jinja?

You should get familiar with checking the compiled SQL in `target/compiled/<your_project>/` and the logs in `logs/dbt.log` to see what dbt is running behind the scenes.

You can also use the [log](https://docs.getdbt.com/reference/dbt-jinja-functions/log.md) function to debug Jinja by printing objects to the command line.

How do I document macros?

To document macros, use a [properties file](https://docs.getdbt.com/reference/macro-properties.md) and nest the configurations under a `macros:` key

## Example[​](#example "Direct link to Example")

macros/properties.yml

```yml
macros:
  - name: cents_to_dollars
    description: A macro to convert cents to dollars
    arguments:
      - name: column_name
        type: column
        description: The name of the column you want to convert
      - name: precision
        type: integer
        description: Number of decimal places. Defaults to 2.
```

tip

From dbt Core v1.10, you can opt into validating the arguments you define in macro documentation using the `validate_macro_args` behavior change flag. When enabled, dbt will:

* Infer arguments from the macro and includes them in the [manifest.json](https://docs.getdbt.com/reference/artifacts/manifest-json.md) file if no arguments are documented.
* Raise a warning if documented argument names don't match the macro definition.
* Raise a warning if `type` fields don't follow [supported formats](https://docs.getdbt.com/reference/resource-properties/arguments.md#supported-types).

Learn more about [macro argument validation](https://docs.getdbt.com/reference/global-configs/behavior-changes.md#macro-argument-validation).

## Document a custom materialization[​](#document-a-custom-materialization "Direct link to Document a custom materialization")

When you create a [custom materialization](https://docs.getdbt.com/guides/create-new-materializations.md), dbt creates an associated macro with the following format:

```text
materialization_{materialization_name}_{adapter}
```

To document a custom materialization, use the previously mentioned format to determine the associated macro name(s) to document.

macros/properties.yml

```yaml
macros:
  - name: materialization_my_materialization_name_default
    description: A custom materialization to insert records into an append-only table and track when they were added.
  - name: materialization_my_materialization_name_xyz
    description: A custom materialization to insert records into an append-only table and track when they were added.
```

Why does my dbt output have so many macros in it?

The output of a dbt run counts over 100 macros in your project!

```shell
$ dbt run
Running with dbt=1.7.0
Found 1 model, 0 tests, 0 snapshots, 0 analyses, 138 macros, 0 operations, 0 seed files, 0 sources
```

This is because dbt ships with its own project, which also includes macros! You can learn more about this [here](https://discourse.getdbt.com/t/did-you-know-dbt-ships-with-its-own-project/764).

## dbtonic Jinja[​](#dbtonic-jinja "Direct link to dbtonic Jinja")

Just like well-written python is pythonic, well-written dbt code is dbtonic.

### Favor readability over DRY-ness[​](#favor-readability-over-dry-ness "Direct link to favor-readability-over-dry-ness")

Once you learn the power of Jinja, it's common to want to abstract every repeated line into a macro! Remember that using Jinja can make your models harder for other users to interpret — we recommend favoring readability when mixing Jinja with SQL, even if it means repeating some lines of SQL in a few places. If all your models are macros, it might be worth re-assessing.

### Leverage package macros[​](#leverage-package-macros "Direct link to Leverage package macros")

Writing a macro for the first time? Check whether we've open sourced one in [dbt-utils](https://hub.getdbt.com/dbt-labs/dbt_utils/latest/) that you can use, and save yourself some time!

### Set variables at the top of a model[​](#set-variables-at-the-top-of-a-model "Direct link to Set variables at the top of a model")

`{% set ... %}` can be used to create a new variable, or update an existing one. We recommend setting variables at the top of a model, rather than hardcoding it inline. This is a practice borrowed from many other coding languages, since it helps with readability, and comes in handy if you need to reference the variable in two places:

```sql
-- 🙅 This works, but can be hard to maintain as your code grows
{% for payment_method in ["bank_transfer", "credit_card", "gift_card"] %}
...
{% endfor %}


-- ✅ This is our preferred method of setting variables
{% set payment_methods = ["bank_transfer", "credit_card", "gift_card"] %}

{% for payment_method in payment_methods %}
...
{% endfor %}
```

### Questions from the Community[​](#questions-from-the-community "Direct link to Questions from the Community")

![Loading](/img/loader-icon.svg)[Ask the Community](https://discourse.getdbt.com/new-topic?category=help\&tags=wee "Ask the Community")

## Was this page helpful?

YesNo

[Privacy policy](https://www.getdbt.com/cloud/privacy-policy)[Create a GitHub issue](https://github.com/dbt-labs/docs.getdbt.com/issues)

This site is protected by reCAPTCHA and the Google [Privacy Policy](https://policies.google.com/privacy) and [Terms of Service](https://policies.google.com/terms) apply.
