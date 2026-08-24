# About dbt projects

A dbt project informs dbt about the context of your project and how to transform your data (build your data sets). By design, dbt enforces the top-level structure of a dbt project such as the `dbt_project.yml` file, the `models` directory, the `snapshots` directory, and so on. Within the directories of the top-level, you can organize your project in any way that meets the needs of your organization and data pipeline.

At a minimum, all a project needs is the `dbt_project.yml` project configuration file. dbt supports a number of different resources, so a project may also include:

| Resource                                                                 | Description                                                                                                                                                                                                                                                                       |
| ------------------------------------------------------------------------ | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| [models](./models.md)                   | Each model lives in a single file and contains logic that either transforms raw data into a dataset that is ready for analytics or, more often, is an intermediate step in such a transformation.                                                                                 |
| [snapshots](./snapshots.md)             | A way to capture the state of your mutable tables so you can refer to it later.                                                                                                                                                                                                   |
| [seeds](./seeds.md)                     | CSV files with static data that you can load into your data platform with dbt.                                                                                                                                                                                                    |
| [data tests](./data-tests.md)           | SQL queries that you can write to test the models and resources in your project.                                                                                                                                                                                                  |
| [macros](./jinja-macros.md)             | Blocks of code that you can reuse multiple times.                                                                                                                                                                                                                                 |
| [docs](./documentation.md)              | Docs for your project that you can build.                                                                                                                                                                                                                                         |
| [sources](./sources.md)                 | A way to name and describe the data loaded into your warehouse by your Extract and Load tools.                                                                                                                                                                                    |
| [exposures](./exposures.md)             | A way to define and describe a downstream use of your project.                                                                                                                                                                                                                    |
| [metrics](./build-metrics-intro.md)     | A way for you to define metrics for your project.                                                                                                                                                                                                                                 |
| [groups](./groups.md)                   | Groups enable collaborative node organization in restricted collections.                                                                                                                                                                                                          |
| [analysis](./analyses.md)               | A way to organize analytical SQL queries in your project such as the general ledger from your QuickBooks.                                                                                                                                                                         |
| [semantic models](./semantic-models.md) | Semantic models define the foundational data relationships in [MetricFlow](./about-metricflow.md) and the [Semantic Layer](../use-dbt-semantic-layer/dbt-sl.md), enabling you to query metrics using a semantic graph. |
| [saved queries](./saved-queries.md)     | Saved queries organize reusable queries by grouping metrics, dimensions, and filters into nodes visible in the dbt DAG.                                                                                                                                                           |
| [user-defined functions](./udfs.md)     | User-defined functions (UDFs) let you create reusable custom functions in your warehouse, shareable across dbt, BI tools, data science workflows, and more.                                                                                                                       |

When building out the structure of your project, you should consider these impacts on your organization's workflow:

* **How would people run dbt commands** — Selecting a path
* **How would people navigate within the project** — Whether as developers in the Studio IDE or stakeholders from the docs
* **How would people configure the models** — Some bulk configurations are easier done at the directory level so people don't have to remember to do everything in a config block with each new model

## Project configuration

Every dbt project includes a project configuration file called `dbt_project.yml`. It defines the directory of the dbt project and other project configurations.

Edit `dbt_project.yml` to set up common project configurations such as:

| YAML key                                                                                        | Value description                                                                                                    |
| ----------------------------------------------------------------------------------------------- | -------------------------------------------------------------------------------------------------------------------- |
| [name](../../reference/project-configs/name.md)                               | Your project’s name in [snake case](https://en.wikipedia.org/wiki/Snake_case)                                        |
| [version](../../reference/project-configs/version.md)                         | Version of your project                                                                                              |
| [require-dbt-version](../../reference/project-configs/require-dbt-version.md) | Restrict your project to only work with a range of [dbt Core versions](../dbt-versions.md) |
| [profile](../../reference/project-configs/profile.md)                         | The profile dbt uses to connect to your data platform                                                                |
| [model-paths](../../reference/project-configs/model-paths.md)                 | Directories to where your model and source files live                                                                |
| [seed-paths](../../reference/project-configs/seed-paths.md)                   | Directories to where your seed files live                                                                            |
| [test-paths](../../reference/project-configs/test-paths.md)                   | Directories to where your test files live                                                                            |
| [analysis-paths](../../reference/project-configs/analysis-paths.md)           | Directories to where your analyses live                                                                              |
| [macro-paths](../../reference/project-configs/macro-paths.md)                 | Directories to where your macros live                                                                                |
| [snapshot-paths](../../reference/project-configs/snapshot-paths.md)           | Directories to where your snapshots live                                                                             |
| [docs-paths](../../reference/project-configs/docs-paths.md)                   | Directories to where your docs blocks live                                                                           |
| [vars](./project-variables.md)                                 | Project variables you want to use for data compilation                                                               |

For complete details on project configurations, see [dbt\_project.yml](../../reference/dbt_project.yml.md).

## Project subdirectories

You can use the Project subdirectory option in dbt to specify a subdirectory in your git repository that dbt should use as the root directory for your project. This is helpful when you have multiple dbt projects in one repository or when you want to organize your dbt project files into subdirectories for easier management.

To use the Project subdirectory option in dbt, follow these steps:

1. Click your account name in the bottom left and select **Your profile**.

2. Under **Projects**, select the project you want to configure as a project subdirectory.

3. Select **Edit** on the lower right-hand corner of the page.

4. In the **Project subdirectory** field, add the name of the subdirectory. For example, if your project YAML files are located in a subdirectory called `<repository>/finance`, you would enter `finance` as the subdirectory.

   * You can also reference nested subdirectories. For example, if your project YAML files are located in `<repository>/teams/finance`, you would enter `teams/finance` as the subdirectory. **Note**: You do not need a leading or trailing `/` in the Project subdirectory field.

5. Click **Save** when you've finished.

After configuring the Project subdirectory option, dbt will use it as the root directory for your dbt project. This means that dbt commands, such as `dbt run` or `dbt test`, will operate on files within the specified subdirectory. If there is no `dbt_project.yml` file in the Project subdirectory, you will be prompted to initialize the dbt project.

Project support in dbt plans

Some [plans](https://www.getdbt.com/pricing) support only one dbt project, while [Enterprise-tier plans](https://www.getdbt.com/contact) allow multiple projects and [cross-project references](../../best-practices/how-we-mesh/mesh-1-intro.md) with Mesh.

## New projects

You can create new projects and [share them](../platform/git/git-version-control.md) with other people by making them available on a hosted git repository like GitHub, GitLab, and BitBucket.

After you set up a connection with your data platform, you can [initialize your new project in dbt](https://docs.getdbt.com/guides) and start developing. Or, run [dbt init from the command line](../../reference/commands/init.md) to set up your new project.

During project initialization, dbt creates sample model files in your project directory to help you start developing quickly.

## Sample projects

If you want to explore dbt projects more in-depth, refer to [Clone the Jaffle Shop sample project](../../guides/clone-jaffle-shop.md). It's a runnable project that contains sample configurations and helpful notes.

If you want to see what a mature, production project looks like, check out the [GitLab Data Team public repo](https://gitlab.com/gitlab-data/analytics/-/tree/master/transform/snowflake-dbt).

## Related docs

* [Best practices: How we structure our dbt projects](../../best-practices/how-we-structure/1-guide-overview.md)
* [Quickstarts for dbt](https://docs.getdbt.com/guides)
* [Quickstart for dbt Core](../../guides/manual-install.md)
