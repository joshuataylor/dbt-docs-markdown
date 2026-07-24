# About Canvas

dbt platform | Enterprise, Enterprise+ⓘ

Canvas helps you quickly access and transform data through a visual, drag-and-drop experience.

Canvas allows organizations to enjoy the many benefits of code-driven development, such as increased precision, ease of debugging, and ease of validation, while retaining the flexibility to have different contributors develop wherever they are most comfortable. For natural language and AI-assisted workflows, use [dbt Copilot in Canvas](https://docs.getdbt.com/docs/platform/build-canvas-copilot.md).

These models compile directly to SQL and are indistinguishable from other dbt models in your projects:

* Visual models are version-controlled in your backing Git provider.
* All models are accessible across projects in [Mesh](https://docs.getdbt.com/best-practices/how-we-mesh/mesh-1-intro.md).
* Models can be materialized into production through [dbt orchestration](https://docs.getdbt.com/docs/deploy/deployments.md), or be built directly into a user's development schema.
* Integrate with [Catalog](https://docs.getdbt.com/docs/explore/explore-projects.md) and the [Studio IDE](https://docs.getdbt.com/docs/platform/studio-ide/develop-in-studio.md).

[![Create or edit dbt models with Canvas, enabling everyone to develop with dbt through a drag-and-drop experience inside of dbt.](/img/docs/dbt-platform/canvas/canvas.png?v=2 "Create or edit dbt models with Canvas, enabling everyone to develop with dbt through a drag-and-drop experience inside of dbt.")](#)Create or edit dbt models with Canvas, enabling everyone to develop with dbt through a drag-and-drop experience inside of dbt.

## Canvas prerequisites[​](#canvas-prerequisites "Direct link to Canvas prerequisites")

Before using Canvas, you should:

* Have a [dbt Enterprise or Enterprise+](https://www.getdbt.com/pricing) account.

* Have a [developer license](https://docs.getdbt.com/docs/platform/manage-access/seats-and-users.md) with user credentials set up.

* Be using one of the following adapters:

  <!-- -->

  * Bigquery
  * Databricks
  * Redshift
  * Snowflake
  * Trino
  * You can access the Canvas with adapters not listed, but some features may be missing at this time.

* Use [GitHub](https://docs.getdbt.com/docs/platform/git/connect-github.md), [GitLab](https://docs.getdbt.com/docs/platform/git/connect-gitlab.md), or [Azure DevOps](https://docs.getdbt.com/docs/platform/git/connect-azure-devops.md) as your Git provider, connected to dbt via HTTPS.

  <!-- -->

  * SSH connections aren't supported at this time.
  * Self-hosted or on-premises deployments of any Git provider aren't supported for Canvas at this time.

* Have an existing dbt project already created with a Staging or Production run completed.

* Verify your Development environment is on a supported [release track](https://docs.getdbt.com/docs/dbt-versions/dbt-release-tracks.md) to receive ongoing updates.

* Have read-only access to the [Staging environment](https://docs.getdbt.com/docs/deploy/deploy-environments.md#staging-environment) with the data to be able to execute `run` in the Canvas. To customize the required access for the Canvas user group, refer to [Set up environment-level permissions](https://docs.getdbt.com/docs/platform/manage-access/environment-permissions-setup.md) for more information.

<!-- -->

info

dbt Wizard is the new and recommended AI agent for governed data development in dbt. It handles the full development lifecycle — investigation, building, validation, and shipping — grounded in your dbt project's lineage, tests, contracts, and metric definitions.

dbt Copilot is separate from dbt Wizard and is dbt's inline AI assistance experience, providing single-click generation of SQL, documentation, tests, and semantic models in Studio IDE, Canvas, and Insights.

Refer to [dbt AI FAQs](https://docs.getdbt.com/docs/dbt-ai/dbt-ai-faqs.md#is-dbt-wizard-the-same-as-dbt-copilot), [Billing](https://docs.getdbt.com/docs/platform/billing.md), and [dbt's Terms of Use](https://www.getdbt.com/terms-of-use) for more information.

## Feedback[​](#feedback "Direct link to Feedback")

Please note, always review AI-generated code and content as it may produce incorrect results.

To give feedback, please reach out to your dbt Labs account team. We appreciate your feedback and suggestions as we improve Canvas.

## Resources[​](#resources "Direct link to Resources")

Learn more about Canvas:

* How to [use Canvas](https://docs.getdbt.com/docs/platform/use-canvas.md)
* The Canvas [quickstart guide](https://docs.getdbt.com/guides/canvas.md)
* [Canvas fundamentals course](https://learn.getdbt.com/learn/course/canvas-fundamentals) on [dbt Learn](https://learn.getdbt.com/catalog)
