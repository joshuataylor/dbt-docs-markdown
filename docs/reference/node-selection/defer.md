# Defer

Defer is a powerful feature that makes it possible to run a subset of models or tests in a [sandbox environment](https://docs.getdbt.com/docs/environments-in-dbt.md) without having to first build their upstream parents. This can save time and computational resources when you want to test a small number of models in a large project.

[![Use 'defer' to modify end-of-pipeline models by pointing to production models, instead of running everything upstream.](/img/docs/reference/defer-diagram.png?v=2 "Use 'defer' to modify end-of-pipeline models by pointing to production models, instead of running everything upstream.")](#)Use 'defer' to modify end-of-pipeline models by pointing to production models, instead of running everything upstream.

Defer requires a manifest from a previous dbt invocation. Provide the path using the `--state flag` or by setting the `DBT_STATE` environment variable. Together with the `state:` selection method, these features enable "Slim CI". Read more about [state](https://docs.getdbt.com/reference/node-selection/state-selection.md).

For some use cases, you can use `dbt clone` to achieve similar functionality. For more details, refer to [clone](https://docs.getdbt.com/reference/commands/clone.md#when-to-use-dbt-clone-instead-of-deferral).

It is possible to use separate state for `state:modified` and `--defer`, by passing paths to different manifests to each of the `--state`/`DBT_STATE` and `--defer-state`/`DBT_DEFER_STATE`. This enables more granular control in cases where you want to:

* Compare against logical state from one environment or past point in time
* Defer to applied state from a different environment or point in time

If `--defer-state` is not specified, deferral will use the manifest supplied to `--state`. In most cases, you will want to use the same state for both; compare logical changes against production, and also "fail over" to the production environment for unbuilt upstream resources.

### Usage[​](#usage "Direct link to Usage")

```shell
dbt run --select [...] --defer --state path/to/artifacts
dbt test --select [...] --defer --state path/to/artifacts
```

By default, dbt uses the [`target`](https://docs.getdbt.com/reference/dbt-jinja-functions/target.md) namespace to resolve `ref` calls.

When `--defer` is enabled, dbt resolves `ref` calls using the state manifest instead, but only if:

1. The node isn’t among the selected nodes, *and*
2. It doesn’t exist in the database (or `--favor-state` is used).

Ephemeral models are never deferred, since they serve as "passthroughs" for other `ref` calls.

info

When using defer, you may be selecting from production datasets, development datasets, or a mix of both. Note that this can yield unexpected results:

* If you apply environment-specific limits in development but not in production, you may select more data than expected.
* Tests that depend on multiple parents (for example, `relationships`), may run across environments.

Deferral requires both `--defer` and `--state` to be set, either by passing flags explicitly or by setting environment variables (`DBT_DEFER` and `DBT_STATE`). Refer to [Continuous integration](https://docs.getdbt.com/docs/deploy/continuous-integration.md) for more information.

#### Favor state[​](#favor-state "Direct link to Favor state")

When `--favor-state` is passed, dbt prioritizes node definitions from the `--state` directory. However, this doesn’t apply if the node is also part of the selected nodes.

### Example[​](#example "Direct link to Example")

In my local development environment, I create all models in my target schema, `dev_alice`. In production, the same models are created in a schema named `prod`.

I access the dbt-generated [artifacts](https://docs.getdbt.com/docs/deploy/artifacts.md) (namely `manifest.json`) from a production run, and copy them into a local directory called `prod-run-artifacts`.

### run[​](#run "Direct link to run")

I've been working on `model_b`:

models/model\_b.sql

```sql
select

    id,
    count(*)

from {{ ref('model_a') }}
group by 1
```

I want to test my changes. Nothing exists in my development schema, `dev_alice`.

* Standard run
* Deferred run

```shell
dbt run --select "model_b"
```

target/run/my\_project/model\_b.sql

```sql
create or replace view dev_me.model_b as (

    select

        id,
        count(*)

    from dev_alice.model_a
    group by 1

)
```

Unless I had previously run `model_a` into this development environment, `dev_alice.model_a` will not exist, thereby causing a database error.

```shell
dbt run --select "model_b" --defer --state prod-run-artifacts
```

target/run/my\_project/model\_b.sql

```sql
create or replace view dev_me.model_b as (

    select

        id,
        count(*)

    from prod.model_a
    group by 1

)
```

Because `model_a` is unselected, dbt will check to see if `dev_alice.model_a` exists. If it doesn't exist, dbt will resolve all instances of `{{ ref('model_a') }}` to `prod.model_a` instead.

### test[​](#test "Direct link to test")

I also have a `relationships` test that establishes referential integrity between `model_a` and `model_b`:

models/resources.yml

```yml

models:
  - name: model_b
    columns:
      - name: id
        data_tests:
          - relationships:
              arguments: # available in v1.10.5 and higher. Older versions can set the <argument_name> as the top-level property.
                to: ref('model_a')
                field: id
```

(This is a simplified example, since all the data in `model_b` already comes from `model_a`)

* Without defer
* With defer

```shell
dbt test --select "model_b"
```

target/compiled/.../relationships\_model\_b\_id\_\_id\_\_ref\_model\_a\_.sql

```sql
select count(*) as validation_errors
from (
    select id as id from dev_alice.model_b
) as child
left join (
    select id as id from dev_alice.model_a
) as parent on parent.id = child.id
where child.id is not null
  and parent.id is null
```

The `relationships` test requires both `model_a` and `model_b`. Because I did not build `model_a` in my previous `dbt run`, `dev_alice.model_a` does not exist and this test query fails.

```shell
dbt test --select "model_b" --defer --state prod-run-artifacts
```

target/compiled/.../relationships\_model\_b\_id\_\_id\_\_ref\_model\_a\_.sql

```sql
select count(*) as validation_errors
from (
    select id as id from dev_alice.model_b
) as child
left join (
    select id as id from prod.model_a
) as parent on parent.id = child.id
where child.id is not null
  and parent.id is null
```

dbt will check to see if `dev_alice.model_a` exists. If it doesn't exist, dbt will resolve all instances of `{{ ref('model_a') }}`, including those in schema tests, to use `prod.model_a` instead. The query succeeds. Whether I really want to test for referential integrity across environments is a different question.

## Related docs[​](#related-docs "Direct link to Related docs")

* [Using defer in dbt](https://docs.getdbt.com/docs/cloud/about-cloud-develop-defer.md)
* [on\_configuration\_change](https://docs.getdbt.com/reference/resource-configs/on_configuration_change.md)

## Was this page helpful?

YesNo

[Privacy policy](https://www.getdbt.com/cloud/privacy-policy)[Create a GitHub issue](https://github.com/dbt-labs/docs.getdbt.com/issues)

This site is protected by reCAPTCHA and the Google [Privacy Policy](https://policies.google.com/privacy) and [Terms of Service](https://policies.google.com/terms) apply.
