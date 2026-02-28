# Build and view your docs with dbt

dbt enables you to generate documentation for your project and data platform. The documentation is automatically updated with new information after a fully successful job run, ensuring accuracy and relevance.

The default documentation experience in dbt is [Catalog](https://docs.getdbt.com/docs/explore/explore-projects.md), available on [Starter, Enterprise, or Enterprise+ plans](https://www.getdbt.com/pricing/). Use [Catalog](https://docs.getdbt.com/docs/explore/explore-projects.md) to view your project's resources (such as models, tests, and metrics) and their lineage to gain a better understanding of its latest production state.

Refer to [documentation](https://docs.getdbt.com/docs/build/documentation.md) for more configuration details.

This shift makes [dbt Docs](#dbt-docs) a legacy documentation feature in dbt. dbt Docs is still accessible and offers basic documentation, but it doesn't offer the same speed, metadata, or visibility as Catalog. dbt Docs is available to dbt developer plans or dbt Core users.

## Set up a documentation job[​](#set-up-a-documentation-job "Direct link to Set up a documentation job")

Catalog uses the [metadata](https://docs.getdbt.com/docs/explore/explore-projects.md#generate-metadata) generated after each job run in the production or staging environment, ensuring it always has the latest project results. To view richer metadata, you can set up documentation for a job in dbt when you edit your job settings or create a new job.

Configure the job to [generate metadata](https://docs.getdbt.com/docs/explore/explore-projects.md#generate-metadata) when it runs. If you want to view column and statistics for models, sources, and snapshots in Catalog, then this step is necessary.

To set up a job to generate docs:

1. In the top left, click **Deploy** and select **Jobs**.
2. Create a new job or select an existing job and click **Settings**.
3. Under **Execution Settings**, select **Generate docs on run** and click **Save**.
   <!-- -->
   [![Setting up a job to generate documentation](/img/docs/dbt-cloud/using-dbt-cloud/documentation-job-execution-settings.png?v=2 "Setting up a job to generate documentation")](#)Setting up a job to generate documentation

*Note, for dbt Docs users you need to configure the job to generate docs when it runs, then manually link that job to your project. Proceed to [configure project documentation](#configure-project-documentation) so your project generates the documentation when this job runs.*

You can also add the [`dbt docs generate` command](https://docs.getdbt.com/reference/commands/cmd-docs.md) to the list of commands in the job run steps. However, you can expect different outcomes when adding the command to the run steps compared to configuring a job selecting the **Generate docs on run** checkbox.

Review the following options and outcomes:

| Options               | Outcomes                                                                                                                                                                                                                                |
| --------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **Select checkbox**   | Select the **Generate docs on run** checkbox to automatically generate updated project docs each time your job runs. If that particular step in your job fails, the job can still be successful if all subsequent steps are successful. |
| **Add as a run step** | Add `dbt docs generate` to the list of commands in the job run steps, in whatever order you prefer. If that particular step in your job fails, the job will fail and all subsequent steps will be skipped.                              |

Search table...

|                  |   |   |   |   |
| ---------------- | - | - | - | - |
| Loading table... |   |   |   |   |

Tip — Documentation-only jobs

To create and schedule documentation-only jobs at the end of your production jobs, add the `dbt compile` command in the **Commands** section.

## dbt Docs[​](#dbt-docs "Direct link to dbt Docs")

dbt Docs, available on developer plans or dbt Core users, generates a website from your dbt project using the `dbt docs generate` command. It provides a central location to view your project's resources, such as models, tests, and lineage — and helps you understand the data in your warehouse.

### Configure project documentation[​](#configure-project-documentation "Direct link to Configure project documentation")

You configure project documentation to generate documentation when the job you set up in the previous section runs. In the project settings, specify the job that generates documentation artifacts for that project. Once you configure this setting, subsequent runs of the job will automatically include a step to generate documentation.

1. From dbt, click on your account name in the left side menu and select **Account settings**.
2. Navigate to **Projects** and select the project that needs documentation.
3. Click **Edit**.
4. Under **Artifacts**, select the job that should generate docs when it runs and click **Save**.
   <!-- -->
   [![Configuring project documentation](/img/docs/dbt-cloud/using-dbt-cloud/documentation-project-details.png?v=2 "Configuring project documentation")](#)Configuring project documentation

Use Catalog for a richer documentation experience

For a richer and more interactive experience, try out [Catalog](https://docs.getdbt.com/docs/explore/explore-projects.md), available on [Starter, Enterprise, or Enterprise+ plans](https://www.getdbt.com/pricing/). It includes map layers of your DAG, keyword search, interacts with the Studio IDE, model performance, project recommendations, and more.

### Generating documentation[​](#generating-documentation "Direct link to Generating documentation")

To generate documentation in the Studio IDE, run the `dbt docs generate` command in the **Command Bar** in the Studio IDE. This command will generate the documentation for your dbt project as it exists in development in your IDE session.

After running `dbt docs generate` in the Studio IDE, click the icon above the file tree, to see the latest version of your documentation rendered in a new browser window.

### View documentation[​](#view-documentation "Direct link to View documentation")

Once you set up a job to generate documentation for your project, you can click **Catalog** in the navigation and then click on **dbt Docs**. Your project's documentation should open. This link will always help you find the most recent version of your project's documentation in dbt.

These generated docs always show the last fully successful run, which means that if you have any failed tasks, including tests, then you will not see changes to the docs by this run. If you don't see a fully successful run, then you won't see any changes to the documentation.

The Studio IDE makes it possible to view [documentation](https://docs.getdbt.com/docs/build/documentation.md) for your dbt project while your code is still in development. With this workflow, you can inspect and verify what your project's generated documentation will look like before your changes are released to production.

## Related docs[​](#related-docs "Direct link to Related docs")

* [Documentation](https://docs.getdbt.com/docs/build/documentation.md)
* [Catalog](https://docs.getdbt.com/docs/explore/explore-projects.md)

## Was this page helpful?

YesNo

[Privacy policy](https://www.getdbt.com/cloud/privacy-policy)[Create a GitHub issue](https://github.com/dbt-labs/docs.getdbt.com/issues)

This site is protected by reCAPTCHA and the Google [Privacy Policy](https://policies.google.com/privacy) and [Terms of Service](https://policies.google.com/terms) apply.
