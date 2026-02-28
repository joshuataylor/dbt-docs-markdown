# About Canvas [Enterprise](https://www.getdbt.com/pricing "Go to https://www.getdbt.com/pricing")[Enterprise +](https://www.getdbt.com/pricing "Go to https://www.getdbt.com/pricing")

Canvas helps you quickly access and transform data through a visual, drag-and-drop experience and with a built-in AI for custom code generation.

Canvas allows organizations to enjoy the many benefits of code-driven development—such as increased precision, ease of debugging, and ease of validation — while retaining the flexibility to have different contributors develop wherever they are most comfortable. Users can also take advantage of built-in AI for custom code generation, making it an end-to-end frictionless experience.

These models compile directly to SQL and are indistinguishable from other dbt models in your projects:

* Visual models are version-controlled in your backing Git provider.
* All models are accessible across projects in [Mesh](https://docs.getdbt.com/best-practices/how-we-mesh/mesh-1-intro.md).
* Models can be materialized into production through [dbt orchestration](https://docs.getdbt.com/docs/deploy/deployments.md), or be built directly into a user's development schema.
* Integrate with [Catalog](https://docs.getdbt.com/docs/explore/explore-projects.md) and the [Studio IDE](https://docs.getdbt.com/docs/cloud/studio-ide/develop-in-studio.md).

[![Create or edit dbt models with Canvas, enabling everyone to develop with dbt through a drag-and-drop experience inside of dbt.](/img/docs/dbt-cloud/canvas/canvas.png?v=2 "Create or edit dbt models with Canvas, enabling everyone to develop with dbt through a drag-and-drop experience inside of dbt.")](#)Create or edit dbt models with Canvas, enabling everyone to develop with dbt through a drag-and-drop experience inside of dbt.

## Canvas prerequisites[​](#canvas-prerequisites "Direct link to Canvas prerequisites")

Before using Canvas, you should:

* Have a [dbt Enterprise or Enterprise+](https://www.getdbt.com/pricing) account.

* Have a [developer license](https://docs.getdbt.com/docs/cloud/manage-access/seats-and-users.md) with developer credentials set up.

* Be using one of the following adapters:

  <!-- -->

  * Bigquery
  * Databricks
  * Redshift
  * Snowflake
  * Trino
  * You can access the Canvas with adapters not listed, but some features may be missing at this time.

* Use [GitHub](https://docs.getdbt.com/docs/cloud/git/connect-github.md), [GitLab](https://docs.getdbt.com/docs/cloud/git/connect-gitlab.md), or [Azure DevOps](https://docs.getdbt.com/docs/cloud/git/connect-azure-devops.md) as your Git provider, connected to dbt via HTTPS.

  <!-- -->

  * SSH connections aren't supported at this time.
  * Git integration for GitHub self-hosted / on-premises deployments aren't supported at this time.

* Have an existing dbt project already created with a Staging or Production run completed.

* Verify your Development environment is on a supported [release track](https://docs.getdbt.com/docs/dbt-versions/cloud-release-tracks.md) to receive ongoing updates.

* Have read-only access to the [Staging environment](https://docs.getdbt.com/docs/deploy/deploy-environments.md#staging-environment) with the data to be able to execute `run` in the Canvas. To customize the required access for the Canvas user group, refer to [Set up environment-level permissions](https://docs.getdbt.com/docs/cloud/manage-access/environment-permissions-setup.md) for more information.

* Have the AI-powered features toggle enabled (for [Copilot integration](https://docs.getdbt.com/docs/cloud/dbt-copilot.md)).

## Feedback[​](#feedback "Direct link to Feedback")

Please note, always review AI-generated code and content as it may produce incorrect results.

To give feedback, please reach out to your dbt Labs account team. We appreciate your feedback and suggestions as we improve Canvas.

## Resources[​](#resources "Direct link to Resources")

Learn more about Canvas:

* How to [use Canvas](https://docs.getdbt.com/docs/cloud/use-canvas.md)
* The Canvas [quickstart guide](https://docs.getdbt.com/guides/canvas.md)
* [Canvas fundamentals course](https://learn.getdbt.com/learn/course/canvas-fundamentals) on [dbt Learn](https://learn.getdbt.com/catalog)

## Was this page helpful?

YesNo

[Privacy policy](https://www.getdbt.com/cloud/privacy-policy)[Create a GitHub issue](https://github.com/dbt-labs/docs.getdbt.com/issues)

This site is protected by reCAPTCHA and the Google [Privacy Policy](https://policies.google.com/privacy) and [Terms of Service](https://policies.google.com/terms) apply.
