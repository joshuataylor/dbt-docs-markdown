# How to use Wizard in your dbt project

dbt Wizard is an AI agent that works from your project's dbt metadata — models, lineage, tests, and metrics — instead of guessing from code alone. You can run it locally from your CLI or dbt platform. Refer to [dbt Wizard overview](../../docs/platform/wizard-overview.md) for what it is and [Use Wizard locally](../../docs/dbt-ai/wizard-quickstart.md) to set it up.

This guide collects the workflows we recommend once Wizard is running on a real project: mapping an unfamiliar codebase, validating a change before you ship it, adding tests, debugging a failed job, developing against production state, building Semantic Layer definitions, and extending Wizard with plugins and hooks.

## Learning goals

* Understand the prompts and context that get reliable results from dbt Wizard on real project tasks.
* Develop an intuition for when to reach for dbt Wizard versus doing a task by hand.
* Establish repeatable workflows you and your team can reuse across projects.

## Before you begin

These workflows apply to the dbt Wizard CLI and dbt platform. Each page will reference whether it applies to the CLI or dbt platform.

For local workflows, make sure the dbt Wizard CLI is installed, configured, and connected to a dbt project with an up-to-date `target/manifest.json`.

* To set up the CLI, check out [Use Wizard locally](../../docs/dbt-ai/wizard-quickstart.md)
* For options such as deferral and approval policies, check out the [Wizard CLI config reference](../../docs/dbt-ai/wizard-config.md)
* To use Wizard in the dbt platform, check out [Use Wizard in dbt platform](../../docs/platform/wizard-platform.md)

## Use one chat per task

Start a new dbt Wizard chat for each discrete unit of work, such as building one model, debugging one job failure, or validating a change. A focused conversation keeps the context relevant to the task so dbt Wizard doesn't carry over assumptions from unrelated work. Go back to an existing chat only when you're continuing that same task.

The workflows in this guide follow that pattern: each one is a self-contained task you can run in its own chat. For how to start, resume, and manage sessions on each surface, refer to [Sessions and conversations](../../docs/dbt-ai/wizard-how-it-works.md#sessions-and-conversations) for the dbt platform and [Sessions](../../docs/dbt-ai/wizard-how-it-works.md#sessions) for the CLI and Wizard Desktop.
