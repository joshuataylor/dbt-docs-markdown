# Project dependencies

dbt platform | Enterprise, Enterprise+

Available on dbt [Enterprise or Enterprise+](https://www.getdbt.com/pricing) plans.

For a long time, dbt has supported code reuse and extension by installing other projects as [packages](../../build/packages.md). When you install another project as a package, you are pulling in its full source code, and adding it to your own. This enables you to call macros and run models defined in that other project.

While this is a great way to reuse code, share utility macros, and establish a starting point for common transformations, it's not a great way to enable collaboration across teams and at scale, especially in larger organizations.

dbt Labs supports an expanded notion of `dependencies` across multiple dbt projects:

* **Packages** — Familiar and pre-existing type of dependency. You take this dependency by installing the package's full source code (like a software library).
* **Projects** — The dbt method to take a dependency on another project. Using a metadata service that runs behind the scenes, dbt resolves references on-the-fly to public models defined in other projects. You don't need to parse or run those upstream models yourself. Instead, you treat your dependency on those models as an API that returns a dataset. The maintainer of the public model is responsible for guaranteeing its quality and stability.

## Prerequisites

* Available in [dbt Enterprise or Enterprise+](https://www.getdbt.com/pricing). To use it, designate a [public model](./model-access.md) and add a [cross-project ref](#how-to-write-cross-project-ref).

* For the upstream ("producer") project setup:

  * Configure models in upstream project with [`access: public`](../../../reference/resource-configs/access.md) and have at least one successful job run after defining `access`.
  * Define a [Production deployment environment](../../deploy/deploy-environments.md#set-as-production-environment) in the upstream project and make sure at least *one deployment job* has run successfully there. This job should generate a [`manifest.json` file](../../../reference/artifacts/manifest-json.md) — it includes the metadata needed for downstream projects.
  * If the upstream project has a Staging environment, run at least one successful deployment job there to ensure downstream cross-project references resolve correctly.

* Each project `name` must be unique in your dbt account. For example, if you have a dbt project (codebase) for the `jaffle_marketing` team, avoid creating projects for `Jaffle Marketing - Dev` and `Jaffle Marketing - Prod`; use [environment-level isolation](../../dbt-platform-environments.md#types-of-environments) instead.
  * dbt supports [Connections](../../platform/connect-data-platform/about-connections.md#connection-management), available to all dbt users. Connections allows different data platform connections per environment, eliminating the need to duplicate projects. Projects can use multiple connections of the same warehouse type. Connections are reusable across projects and environments.

* The `dbt_project.yml` file is case-sensitive, which means the project name must exactly match the name in your `dependencies.yml`. For example, `jaffle_marketing`, not `JAFFLE_MARKETING`.

## Use cases

The following setup will work for every dbt project:

* Add [any package dependencies](./project-dependencies.md#when-to-use-project-dependencies) to `packages.yml`
* Add [any project dependencies](./project-dependencies.md#when-to-use-package-dependencies) to `dependencies.yml`

However, you may be able to consolidate both into a single `dependencies.yml` file. Read the following section to learn more.

#### About packages.yml and dependencies.yml

The `dependencies.yml` file can contain both types of dependencies: "package" and "project" dependencies.

* [Package dependencies](../../build/packages.md#how-do-i-add-a-package-to-my-project) lets you add source code from someone else's dbt project into your own, like a library.
* Project dependencies provide a different way to build on top of someone else's work in dbt.
* For private Git packages that need Jinja (for example the [Git token method](../../build/packages.md#git-token-method)), use `packages.yml` on dbt v1, which supports Jinja in package specs. dbt v2 also supports Jinja in `dependencies.yml`, but use `packages.yml` if the project must also run on dbt v1. Refer to [Jinja support by file type](../../../reference/jinja-file-support.md).

For private packages without Jinja tokens, you can use [native private packages](../../build/packages.md#native-private-packages) in `packages.yml` or `dependencies.yml`.

If your dbt project doesn't require Jinja in package specifications, you can rename `packages.yml` to `dependencies.yml`. If you need Jinja in package specs (for example an environment variable or the [Git token method](../../build/packages.md#git-token-method)):

* On dbt v1, keep using `packages.yml`. dbt v1 does not render Jinja in `dependencies.yml`.
* On the dbt v2, you can use Jinja in `dependencies.yml`. It is recommended to use `packages.yml` if the project must also run on dbt v1.

Refer to [Jinja support by file type](../../../reference/jinja-file-support.md) for the full matrix. Use the following toggles to understand the differences and determine when to use `dependencies.yml` or `packages.yml` (or both). Refer to the [FAQs](#faqs) for more info.

 When to use Project dependencies

Project dependencies are designed for the [dbt Mesh](../../../best-practices/how-we-mesh/mesh-1-intro.md) and [cross-project reference](./project-dependencies.md#how-to-write-cross-project-ref) workflow:

* Use `dependencies.yml` when you need to set up cross-project references between different dbt projects, especially in a dbt Mesh setup.
* Use `dependencies.yml` when you want to include both projects and non-private dbt packages in your project's dependencies.
* Use `dependencies.yml` for organization and maintainability if you're using both [cross-project refs](./project-dependencies.md#how-to-write-cross-project-ref) and [dbt Hub packages](https://hub.getdbt.com/). This reduces the need for multiple YAML files to manage dependencies.

 When to use Package dependencies

Package dependencies allow you to add source code from someone else's dbt project into your own, like a library:

* If you only use packages like those from the [dbt Hub](https://hub.getdbt.com/), remain with `packages.yml`.
* Use `packages.yml` when you want to download dbt packages, such as dbt projects, into your root or parent dbt project. Something to note is that it doesn't contribute to the dbt Mesh workflow.
* Use `packages.yml` to include packages in your project's dependencies. This includes both public packages, such as those from the [dbt Hub](https://hub.getdbt.com/), and private packages. dbt now supports [native private packages](../../build/packages.md#native-private-packages).
* [`packages.yml` supports Jinja rendering](../../build/dbt-tips.md#yaml-tips) for historical reasons, allowing dynamic configurations. This can be useful if you need to insert values, like a [Git token method](../../build/packages.md#git-token-method) from an environment variable, into your package specifications. dbt v2 also supports Jinja in `dependencies.yml`; refer to [Jinja support by file type](../../../reference/jinja-file-support.md).

Previously, to use private Git repositories in dbt, you needed to use a workaround that involved embedding a Git token with Jinja. This is not ideal as it requires extra steps like creating a user and sharing a Git token. We’ve introduced support for [native private packages](../../build/packages.md#native-private-packages-) to address this.

## Define project dependencies

If your dbt project relies on models from another project, you can define that relationship using project dependencies. The following steps walk you through specifying project dependencies in dbt:

1. Create a file called `dependencies.yml` at the root of your dbt project.
2. In the `dependencies.yml`, list the upstream dbt project your project depends on as they appear in the `dbt_projects.yml` file.
3. (Optional) Define the specific models you expect from that upstream project to make the dependency explicit.
4. Use [`ref()`](../../../reference/dbt-jinja-functions/ref.md) with the project name to reference upstream models in your SQL.
5. Commit the changes and ensure the dependency is configured in dbt.
6. dbt resolves the dependency using public model metadata from the selected upstream environment and surfaces cross-project lineage in the lineage and DAG (Directed Acyclic Graph) views. Run upstream deployment jobs separately, or configure [job chaining](../../deploy/deploy-jobs.md#trigger-on-job-completion) if you need coordinated execution.

### Example

As an example, let's say you work on the Marketing team at the Jaffle Shop. The name of your team's project is `jaffle_marketing`:

dbt\_project.yml

```yml
name: jaffle_marketing
```

As part of your modeling of marketing data, you need to take a dependency on two other projects:

* `dbt_utils` as a package: A collection of utility macros you can use while writing the SQL for your own models. This package is open-source public and maintained by dbt Labs.
* `jaffle_finance` as a project use case: Data models about the Jaffle Shop's revenue. This project is private and maintained by your colleagues on the Finance team. You want to select from some of this project's final models, as a starting point for your own work.

Refer to [Use cases](./project-dependencies.md#use-cases) for information on package and project dependencies.

dependencies.yml

```yml
packages:
  - package: dbt-labs/dbt_utils
    version: 1.1.1

projects:
  - name: jaffle_finance  # case sensitive and matches the 'name' in the 'dbt_project.yml'
```

What's happening here?

The `dbt_utils` package — When you run `dbt deps`, dbt will pull down this package's full contents (100+ macros) as source code and add them to your environment. You can then call any macro from the package, just as you can call macros defined in your own project.

The `jaffle_finance` projects — This is a new scenario. Unlike installing a package, the models in the `jaffle_finance` project will *not* be pulled down as source code and parsed into your project. Instead, dbt provides a metadata service that resolves references to [**public models**](./model-access.md) defined in the `jaffle_finance` project.

### Advantages

When you're building on top of another team's work, resolving the references in this way has several advantages:

* You're using an intentional interface designated by the model's maintainer with `access: public`.
* You're keeping the scope of your project narrow, and avoiding unnecessary resources and complexity. This is faster for you and faster for dbt.
* You don't need to mirror any conditional configuration of the upstream project such as `vars`, environment variables, or `target.name`. You can reference the Finance team's public models in the upstream environment selected for this dependency. Even if the Finance team makes changes like renaming the model, changing the name of its schema, or [bumping its version](./model-versions.md), your `ref` would still resolve successfully.
* You eliminate the risk of accidentally building those models with `dbt run` or `dbt build`. While you can select those models, you can't actually build them. This prevents unexpected warehouse costs and permissions issues. This also ensures proper ownership and cost allocation for each team's models.

### How to write cross-project ref

**Writing `ref`:** Models referenced from a `project`-type dependency must use [two-argument `ref`](../../../reference/dbt-jinja-functions/ref.md#ref-project-specific-models), including the project name:

models/marts/roi\_by\_channel.sql

```sql
with monthly_revenue as (
  
    select * from {{ ref('jaffle_finance', 'monthly_revenue') }}

),

...
```

(Applies to dbt v1.12 and later)

Cross-project refs unsupported in latest SL YAML spec

When using [dbt Mesh](../../../best-practices/how-we-mesh/mesh-1-intro.md) with the [Semantic Layer](../../use-dbt-semantic-layer/dbt-sl.md), [referencing models from different projects](../../../reference/dbt-jinja-functions/ref.md#ref-project-specific-models) is only supported in the legacy YAML spec, where semantic models are defined as top-level resources and can reference models across projects.

In the [latest YAML spec](../../build/latest-metrics-spec.md), semantic models are defined within model YAML files, and cross-project references are not yet supported. Support for this capability in the latest spec is planned for a future release.

#### Cycle detection

You can enable bidirectional dependencies across projects so these relationships can go in either direction, meaning that the `jaffle_finance` project can add a new model that depends on any public models produced by the `jaffle_marketing` project, so long as the new dependency doesn't introduce any node-level cycles. dbt checks for cycles across projects and raises errors if any are detected.

When setting up projects that depend on each other, it's important to do so in a stepwise fashion. Each project must run and produce public models before the original producer project can take a dependency on the original consumer project. For example, the order of operations would be as follows for a simple two-project setup:

1. The `project_a` project runs in a deployment environment and produces public models.
2. The `project_b` project adds `project_a` as a dependency.
3. The `project_b` project runs in a deployment environment and produces public models.
4. The `project_a` project adds `project_b` as a dependency.

For more guidance on how to use Mesh, refer to the dedicated [Mesh guide](../../../best-practices/how-we-mesh/mesh-1-intro.md) and also our freely available [Mesh learning course](https://learn.getdbt.com/courses/dbt-mesh).

### Safeguarding production data with staging environments

To protect production data, set up a [Staging deployment environment](../../deploy/deploy-environments.md#staging-environment) within your projects.

When the upstream project has a Staging environment, cross-project `ref`s from downstream Development and Staging environments resolve to the upstream Staging environment. When the upstream project has no Staging environment, they resolve to the upstream Production environment. By default, references from downstream Production environments resolve to the upstream Production environment. This ensures consistency between environments and adds a layer of security by preventing access to production data during development workflows.

Read [Why use a staging environment](../../deploy/deploy-environments.md#why-use-a-staging-environment) for more information about the benefits. To override this automatic selection, refer to [Route environments across projects](#route-environments-across-projects).

#### Staging with downstream dependencies

dbt begins using the Staging environment to resolve cross-project references from downstream projects as soon as it exists in a project without "fail-over" to Production. This means that dbt will consistently use metadata from the Staging environment to resolve references in downstream projects, even if there haven't been any successful runs in the configured Staging environment.

To avoid causing downtime for downstream developers, you should define and trigger a job before marking the environment as Staging:

1. Create a new environment, but do NOT mark it as **Staging**.
2. Define a job in that environment.
3. Trigger the job to run, and ensure it completes successfully.
4. Update the environment to mark it as **Staging**.

### Route environments across projects

By default, dbt picks the upstream environment for a cross-project `ref` using the production and staging rules detailed in the previous section. To customize which upstream environment is used, edit the `dependencies.yml` file in the *downstream* project and add the `mesh_environment_routing` field under each upstream project you want to route. You don't change anything in the upstream project. The `ref` in your model code remains unchanged.

Availability

Environment routing is available on the dbt platform dbt v1 Latest and Compatible, and on dbt v2 Stable and Nightly [release tracks](../../dbt-versions/dbt-release-tracks.md). It ships to the Extended tracks in November 2026.

#### How routing works

You set routes per upstream project in the downstream `dependencies.yml`, so one downstream run can point at different environments for different projects. The following table compares the upstream environment dbt uses for each downstream environment:

| Downstream environment | Default without routing                              | With routing                                                                  |
| ---------------------- | ---------------------------------------------------- | ----------------------------------------------------------------------------- |
| Production             | Upstream production                                  | Upstream environment you choose                                               |
| Staging                | Upstream staging if configured; otherwise production | Upstream environment you choose, such as production                           |
| General                | Upstream staging if configured; otherwise production | Upstream environment you choose, such as a specific general environment by ID |
| Development            | Upstream staging if configured; otherwise production | Upstream environment you choose                                               |

If a downstream environment doesn't match a route, dbt uses that project's `default_upstream_environment` (if you set one). With no matching route and no default, dbt keeps the default behavior for that project. You don't need to route every environment.

#### Configure routes

In the following example:

* `orders_project` sends staging runs to its production environment, sends development runs to whichever environment the `DBT_ROUTE_TO_ID` environment variable names, and sends the downstream general environment to a specific upstream general environment. Any other environment uses production because of the default.
* `finance_project` has no default, so downstream environments other than `12001` keep the automatic behavior.

dependencies.yml

```yml
projects:
  - name: orders_project
    default_upstream_environment: production
    mesh_environment_routing:
      - in_this_project_environment: staging
        use_upstream_environment: production
      - in_this_project_environment: development
        use_upstream_environment: DBT_ROUTE_TO_ID
      - in_this_project_environment: 12001  # Downstream general environment ID
        use_upstream_environment: 34002      # orders_project general environment ID
  - name: finance_project
    mesh_environment_routing:
      - in_this_project_environment: 12001
        use_upstream_environment: production
```

The following table describes each key:

| Key                            | Description                                                                                                | Valid values                                                                                          |
| ------------------------------ | ---------------------------------------------------------------------------------------------------------- | ----------------------------------------------------------------------------------------------------- |
| `name`                         | The upstream project. Must match the `name` in its `dbt_project.yml`.                                      | An existing project name (case-sensitive).                                                            |
| `mesh_environment_routing`     | Ordered, explicit routes for this upstream project. Optional.                                              | A YAML list of route pairs.                                                                           |
| `in_this_project_environment`  | The downstream environment where dbt runs.                                                                 | A numeric environment ID; `production`, `staging`, or `development`; or an environment variable name. |
| `use_upstream_environment`     | The environment in the upstream project to resolve the `ref` against.                                      | A numeric *deployment* environment ID; `production` or `staging`; or an environment variable name.    |
| `default_upstream_environment` | The upstream environment to use when no route matches. Set it beside `mesh_environment_routing`. Optional. | Same values as `use_upstream_environment`.                                                            |

Keep the following rules in mind when configuring routes:

* The first matching route in a project entry takes precedence. Avoid duplicate selectors.
* A matching route takes precedence over the project's `default_upstream_environment`.
* To target a general or other custom deployment environment, use its environment ID. `general` isn't a keyword.
* Write the bare variable name (`DBT_ROUTE_TO_ID`), not Jinja delimiters such as `{{ env_var('DBT_ROUTE_TO_ID') }}`.
* Define `DBT_ROUTE_TO_ID` in the downstream run or development environment.
* Set its value to `production`, `staging`, or the ID of a deployment environment in `orders_project`.

#### Validate routes

Routing changes which upstream environment resolves the `ref`. It doesn't copy data or grant access. Before you rely on a route, check the following:

* The upstream model is [public](./model-access.md).
* The selected upstream environment has public model metadata from a successful job.
* Warehouse permissions allow the downstream query.

Run `dbt compile --select YOUR_DOWNSTREAM_MODEL` and check the database and schema in the compiled SQL or `target/manifest.json`. Then run `dbt build --select YOUR_DOWNSTREAM_MODEL` to validate warehouse access.

Routing isn't deferral

Mesh routing chooses an environment in *another project* for a cross-project `ref`. The Studio IDE **Defer** menu and [`--defer`](../../../reference/node-selection/defer.md) use state to resolve unbuilt or unselected nodes in the *current project*. You can use both in the same development workflow.

### Comparison

If you were to instead install the `jaffle_finance` project as a `package` dependency, you would instead be pulling down its full source code and adding it to your runtime environment. This means:

* dbt needs to parse and resolve more inputs (which is slower)
* dbt expects you to configure these models as if they were your own (with `vars`, env vars, etc)
* dbt will run these models as your own unless you explicitly `--exclude` them
* You could be using the project's models in a way that their maintainer (the Finance team) hasn't intended

There are a few cases where installing another internal project as a package can be a useful pattern:

* Unified deployments — In a production environment, if the central data platform team of Jaffle Shop wanted to schedule the deployment of models across both `jaffle_finance` and `jaffle_marketing`, they could use dbt's [selection syntax](../../../reference/node-selection/syntax.md) to create a new "passthrough" project that installed both projects as packages.
* Coordinated changes — In development, if you wanted to test the effects of a change to a public model in an upstream project (`jaffle_finance.monthly_revenue`) on a downstream model (`jaffle_marketing.roi_by_channel`) *before* introducing changes to a staging or production environment, you can install the `jaffle_finance` package as a package within `jaffle_marketing`. The installation can point to a specific git branch, however, if you find yourself frequently needing to perform end-to-end testing across both projects, we recommend you re-examine if this represents a stable interface boundary.

These are the exceptions, rather than the rule. Installing another team's project as a package adds complexity, latency, and risk of unnecessary costs. By defining clear interface boundaries across teams, by serving one team's public models as "APIs" to another, and by enabling practitioners to develop with a more narrowly defined scope, we can enable more people to contribute, with more confidence, while requiring less context upfront.

## FAQs

Can I define private packages in the dependencies.yml file?

It depends on how you're accessing your private packages:

* If you're using [native private packages](../../build/packages.md#native-private-packages), you can define them in the `dependencies.yml` file.
* If you're using the [git token method](../../build/packages.md#git-token-method), use `packages.yml` on dbt v1 because Jinja isn't supported in `dependencies.yml`. On dbt v2, you can use `dependencies.yml`, but we recommend using `packages.yml` if your project must also run on dbt v1. Refer to [Jinja support by file type](../../../reference/jinja-file-support.md).

Why doesn’t an indirectly referenced upstream public model appear in Explorer?

For [project dependencies](./project-dependencies.md) in Mesh, [Catalog](../../explore/explore-multiple-projects.md) only displays directly referenced [public models](./model-access.md) from upstream projects, even if an upstream model indirectly depends on another public model.

So for example, if:

* `project_b` adds `project_a` as a dependency
* `project_b`'s model `downstream_c` references `project_a.upstream_b`
* `project_a.upstream_b` references another public model, `project_a.upstream_a`

Then:

* In Explorer, only directly referenced public models (`upstream_b` in this case) appear.
* In the [Studio IDE](../../platform/studio-ide/develop-in-studio.md) lineage view, however, `upstream_a` (the indirect dependency) *will* appear because dbt dynamically resolves the full dependency graph.

This behavior makes sure that Catalog only shows the immediate dependencies available to that specific project.

## Related docs

* Refer to the [Mesh](../../../best-practices/how-we-mesh/mesh-1-intro.md) guide for more guidance on how to use Mesh.
* [Quickstart with Mesh](../../../guides/mesh-qs.md)
* [Hybrid development with dbt platform and dbt v2](../../../guides/dbt-platform-local-workflow.md) — manage credentials, env vars, deferral, and dbt v2 versions when developing locally against a Mesh project
