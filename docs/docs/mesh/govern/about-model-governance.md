# About model governance

dbt supports model governance to help you control who can access models, what data they contain, how they change over time, and reference them across projects. dbt supports model governance in dbt Core and the dbt platform, with some differences in the features available across environments/plans.

* Use model governance to define model structure and visibility in dbt Core and the dbt platform.
* dbt builds on this with features like [cross-project ref](https://docs.getdbt.com/docs/mesh/govern/project-dependencies.md) that enable collaboration at scale across multiple projects, powered by its metadata service and [Catalog](https://docs.getdbt.com/docs/explore/explore-projects.md). Available in dbt Enterprise or Enterprise+ plans.

All of the following features are available in dbt Core and the dbt platform, *except* project dependencies, which is only available to [dbt Enterprise-tier plans](https://www.getdbt.com/pricing).

* [**Model access**](https://docs.getdbt.com/docs/mesh/govern/model-access.md) — Mark models as "public" or "private" to distinguish between mature data products and implementation details — and to control who can `ref` each.
* [**Model contracts**](https://docs.getdbt.com/docs/mesh/govern/model-contracts.md) —Guarantee the shape of a model (column names, data types, constraints) before it builds, to prevent surprises for downstream data consumers.
* [**Model versions**](https://docs.getdbt.com/docs/mesh/govern/model-versions.md) — When a breaking change is unavoidable, provide a smoother upgrade pathway and deprecation window for downstream data consumers.
* [**Model namespaces**](https://docs.getdbt.com/reference/dbt-jinja-functions/ref.md#ref-project-specific-models) — Organize models into [groups](https://docs.getdbt.com/docs/build/groups.md) and [packages](https://docs.getdbt.com/docs/build/packages.md) to delineate ownership boundaries. Models in different packages can share the same name, and the `ref` function can take the project/package namespace as its first argument.
* [**Project dependencies**](https://docs.getdbt.com/docs/mesh/govern/project-dependencies.md) — Resolve references to public models in other projects ("cross-project ref") using an always-on stateful metadata service, instead of importing all models from those projects as packages. Each project serves data products (public model references) while managing its own implementation details, enabling an [enterprise data mesh](https://docs.getdbt.com/best-practices/how-we-mesh/mesh-1-intro.md). [Enterprise](https://www.getdbt.com/pricing "Go to https://www.getdbt.com/pricing")[Enterprise +](https://www.getdbt.com/pricing "Go to https://www.getdbt.com/pricing")

<!-- -->

#### Considerations[​](#considerations "Direct link to Considerations")

There are some considerations to keep in mind when using model governance features:

* Model governance features like model access, contracts, and versions strengthen trust and stability in your dbt project. Because they add structure, they can make rollbacks harder (for example, removing model access) and increase maintenance if adopted too early. Before adding governance features, consider whether your dbt project is ready to benefit from them. Introducing governance while models are still changing can complicate future changes.

* Governance features are model-specific. They don't apply to other resource types, including snapshots, seeds, or sources. This is because these objects can change structure over time (for example, snapshots capture evolving historical data) and aren't suited to guarantees like contracts, access, or versioning.

## Was this page helpful?

YesNo

[Privacy policy](https://www.getdbt.com/cloud/privacy-policy)[Create a GitHub issue](https://github.com/dbt-labs/docs.getdbt.com/issues)

This site is protected by reCAPTCHA and the Google [Privacy Policy](https://policies.google.com/privacy) and [Terms of Service](https://policies.google.com/terms) apply.
