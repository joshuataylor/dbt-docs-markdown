# About dbt Core installation

[dbt Core](https://github.com/dbt-labs/dbt-core) is an open-source project where you can develop from the command line and run your dbt project.

To use dbt Core, your workflow generally looks like:

1. **Build your dbt project in a code editor —** popular choices include VS Code and Atom.

2. **Run your project from the command line —** macOS ships with a default Terminal program, however you can also use iTerm or the command line prompt within a code editor to execute dbt commands.

How we set up our computers for working on dbt projects

We've written a [guide](https://discourse.getdbt.com/t/how-we-set-up-our-computers-for-working-on-dbt-projects/243) for our recommended setup when running dbt projects using dbt Core.

If you're using the command line, we recommend learning some basics of your terminal to help you work more effectively. In particular, it's important to understand `cd`, `ls` and `pwd` to be able to navigate through the directory structure of your computer easily.

## Install dbt Core[​](#install-dbt-core "Direct link to Install dbt Core")

You can install dbt Core on the command line by using one of these methods:

* [Use pip to install dbt](https://docs.getdbt.com/docs/core/pip-install.md) (recommended)
* [Use a Docker image to install dbt](https://docs.getdbt.com/docs/core/docker-install.md)
* [Install dbt from source](https://docs.getdbt.com/docs/core/source-install.md)
* You can also develop locally using the [dbt CLI](https://docs.getdbt.com/docs/cloud/cloud-cli-installation.md). The dbt CLI and dbt Core are both command line tools that let you run dbt commands. The key distinction is the dbt CLI is tailored for dbt's infrastructure and integrates with all its [features](https://docs.getdbt.com/docs/cloud/about-cloud/dbt-cloud-features.md).

## Upgrading dbt Core[​](#upgrading-dbt-core "Direct link to Upgrading dbt Core")

dbt provides a number of resources for understanding [general best practices](https://docs.getdbt.com/blog/upgrade-dbt-without-fear) while upgrading your dbt project as well as detailed [migration guides](https://docs.getdbt.com/docs/dbt-versions/core-upgrade.md) highlighting the changes required for each [minor and major release](https://docs.getdbt.com/docs/dbt-versions/core.md).

* [Upgrade `pip`](https://docs.getdbt.com/docs/core/pip-install.md#change-dbt-core-versions)

## About dbt data platforms and adapters[​](#about-dbt-data-platforms-and-adapters "Direct link to About dbt data platforms and adapters")

dbt works with a number of different data platforms (databases, query engines, and other SQL-speaking technologies). It does this by using a dedicated *adapter* for each. When you install dbt Core, you'll also want to install the specific adapter for your database. For more details, see [Supported Data Platforms](https://docs.getdbt.com/docs/supported-data-platforms.md).

Pro tip: Using the --help flag

Most command-line tools, including dbt, have a `--help` flag that you can use to show available commands and arguments. For example, you can use the `--help` flag with dbt in two ways:<br /><br />— `dbt --help`: Lists the commands available for dbt<br />— `dbt run --help`: Lists the flags available for the `run` command

## Create a project[​](#create-a-project "Direct link to Create a project")

After installing dbt Core, create your first [dbt project](https://docs.getdbt.com/docs/build/projects.md) using the [`dbt init`](https://docs.getdbt.com/reference/commands/init.md) command. This initializes a new project with the standard dbt directory structure and helps verify that your installation is working as expected.

## Related content[​](#related-content "Direct link to Related content")

* [Quickstart for dbt Core from a manual install](https://docs.getdbt.com/guides/manual-install?step=1)

## Was this page helpful?

YesNo

[Privacy policy](https://www.getdbt.com/cloud/privacy-policy)[Create a GitHub issue](https://github.com/dbt-labs/docs.getdbt.com/issues)

This site is protected by reCAPTCHA and the Google [Privacy Policy](https://policies.google.com/privacy) and [Terms of Service](https://policies.google.com/terms) apply.
