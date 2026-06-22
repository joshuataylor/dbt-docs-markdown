# About dbt Mesh

Organizations of all sizes rely upon dbt to manage their data transformations, from small startups to large enterprises. At scale, it can be challenging to coordinate all the organizational and technical requirements demanded by your stakeholders within the scope of a single dbt project.

To date, there also hasn't been a first-class way to effectively manage the dependencies, governance, and workflows between multiple dbt projects.

That's where **Mesh** comes in - empowering data teams to work *independently and collaboratively*; sharing data, code, and best practices without sacrificing security or autonomy.

Mesh is not a single product - it is a pattern enabled by a convergence of several features in dbt:

* **[Cross-project references](https://docs.getdbt.com/docs/mesh/govern/project-dependencies.md#how-to-write-cross-project-ref)** - this is the foundational feature that enables the multi-project deployments. `{{ ref() }}`s now work across dbt projects on Enterprise and Enterprise+ plans.

* **[Catalog](https://docs.getdbt.com/docs/explore/explore-projects.md)** - dbt's metadata-powered documentation platform, complete with full, cross-project lineage.

* **Governance** - dbt's governance features allow you to manage access to your dbt models both within and across projects.

  <!-- -->

  * **[Groups](https://docs.getdbt.com/docs/mesh/govern/model-access.md#groups)** - With groups, you can organize nodes in your dbt DAG that share a logical connection (for example, by functional area) and assign an owner to the entire group.
  * **[Access](https://docs.getdbt.com/docs/mesh/govern/model-access.md#access-modifiers)** - access configs allow you to control who can reference models.
  * **[Model Versions](https://docs.getdbt.com/docs/mesh/govern/model-versions.md)** - when coordinating across projects and teams, we recommend treating your data models as stable APIs. Model versioning is the mechanism to allow graceful adoption and deprecation of models as they evolve.
  * **[Model Contracts](https://docs.getdbt.com/docs/mesh/govern/model-contracts.md)** - data contracts set explicit expectations on the shape of the data to ensure data changes upstream of dbt or within a project's logic don't break downstream consumers' data products.

## When is the right time to use dbt Mesh?[​](#when-is-the-right-time-to-use-dbt-mesh "Direct link to When is the right time to use dbt Mesh?")

The multi-project architecture helps organizations with mature, complex transformation workflows in dbt increase the flexibility and performance of their dbt projects. If you're already using dbt and your project has started to experience any of the following, you're likely ready to start exploring this paradigm:

* The **number of models** in your project is degrading performance and slowing down development.
* Teams have developed **separate workflows** and need to decouple development from each other.
* Teams are experiencing **communication challenges**, and the reliability of some of your data products has started to deteriorate.
* **Security and governance** requirements are increasing and would benefit from increased isolation.

dbt is designed to coordinate the features above and simplify the complexity to solve for these problems.

If you're just starting your dbt journey, don't worry about building a multi-project architecture right away. You can *incrementally* adopt the features as you scale. The collection of features work effectively as independent tools. Familiarizing yourself with the tooling and features that make up a multi-project architecture, and how they can apply to your organization will help you make better decisions as you grow.

For additional information, refer to the [Mesh FAQs](https://docs.getdbt.com/best-practices/how-we-mesh/mesh-5-faqs.md).

## Was this page helpful?

YesNo

[Privacy policy](https://www.getdbt.com/cloud/privacy-policy)[Create a GitHub issue](https://github.com/dbt-labs/docs.getdbt.com/issues)

This site is protected by reCAPTCHA and the Google [Privacy Policy](https://policies.google.com/privacy) and [Terms of Service](https://policies.google.com/terms) apply.
