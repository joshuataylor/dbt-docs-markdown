# Set up the dbt Semantic Layer

## Getting started[​](#getting-started "Direct link to Getting started")

There are two options for developing a dbt project, including the Semantic Layer:

* [dbt CLI](https://docs.getdbt.com/docs/cloud/cloud-cli-installation.md) — MetricFlow commands are embedded in the dbt CLI under the `dbt sl` subcommand. This is the easiest, most full-featured way to develop Semantic Layer code for the time being. You can use the editor of your choice and run commands from the terminal.

* [Studio IDE](https://docs.getdbt.com/docs/cloud/studio-ide/develop-in-studio.md) — You can create semantic models and metrics in the Studio IDE.

## Basic commands[​](#basic-commands "Direct link to Basic commands")

* 🔍 A less common command that will come in handy with the Semantic Layer is `dbt parse`. This will parse your project and generate a **semantic manifest**, a representation of meaningful connections described by your project. This is uploaded to dbt, and used for running `dbt sl` commands in development. This file gives MetricFlow a **state of the world from which to generate queries**.
* 🧰 `dbt sl query` is your other best friend, it will execute a query against your semantic layer and return a sample of the results. This is great for testing your semantic models and metrics as you build them. For example, if you're building a revenue model you can run `dbt sl query --metrics revenue --group-by metric_time__month` to validate that monthly revenue is calculating correctly.
* 📝 Lastly, `dbt sl list dimensions --metrics [metric name]` will list all the dimensions available for a given metric. This is useful for checking that you're increasing dimensionality as you progress. You can `dbt sl list` other aspects of your Semantic Layer as well, run `dbt sl list --help` for the full list of options.

For more information on the available commands, refer to the [MetricFlow commands](https://docs.getdbt.com/docs/build/metricflow-commands.md) reference, or use `dbt sl --help` and `dbt sl [subcommand] --help` on the command line. If you need to set up a dbt project first, check out the [quickstart guides](https://docs.getdbt.com/docs/get-started-dbt.md).

## Onward\![​](#onward "Direct link to Onward!")

Throughout the rest of the guide, we'll show example code based on the Jaffle Shop project, a fictional chain of restaurants. You can check out the code yourself and try things out in the [Jaffle Shop repository](https://github.com/dbt-labs/jaffle-shop). So if you see us calculating metrics like `food_revenue` later in this guide, this is why!

## Was this page helpful?

YesNo

[Privacy policy](https://www.getdbt.com/cloud/privacy-policy)[Create a GitHub issue](https://github.com/dbt-labs/docs.getdbt.com/issues)

This site is protected by reCAPTCHA and the Google [Privacy Policy](https://policies.google.com/privacy) and [Terms of Service](https://policies.google.com/terms) apply.
