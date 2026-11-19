# About hybrid projects

dbt platform | Enterprise+

With hybrid projects, your organization can integrate local or externally deployed dbt v1 and dbt v2 workflows with the dbt platform by automatically uploading [artifacts](../../reference/artifacts/dbt-artifacts.md) to the platform.

Available in public preview

Hybrid projects are available in public preview to [dbt platform Enterprise+ accounts](https://www.getdbt.com/pricing).

You can upload artifacts such as [run\_results.json](../../reference/artifacts/run-results-json.md), [manifest.json](../../reference/artifacts/manifest-json.md), [catalog.json](../../reference/artifacts/catalog-json.md), and [sources.json](../../reference/artifacts/sources-json.md) to the dbt platform after executing a run locally or in an external environment. This helps you:

* Collaborate across projects by visualizing and using [cross-project references](../mesh/govern/project-dependencies.md#how-to-write-cross-project-ref) to models in hybrid projects.
* View models and assets in [Catalog](../explore/explore-projects.md). To view Catalog, you must have a [read-only seat](../platform/manage-access/seats-and-users.md).

Hybrid projects can also upload [semantic\_manifest.json](../../reference/artifacts/sl-manifest.md). In dbt v2, hybrid projects upload the artifact when the run generates it using the [latest Semantic Layer specification](../build/latest-metrics-spec.md). In dbt v1, you can upload the semantic manifest from versions 1.11.16 and later, 1.12.6 and later, and 1.13.0b1 and later.

## Prerequisites

To upload artifacts, make sure you meet these prerequisites:

* Your organization is on a [dbt platform Enterprise+ plan](https://www.getdbt.com/pricing).

* Your dbt platform account is on [release tracks](../dbt-versions/dbt-release-tracks.md), and your project is configured for the [latest supported version](../dbt-versions.md) of dbt.

* You have [configured](./hybrid-setup.md#create-hybrid-project) a hybrid project in the dbt platform.

* You have updated your existing dbt project with the latest changes and [configured it with model access](./hybrid-setup.md#make-dbt-models-public):

  * Ensure models that you want to share with other dbt platform projects use `access: public` in their model configuration. This makes the models more discoverable and shareable.
  * Learn more about [access modifiers](../mesh/govern/model-access.md#access-modifiers) and how to set the [`access` config](../../reference/resource-configs/access.md).

* You have updated your [dbt platform permissions](../platform/manage-access/enterprise-permissions.md) to create a new project in the dbt platform.

note

Uploading artifacts doesn't count against dbt platform run slots.
