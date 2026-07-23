# Set operators

Unions and intersections combine selector expressions when you list multiple arguments for `--select` or `--exclude`. A selector expression defines a subset of nodes in your project (models, tests, seeds, and other resource types) so you do not have to run the entire DAG. For the full selection language and for named selectors in `selectors.yml`, refer to [Syntax overview](https://docs.getdbt.com/reference/node-selection/syntax.md) and [YAML selectors](https://docs.getdbt.com/reference/node-selection/yaml-selectors.md).

## Unions[​](#unions "Direct link to Unions")

A union merges the node sets from several selector expressions; every resource that matches *any* of those expressions is selected. In other words, union behaves like OR across the arguments you pass to `--select` or `--exclude`.

With unions, you can pass multiple arguments separated by spaces (space-delimited). dbt resolves each argument using the normal selection rules ([selection methods](https://docs.getdbt.com/reference/node-selection/methods.md), [graph operators](https://docs.getdbt.com/reference/node-selection/graph-operators.md), and other selection syntax), then combines the results. If more than one argument matches the same node, that node appears only once in the final selection.

For example, the following command combines two selector expressions. Each uses `+` to include a model and its ancestors, and the space between them merges both sets:

```bash
dbt run --select "+snowplow_sessions +fct_orders"
```

This behavior differs from an [intersection](#intersections), where comma-separated arguments with no spaces between them require a resource to satisfy *all* criteria at once.

## Intersections[​](#intersections "Direct link to Intersections")

An intersection keeps only the nodes that match every selector expression; a resource must satisfy *all* comma-separated arguments to remain in the final set.

Use commas with no spaces between arguments to define an intersection when you pass multiple arguments to `--select` or `--exclude`. Spaces between arguments still mean a [union](#unions). dbt resolves each argument using the normal selection rules ([selection methods](https://docs.getdbt.com/reference/node-selection/methods.md), [graph operators](https://docs.getdbt.com/reference/node-selection/graph-operators.md), and other selection syntax), then keeps only resources that satisfy all of them. The order of comma-separated arguments does not change the final set.

The following examples show intersections:

Select shared upstream nodes (common ancestors of `snowplow_sessions` and `fct_orders`):

```bash
dbt run --select "+snowplow_sessions,+fct_orders"
```

Select shared downstream nodes (common descendants of `stg_invoices` and `stg_accounts`):

```bash
dbt run --select "stg_invoices+,stg_accounts+"
```

Select models that are under the `marts/finance` path and tagged `nightly`:

```bash
dbt run --select "marts.finance,tag:nightly"
```

## Combining unions and intersections[​](#combining-unions-and-intersections "Direct link to Combining unions and intersections")

You can combine unions and intersections in a single `--select` or `--exclude` value to build complex selections in one command. dbt evaluates each space-delimited argument independently: commas with no spaces within an argument define an intersection, and a space between arguments combines their results as a union. Combining both operators lets you apply different filtering logic to different subsets of your project at once.

For example, the following command contains two space-separated arguments, each defining an intersection:

* `+snowplow_sessions,+fct_orders` selects nodes that are upstream of both `snowplow_sessions` and `fct_orders`.
* `marts.finance,tag:nightly` selects models under the `marts/finance` path that are tagged `nightly`.

The space between those arguments creates a union and combines both results into one selection:

```bash
dbt run --select "+snowplow_sessions,+fct_orders marts.finance,tag:nightly"
```
