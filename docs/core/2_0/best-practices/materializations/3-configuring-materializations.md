# Configuring materializations

## Configuring materializations[​](#configuring-materializations "Direct link to Configuring materializations")

Choosing which materialization is as simple as setting any other configuration in dbt. We’ll look first at how we select our materializations for individual models, then at more powerful ways of setting materializations for entire folders of models.

### Configuring tables and views[​](#configuring-tables-and-views "Direct link to Configuring tables and views")

Let’s look at how we can use tables and views to get started with materializations:

* ⚙️ We can configure an individual model’s materialization using a **Jinja `config` block**, and passing in the **`materialized` argument**. This tells dbt what materialization to use.
* 🚰 The underlying specifics of what is run depends on [which **adapter** you’re using](https://docs.getdbt.com/docs/supported-data-platforms.md), but the end results will be equivalent.
* 😌 This is one of the many valuable aspects of dbt: it lets us use a **declarative** approach, specifying the *outcome* that we want in our code, rather than *specific steps* to achieve it (the latter is an *imperative* approach if you want to get computer science-y about it 🤓).
* 🔍 In the below case, we want to create a SQL **view**, and can **declare** that in a **single line of code**. Note that python models [do not support materializing as views](https://docs.getdbt.com/docs/build/materializations.md#python-materializations) at this time.

```sql
    {{
        config(
            materialized='view'
        )
    }}

    select ...
```

info

🐍 **Not all adapters support python yet**, check the [docs here to be sure](https://docs.getdbt.com/docs/build/python-models.md#specific-data-platforms) before spending time writing python models.

* Configuring a model to materialize as a `table` is simple, and possible for both SQL and python models.

- SQL
- Python

```sql
{{
    config(
        materialized='table'
    )
}}

select ...
```

```python
def model(dbt, session):

    dbt.config(materialized="table")

    # model logic

    return model_df
```

Go ahead and try some of these out!

## Was this page helpful?

YesNo

[Privacy policy](https://www.getdbt.com/cloud/privacy-policy)[Create a GitHub issue](https://github.com/dbt-labs/docs.getdbt.com/issues)

This site is protected by reCAPTCHA and the Google [Privacy Policy](https://policies.google.com/privacy) and [Terms of Service](https://policies.google.com/terms) apply.
