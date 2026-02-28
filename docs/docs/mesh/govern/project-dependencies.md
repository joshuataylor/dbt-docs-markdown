# Project dependencies [Enterprise](https://www.getdbt.com/pricing "Go to https://www.getdbt.com/pricing")[Enterprise +](https://www.getdbt.com/pricing "Go to https://www.getdbt.com/pricing")

Available on dbt [Enterprise or Enterprise+](https://www.getdbt.com/pricing) plans.

For a long time, dbt has supported code reuse and extension by installing other projects as [packages](https://docs.getdbt.com/docs/build/packages.md). When you install another project as a package, you are pulling in its full source code, and adding it to your own. This enables you to call macros and run models defined in that other project.

While this is a great way to reuse code, share utility macros, and establish a starting point for common transformations, it's not a great way to enable collaboration across teams and at scale, especially in larger organizations.

dbt Labs supports an expanded notion of `dependencies` across multiple dbt projects:

* **Packages** — Familiar and pre-existing type of dependency. You take this dependency by installing the package's full source code (like a software library).
* **Projects** — The dbt method to take a dependency on another project. Using a metadata service that runs behind the scenes, dbt resolves references on-the-fly to public models defined in other projects. You don't need to parse or run those upstream models yourself. Instead, you treat your dependency on those models as an API that returns a dataset. The maintainer of the public model is responsible for guaranteeing its quality and stability.

## Prerequisites[​](#prerequisites "Direct link to Prerequisites")

* Available in [dbt Enterprise or Enterprise+](https://www.getdbt.com/pricing). To use it, designate a [public model](https://docs.getdbt.com/docs/mesh/govern/model-access.md) and add a [cross-project ref](#how-to-write-cross-project-ref).

* For the upstream ("producer") project setup:

  <!-- -->

  * Configure models in upstream project with [`access: public`](https://docs.getdbt.com/reference/resource-configs/access.md) and have at least one successful job run after defining `access`.
  * Define a [Production deployment environment](https://docs.getdbt.com/docs/deploy/deploy-environments.md#set-as-production-environment) in the upstream project and make sure at least *one deployment job* has run successfully there. This job should generate a [`manifest.json` file](https://docs.getdbt.com/reference/artifacts/manifest-json.md) — it includes the metadata needed for downstream projects.
  * If the upstream project has a Staging environment, run at least one successful deployment job there to ensure downstream cross-project references resolve correctly.

* Each project `name` must be unique in your dbt account. For example, if you have a dbt project (codebase) for the `jaffle_marketing` team, avoid creating projects for `Jaffle Marketing - Dev` and `Jaffle Marketing - Prod`; use [environment-level isolation](https://docs.getdbt.com/docs/dbt-cloud-environments.md#types-of-environments) instead.
  <!-- -->
  * dbt supports [Connections](https://docs.getdbt.com/docs/cloud/connect-data-platform/about-connections.md#connection-management), available to all dbt users. Connections allows different data platform connections per environment, eliminating the need to duplicate projects. Projects can use multiple connections of the same warehouse type. Connections are reusable across projects and environments.

* The `dbt_project.yml` file is case-sensitive, which means the project name must exactly match the name in your `dependencies.yml`. For example, `jaffle_marketing`, not `JAFFLE_MARKETING`.

<!-- -->

## Use cases[​](#use-cases "Direct link to Use cases")

The following setup will work for every dbt project:

* Add [any package dependencies](https://docs.getdbt.com/docs/mesh/govern/project-dependencies.md#when-to-use-project-dependencies) to `packages.yml`
* Add [any project dependencies](https://docs.getdbt.com/docs/mesh/govern/project-dependencies.md#when-to-use-package-dependencies) to `dependencies.yml`

However, you may be able to consolidate both into a single `dependencies.yml` file. Read the following section to learn more.

#### About packages.yml and dependencies.yml[​](#about-packagesyml-and-dependenciesyml "Direct link to About packages.yml and dependencies.yml")

The `dependencies.yml`. file can contain both types of dependencies: "package" and "project" dependencies.

* [Package dependencies](https://docs.getdbt.com/docs/build/packages.md#how-do-i-add-a-package-to-my-project) lets you add source code from someone else's dbt project into your own, like a library.
* Project dependencies provide a different way to build on top of someone else's work in dbt.
* Private packages are not supported in `dependencies.yml` because they intentionally don't support Jinja rendering or conditional configuration. This is to maintain static and predictable configuration and ensures compatibility with other services, like dbt.

If your dbt project doesn't require the use of Jinja within the package specifications, you can simply rename your existing `packages.yml` to `dependencies.yml`. However, something to note is if your project's package specifications use Jinja, particularly for scenarios like adding an environment variable or a [Git token method](https://docs.getdbt.com/docs/build/packages.md#git-token-method) in a private Git package specification, you should continue using the `packages.yml` file name.

Use the following toggles to understand the differences and determine when to use `dependencies.yml` or `packages.yml` (or both). Refer to the [FAQs](#faqs) for more info.

 When to use Project dependencies

Project dependencies are designed for the [dbt Mesh](https://docs.getdbt.com/best-practices/how-we-mesh/mesh-1-intro.md) and [cross-project reference](https://docs.getdbt.com/docs/mesh/govern/project-dependencies.md#how-to-write-cross-project-ref) workflow:

* Use `dependencies.yml` when you need to set up cross-project references between different dbt projects, especially in a dbt Mesh setup.
* Use `dependencies.yml` when you want to include both projects and non-private dbt packages in your project's dependencies.
* Use `dependencies.yml` for organization and maintainability if you're using both [cross-project refs](https://docs.getdbt.com/docs/mesh/govern/project-dependencies.md#how-to-write-cross-project-ref) and [dbt Hub packages](https://hub.getdbt.com/). This reduces the need for multiple YAML files to manage dependencies.

 When to use Package dependencies

Package dependencies allow you to add source code from someone else's dbt project into your own, like a library:

* If you only use packages like those from the [dbt Hub](https://hub.getdbt.com/), remain with `packages.yml`.
* Use `packages.yml` when you want to download dbt packages, such as dbt projects, into your root or parent dbt project. Something to note is that it doesn't contribute to the dbt Mesh workflow.
* Use `packages.yml` to include packages in your project's dependencies. This includes both public packages, such as those from the [dbt Hub](https://hub.getdbt.com/), and private packages. dbt now supports [native private packages](https://docs.getdbt.com/docs/build/packages.md#native-private-packages).
* [`packages.yml` supports Jinja rendering](https://docs.getdbt.com/docs/build/dbt-tips.md#yaml-tips) for historical reasons, allowing dynamic configurations. This can be useful if you need to insert values, like a [Git token method](https://docs.getdbt.com/docs/build/packages.md#git-token-method) from an environment variable, into your package specifications.

Previously, to use private Git repositories in dbt, you needed to use a workaround that involved embedding a Git token with Jinja. This is not ideal as it requires extra steps like creating a user and sharing a Git token. We’ve introduced support for [native private packages](https://docs.getdbt.com/docs/build/packages.md#native-private-packages-) to address this.

## Define project dependencies[​](#define-project-dependencies "Direct link to Define project dependencies")

If your dbt project relies on models from another project, you can define that relationship using project dependencies. The following steps walk you through specifying project dependencies in dbt:

1. Create a file called `dependencies.yml` at the root of your dbt project.
2. In the `dependencies.yml`, list the upstream dbt project your project depends on as they appear in the `dbt_projects.yml` file.
3. (Optional) Define the specific models you expect from that upstream project to make the dependency explicit.
4. Use [`ref()`](https://docs.getdbt.com/reference/dbt-jinja-functions/ref.md) with the project name to reference upstream models in your SQL.
5. Commit the changes and ensure the dependency is configured in dbt.
6. dbt will resolve the dependency, ensure upstream projects are built first, and surface cross-project lineage in the lineage and DAG (Directed Acyclic Graph) views.

### Example[​](#example "Direct link to Example")

As an example, let's say you work on the Marketing team at the Jaffle Shop. The name of your team's project is `jaffle_marketing`:

dbt\_project.yml

```yml
name: jaffle_marketing
```

As part of your modeling of marketing data, you need to take a dependency on two other projects:

* `dbt_utils` as a package: A collection of utility macros you can use while writing the SQL for your own models. This package is open-source public and maintained by dbt Labs.
* `jaffle_finance` as a project use case: Data models about the Jaffle Shop's revenue. This project is private and maintained by your colleagues on the Finance team. You want to select from some of this project's final models, as a starting point for your own work.

Refer to [Use cases](https://docs.getdbt.com/docs/mesh/govern/project-dependencies.md#use-cases) for information on package and project dependencies.

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

The `jaffle_finance` projects — This is a new scenario. Unlike installing a package, the models in the `jaffle_finance` project will *not* be pulled down as source code and parsed into your project. Instead, dbt provides a metadata service that resolves references to [**public models**](https://docs.getdbt.com/docs/mesh/govern/model-access.md) defined in the `jaffle_finance` project.

### Advantages[​](#advantages "Direct link to Advantages")

When you're building on top of another team's work, resolving the references in this way has several advantages:

* You're using an intentional interface designated by the model's maintainer with `access: public`.
* You're keeping the scope of your project narrow, and avoiding unnecessary resources and complexity. This is faster for you and faster for dbt.
* You don't need to mirror any conditional configuration of the upstream project such as `vars`, environment variables, or `target.name`. You can reference them directly wherever the Finance team is building their models in production. Even if the Finance team makes changes like renaming the model, changing the name of its schema, or [bumping its version](https://docs.getdbt.com/docs/mesh/govern/model-versions.md), your `ref` would still resolve successfully.
* You eliminate the risk of accidentally building those models with `dbt run` or `dbt build`. While you can select those models, you can't actually build them. This prevents unexpected warehouse costs and permissions issues. This also ensures proper ownership and cost allocation for each team's models.

### How to write cross-project ref[​](#how-to-write-cross-project-ref "Direct link to How to write cross-project ref")

**Writing `ref`:** Models referenced from a `project`-type dependency must use [two-argument `ref`](https://docs.getdbt.com/reference/dbt-jinja-functions/ref.md#ref-project-specific-models), including the project name:

models/marts/roi\_by\_channel.sql

```sql
with monthly_revenue as (
  
    select * from {{ ref('jaffle_finance', 'monthly_revenue') }}

),

...
```

#### Cycle detection[​](#cycle-detection "Direct link to Cycle detection")

<!-- -->

You can enable bidirectional dependencies across projects so these relationships can go in either direction, meaning that the `jaffle_finance` project can add a new model that depends on any public models produced by the `jaffle_marketing` project, so long as the new dependency doesn't introduce any node-level cycles. dbt checks for cycles across projects and raises errors if any are detected.

When setting up projects that depend on each other, it's important to do so in a stepwise fashion. Each project must run and produce public models before the original producer project can take a dependency on the original consumer project. For example, the order of operations would be as follows for a simple two-project setup:

1. The `project_a` project runs in a deployment environment and produces public models.
2. The `project_b` project adds `project_a` as a dependency.
3. The `project_b` project runs in a deployment environment and produces public models.
4. The `project_a` project adds `project_b` as a dependency.

For more guidance on how to use Mesh, refer to the dedicated [Mesh guide](https://docs.getdbt.com/best-practices/how-we-mesh/mesh-1-intro.md) and also our freely available [Mesh learning course](https://learn.getdbt.com/courses/dbt-mesh).

### Safeguarding production data with staging environments[​](#safeguarding-production-data-with-staging-environments "Direct link to Safeguarding production data with staging environments")

When working in a Development environment, cross-project `ref`s normally resolve to the Production environment of the project. However, to protect production data, set up a [Staging deployment environment](https://docs.getdbt.com/docs/deploy/deploy-environments.md#staging-environment) within your projects.

With a staging environment integrated into the project, Mesh automatically fetches public model information from the producer’s staging environment if the consumer is also in staging. Similarly, Mesh fetches from the producer’s production environment if the consumer is in production. This ensures consistency between environments and adds a layer of security by preventing access to production data during development workflows.

Read [Why use a staging environment](https://docs.getdbt.com/docs/deploy/deploy-environments.md#why-use-a-staging-environment) for more information about the benefits.

#### Staging with downstream dependencies[​](#staging-with-downstream-dependencies "Direct link to Staging with downstream dependencies")

dbt begins using the Staging environment to resolve cross-project references from downstream projects as soon as it exists in a project without "fail-over" to Production. This means that dbt will consistently use metadata from the Staging environment to resolve references in downstream projects, even if there haven't been any successful runs in the configured Staging environment.

To avoid causing downtime for downstream developers, you should define and trigger a job before marking the environment as Staging:

1. Create a new environment, but do NOT mark it as **Staging**.
2. Define a job in that environment.
3. Trigger the job to run, and ensure it completes successfully.
4. Update the environment to mark it as **Staging**.

### Comparison[​](#comparison "Direct link to Comparison")

If you were to instead install the `jaffle_finance` project as a `package` dependency, you would instead be pulling down its full source code and adding it to your runtime environment. This means:

* dbt needs to parse and resolve more inputs (which is slower)
* dbt expects you to configure these models as if they were your own (with `vars`, env vars, etc)
* dbt will run these models as your own unless you explicitly `--exclude` them
* You could be using the project's models in a way that their maintainer (the Finance team) hasn't intended

There are a few cases where installing another internal project as a package can be a useful pattern:

* Unified deployments — In a production environment, if the central data platform team of Jaffle Shop wanted to schedule the deployment of models across both `jaffle_finance` and `jaffle_marketing`, they could use dbt's [selection syntax](https://docs.getdbt.com/reference/node-selection/syntax.md) to create a new "passthrough" project that installed both projects as packages.
* Coordinated changes — In development, if you wanted to test the effects of a change to a public model in an upstream project (`jaffle_finance.monthly_revenue`) on a downstream model (`jaffle_marketing.roi_by_channel`) *before* introducing changes to a staging or production environment, you can install the `jaffle_finance` package as a package within `jaffle_marketing`. The installation can point to a specific git branch, however, if you find yourself frequently needing to perform end-to-end testing across both projects, we recommend you re-examine if this represents a stable interface boundary.

These are the exceptions, rather than the rule. Installing another team's project as a package adds complexity, latency, and risk of unnecessary costs. By defining clear interface boundaries across teams, by serving one team's public models as "APIs" to another, and by enabling practitioners to develop with a more narrowly defined scope, we can enable more people to contribute, with more confidence, while requiring less context upfront.

## FAQs[​](#faqs "Direct link to FAQs")

Can I define private packages in the dependencies.yml file?

It depends on how you're accessing your private packages:

* If you're using [native private packages](https://docs.getdbt.com/docs/build/packages.md#native-private-packages), you can define them in the `dependencies.yml` file.
* If you're using the [git token method](https://docs.getdbt.com/docs/build/packages.md#git-token-method), you must define them in the `packages.yml` file instead of the `dependencies.yml` file. This is because conditional rendering (like Jinja-in-yaml) is not supported in `dependencies.yml`.

Why doesn’t an indirectly referenced upstream public model appear in Explorer?

For [project dependencies](https://docs.getdbt.com/docs/mesh/govern/project-dependencies.md) in Mesh, [Catalog](https://docs.getdbt.com/docs/explore/explore-multiple-projects.md) only displays directly referenced [public models](https://docs.getdbt.com/docs/mesh/govern/model-access.md) from upstream projects, even if an upstream model indirectly depends on another public model.

So for example, if:

* `project_b` adds `project_a` as a dependency
* `project_b`'s model `downstream_c` references `project_a.upstream_b`
* `project_a.upstream_b` references another public model, `project_a.upstream_a`

Then:

* In Explorer, only directly referenced public models (`upstream_b` in this case) appear.
* In the [Studio IDE](https://docs.getdbt.com/docs/cloud/studio-ide/develop-in-studio.md) lineage view, however, `upstream_a` (the indirect dependency) *will* appear because dbt dynamically resolves the full dependency graph.

This behavior makes sure that Catalog only shows the immediate dependencies available to that specific project.

## Related docs[​](#related-docs "Direct link to Related docs")

* Refer to the [Mesh](https://docs.getdbt.com/best-practices/how-we-mesh/mesh-1-intro.md) guide for more guidance on how to use Mesh.
* [Quickstart with Mesh](https://docs.getdbt.com/guides/mesh-qs.md)

## Was this page helpful?

YesNo

[Privacy policy](https://www.getdbt.com/cloud/privacy-policy)[Create a GitHub issue](https://github.com/dbt-labs/docs.getdbt.com/issues)

This site is protected by reCAPTCHA and the Google [Privacy Policy](https://policies.google.com/privacy) and [Terms of Service](https://policies.google.com/terms) apply.
