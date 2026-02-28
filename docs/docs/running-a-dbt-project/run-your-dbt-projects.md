# Run your dbt projects

You can run your dbt projects with [dbt](https://docs.getdbt.com/docs/cloud/about-cloud/dbt-cloud-features.md) or [dbt Core](https://github.com/dbt-labs/dbt-core):

* **dbt**: A hosted application where you can develop directly from a web browser using the [Studio IDE](https://docs.getdbt.com/docs/cloud/studio-ide/develop-in-studio.md). It also natively supports developing using a command line interface, [dbt CLI](https://docs.getdbt.com/docs/cloud/cloud-cli-installation.md). Among other features, dbt provides:

  * Development environment to help you build, test, run, and [version control](https://docs.getdbt.com/docs/cloud/git/git-version-control.md) your project faster.
  * Share your [dbt project's documentation](https://docs.getdbt.com/docs/build/documentation.md) with your team.
  * Integrates with the Studio IDE, allowing you to run development tasks and environment in the dbt UI for a seamless experience.
  * The dbt CLI to develop and run dbt commands against your dbt development environment from your local command line.
  * For more details, refer to [Develop dbt](https://docs.getdbt.com/docs/cloud/about-develop-dbt.md).

* **dbt Core**: An open source project where you can develop from the [command line](https://docs.getdbt.com/docs/core/installation-overview.md).

The dbt CLI and dbt Core are both command line tools that enable you to run dbt commands. The key distinction is the dbt CLI is tailored for dbt's infrastructure and integrates with all its [features](https://docs.getdbt.com/docs/cloud/about-cloud/dbt-cloud-features.md).

The command line is available from your computer's terminal application such as Terminal and iTerm. With the command line, you can run commands and do other work from the current working directory on your computer. Before running the dbt project from the command line, make sure you are working in your dbt project directory. Learning terminal commands such as `cd` (change directory), `ls` (list directory contents), and `pwd` (present working directory) can help you navigate the directory structure on your system.

In dbt or dbt Core, the commands you commonly use are:

* [dbt run](https://docs.getdbt.com/reference/commands/run.md) — Runs the models you defined in your project
* [dbt build](https://docs.getdbt.com/reference/commands/build.md) — Builds and tests your selected resources such as models, seeds, snapshots, and tests
* [dbt test](https://docs.getdbt.com/reference/commands/test.md) — Executes the tests you defined for your project

For information on all dbt commands and their arguments (flags), see the [dbt command reference](https://docs.getdbt.com/reference/dbt-commands.md). If you want to list all dbt commands from the command line, run `dbt --help`. To list a dbt command’s specific arguments, run `dbt COMMAND_NAME --help` .

## Related docs[​](#related-docs "Direct link to Related docs")

* [How we set up our computers for working on dbt projects](https://discourse.getdbt.com/t/how-we-set-up-our-computers-for-working-on-dbt-projects/243)
* [Model selection syntax](https://docs.getdbt.com/reference/node-selection/syntax.md)
* [dbt CLI](https://docs.getdbt.com/docs/cloud/cloud-cli-installation.md)
* [Cloud Studio IDE features](https://docs.getdbt.com/docs/cloud/studio-ide/develop-in-studio.md#ide-features)
* [Does dbt offer extract and load functionality?](https://docs.getdbt.com/faqs/Project/transformation-tool.md)
* [Why does dbt compile need a data platform connection](https://docs.getdbt.com/faqs/Warehouse/db-connection-dbt-compile.md)

## Was this page helpful?

YesNo

[Privacy policy](https://www.getdbt.com/cloud/privacy-policy)[Create a GitHub issue](https://github.com/dbt-labs/docs.getdbt.com/issues)

This site is protected by reCAPTCHA and the Google [Privacy Policy](https://policies.google.com/privacy) and [Terms of Service](https://policies.google.com/terms) apply.
