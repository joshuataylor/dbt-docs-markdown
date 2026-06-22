# Intro to the dbt Semantic Layer

tip

Note that this best practices guide doesn't yet use the [new YAML specification](https://docs.getdbt.com/docs/build/latest-metrics-spec.md). We're working on updating this guide to use the new spec and file structure soon!

To read more about the new spec, see [Creating metrics](https://docs.getdbt.com/docs/build/metrics-overview.md).

Flying cars, hoverboards, and true self-service analytics: this is the future we were promised. The first two might still be a few years out, but real self-service analytics is here today. With dbt's Semantic Layer, you can resolve the tension between accuracy and flexibility that has hampered analytics tools for years, empowering everybody in your organization to explore a shared reality of metrics. Best of all for analytics engineers, building with these new tools will significantly [DRY](https://docs.getdbt.com/terms/dry) up and simplify your codebase. As you'll see, the deep interaction between your dbt models and the Semantic Layer make your dbt project the ideal place to craft your metrics.

## Learning goals[​](#learning-goals "Direct link to Learning goals")

* ❓ Understand the **purpose and capabilities** of the **Semantic Layer**, particularly MetricFlow as the engine that powers it.
* 🧱 Familiarity with the core components of MetricFlow — **semantic models and metrics** — and how they work together.
* 🔁 Know how to **refactor** dbt models for the Semantic Layer.
* 🏅 Aware of **best practices** to take maximum advantage of the Semantic Layer.

## Guide structure overview[​](#guide-structure-overview "Direct link to Guide structure overview")

1. Getting **setup** in your dbt project.
2. Building a **semantic model** and its fundamental parts: **entities, dimensions, and measures**.
3. Building a **metric**.
4. Defining **advanced metrics**: `ratio` and `derived` types.
5. **File and folder structure**: establishing a system for naming things.
6. **Refactoring** marts and roll-ups for the Semantic Layer.
7. Review **best practices**.

If you're ready to ship your users more power and flexibility with less code, let's dive in!

info

MetricFlow is the engine for defining metrics in dbt and one of the key components of the [Semantic Layer](https://docs.getdbt.com/docs/use-dbt-semantic-layer/dbt-sl.md). It handles SQL query construction and defines the specification for dbt semantic models and metrics.

To fully experience the Semantic Layer, including the ability to query dbt metrics via external integrations, you'll need a [dbt Starter, Enterprise, or Enterprise+ accounts](https://www.getdbt.com/pricing/). Refer to [Semantic Layer FAQs](https://docs.getdbt.com/docs/use-dbt-semantic-layer/sl-faqs.md) for more information.

## Was this page helpful?

YesNo

[Privacy policy](https://www.getdbt.com/cloud/privacy-policy)[Create a GitHub issue](https://github.com/dbt-labs/docs.getdbt.com/issues)

This site is protected by reCAPTCHA and the Google [Privacy Policy](https://policies.google.com/privacy) and [Terms of Service](https://policies.google.com/terms) apply.
