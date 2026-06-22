# Building metrics

tip

Note that this best practices guide doesn't yet use the [new YAML specification](https://docs.getdbt.com/docs/build/latest-metrics-spec.md). We're working on updating this guide to use the new spec and file structure soon!

To read more about the new spec, see [Creating metrics](https://docs.getdbt.com/docs/build/metrics-overview.md).

## How to build metrics[​](#how-to-build-metrics "Direct link to How to build metrics")

* 💹 We'll start with one of the most important metrics for any business: **revenue**.
* 📖 For now, our metric for revenue will be **defined as the sum of order totals excluding tax**.

## Defining revenue[​](#defining-revenue "Direct link to Defining revenue")

* 🔢 Metrics have four basic properties:

  <!-- -->

  * `name:` We'll use 'revenue' to reference this metric.
  * `description:` For documentation.
  * `label:` The display name for the metric in downstream tools.
  * `type:` one of `simple`, `ratio`, or `derived`.

* 🎛️ Each type has different `type_params`.

* 🛠️ We'll build a **simple metric** first to get the hang of it, and move on to ratio and derived metrics later.

* 📏 Simple metrics are built on a **single measure defined as a type parameter**.

* 🔜 Defining **measures as their own distinct component** on semantic models is critical to allowing the **flexibility of more advanced metrics**, though simple metrics act mainly as **pass-through that provide filtering** and labeling options.

models/marts/orders.yml

```yml
metrics:
  - name: revenue
    description: Sum of the order total.
    label: Revenue
    type: simple
    type_params:
      measure: order_total
```

## Query your metric[​](#query-your-metric "Direct link to Query your metric")

You can use the dbt CLI for metric validation or queries during development, via the `dbt sl` set of subcommands. Here are some useful examples:

```bash
dbt sl query revenue --group-by metric_time__month
dbt sl list dimensions --metrics revenue # list all dimensions available for the revenue metric
```

* It's best practice any time we're updating our Semantic Layer code to run `dbt parse` to update our development semantic manifest.
* `dbt sl query` is not how you would typically use the tool in production, that's handled by the dbt Semantic Layer's features. It's available for testing results of various metric queries in development, exactly as we're using it now.
* Note the structure of the above query. We select the metric(s) we want and the dimensions to group them by — we use dunders (double underscores e.g.`metric_time__[time bucket]`) to designate time dimensions or other non-unique dimensions that need a specified entity path to resolve (e.g. if you have an orders location dimension and an employee location dimension both named 'location' you would need dunders to specify `orders__location` or `employee__location`).

## Was this page helpful?

YesNo

[Privacy policy](https://www.getdbt.com/cloud/privacy-policy)[Create a GitHub issue](https://github.com/dbt-labs/docs.getdbt.com/issues)

This site is protected by reCAPTCHA and the Google [Privacy Policy](https://policies.google.com/privacy) and [Terms of Service](https://policies.google.com/terms) apply.
