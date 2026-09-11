# About Hybrid projects

dbt platform | Enterprise+

With Hybrid projects, your organization can adopt complementary dbt v1 and dbt workflows (where some teams deploy projects in dbt v1 and others in dbt) and seamlessly integrate these workflows by automatically uploading dbt v1 [artifacts](../../reference/artifacts/dbt-artifacts.md) into dbt.

Available in public preview

Hybrid projects is available in public preview to [dbt Enterprise accounts](https://www.getdbt.com/pricing).

dbt v1 users can seamlessly upload [artifacts](../../reference/artifacts/dbt-artifacts.md) like [run results.json](../../reference/artifacts/run-results-json.md), [manifest.json](../../reference/artifacts/manifest-json.md), [catalog.json](../../reference/artifacts/catalog-json.md), [sources.json](../../reference/artifacts/sources-json.md), and so on — into dbt after executing a run in the dbt v1 command line interface (CLI), which helps:

* Collaborate with dbt + dbt v1 users by enabling them to visualize and perform [cross-project references](../mesh/govern/project-dependencies.md#how-to-write-cross-project-ref) to dbt models that live in dbt v1 projects.
* (Coming soon) New users interested in the [Canvas](../platform/canvas.md) can build off of dbt models already created by a central data team in dbt v1 rather than having to start from scratch.
* dbt v1 and dbt users can navigate to [Catalog](../explore/explore-projects.md) and view their models and assets. To view Catalog, you must have a [read-only seat](../platform/manage-access/seats-and-users.md).

## Prerequisites

To upload artifacts, make sure you meet these prerequisites:

* Your organization is on a [dbt Enterprise+ plan](https://www.getdbt.com/pricing)

* You're on [dbt's release tracks](../dbt-versions/dbt-release-tracks.md) and your dbt v1 project is on dbt v1.10 or higher

* [Configured](./hybrid-setup.md#connect-project-in-dbt-cloud) a hybrid project in dbt.

* Updated your existing dbt v1 project with latest changes and [configured it with model access](./hybrid-setup.md#make-dbt-models-public):

  * Ensure models that you want to share with other dbt projects use `access: public` in their model configuration. This makes the models more discoverable and shareable
  * Learn more about [access modifier](../mesh/govern/model-access.md#access-modifiers) and how to set the [`access` config](../../reference/resource-configs/access.md)

* Update [dbt permissions](../platform/manage-access/enterprise-permissions.md) to create a new project in dbt

**Note:** Uploading artifacts doesn't count against dbt run slots.
