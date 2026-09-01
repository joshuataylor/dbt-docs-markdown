# dbt Wizard home tab [Preview](https://docs.getdbt.com/docs/dbt-versions/product-lifecycles "Go to https://docs.getdbt.com/docs/dbt-versions/product-lifecycles")

dbt platform | Usage-based

The dbt Wizard home tab is an agent-native development experience in the dbt platform.

Use the dbt Wizard home tab to investigate and generate changes with natural language prompts, review inline diffs and DAG previews, and validate changes without leaving the agent.

AI features are being enabled by default. They're already on for new accounts and are rolling out soon to existing accounts. If your organization opted out, they'll remain off. Admins can [turn AI off or back on and configure providers](./manage-dbt-ai.md) anytime.

The dbt Wizard home tab is complementary to the [dbt Wizard experience in Studio IDE](../dbt-ai/wizard-ide.md). Where the Studio IDE supports users working directly within a traditional IDE environment, the home tab is purpose-built for agent-native development and keeps you focused on supervising and validating agent-generated work.

![dbt Wizard home tab — empty state with quick-start prompts](/img/docs/dbt-platform/wizard-home-empty.png?v=2 "dbt Wizard home tab — empty state with quick-start prompts")dbt Wizard home tab — empty state with quick-start prompts

![dbt Wizard agent refactoring a docs github model for tech writers :) ](/img/docs/dbt-platform/wizard-home-agent.png?v=2 "dbt Wizard agent refactoring a docs github model for tech writers :) ")dbt Wizard agent refactoring a docs github model for tech writers :)

## Prerequisites

* A [dbt account](https://www.getdbt.com/signup) and [Developer seat license](./manage-access/seats-and-users.md).
  * [Legacy Team plans](./billing/plans-and-billing.md#legacy-plans) don't have access to dbt Wizard. Move to a [Starter, Enterprise, or Enterprise+ plan](https://www.getdbt.com/pricing) to use it.
* A [development environment](./studio-ide/develop-in-studio.md#get-started-with-the-studio-ide) and credentials set up in the Studio IDE.
* Use a supported AI provider. Refer to [Supported AI providers](./wizard-platform.md#supported-ai-providers), or the [Model Provider Rate Table](https://www.getdbt.com/legal/dbt-wizard-token-costs-by-model) for the full model list and rates.
* If you're using dbt Wizard in the home tab, you need to [enable experimental features](../dbt-versions/experimental-features.md) for your account.

If dbt Wizard stops responding, your account may have used up its usage credits. Refer to [Trial and billing](../dbt-ai/pricing-billing/trial-and-billing.md).

## What you can do

Use dbt Wizard in the home tab to:

* **Answer project-aware questions**: Ask about lineage, dependencies, model logic, and project context.
* **Debug failed jobs**: Investigate run and job failures using dbt Agent Skills with full project context.
* **Create and manage branches**: Initiate and switch branches directly from the agent workflow.
* **Make model and project changes**: Refactor SQL, update YAML, and modify project configuration through natural language.
* **Generate and refine transformation logic**: Build or rewrite models, tests, documentation, and semantic definitions from plain-language prompts.
* **Run validation workflows**: Execute compile and build checks to validate proposed changes before they're persisted.
* **Choose your model**: Select the dbt managed model you'd like to work with from the [model picker](../dbt-ai/pricing-billing/overview.md#choose-a-model).

Best practices for using dbt Wizard

Refer to [How to use dbt Wizard in your dbt project](../../best-practices/how-to-use-wizard/wizard-1-intro.md) for recommended workflows — including [debugging a failed job](../../best-practices/how-to-use-wizard/wizard-5-debug-failed-job.md), which applies directly to the home tab.

## Inline preview mode

A core part of the home tab experience is **inline preview mode**, which gives you multiple ways to review and validate agent-generated changes directly in the workflow without switching to a separate tool.

The preview experience includes:

* **Enhanced SQL diffs**: Review proposed code changes side by side before accepting them.
* **Structural DAG visualizations**: Inspect transformations as operator-level DAG views that break logic into familiar patterns — joins, filters, aggregations, and projections — making it easier to understand how a model changed beyond the raw SQL.
* **Execution-aware validation feedback**: See results from compile and build checks inline, so you can assess both the proposed implementation and how the transformation behaves in practice.
* **Jump to related surfaces**: From the preview, open a model directly in Catalog to explore metadata and lineage, or open the file in Studio IDE to iterate manually when needed.

## When to use the home tab or Studio IDE

The home tab and Studio IDE support different parts of the development workflow:

|                      | Home tab                                                         | Studio IDE                                                       |
| -------------------- | ---------------------------------------------------------------- | ---------------------------------------------------------------- |
| **Primary workflow** | Supervise and validate agent-generated work                      | Write and edit code directly in the editor                       |
| **Interface**        | Streamlined agent-native chat                                    | Full IDE with file explorer, editor, console                     |
| **Best for**         | Natural language iteration, reviewing diffs, running validations | Manual file edits, direct SQL authoring, complex multi-file work |
| **Inline preview**   | ✅ SQL diffs, DAG visualizations, build feedback                 | File diffs shown before changes are persisted                    |

For most development workflows, you can move between the two surfaces freely. Use the home tab to investigate and generate, and drop into Studio IDE when you need direct control.

## Choose a model

With dbt managed inference, you can switch between the supported managed models at any using the model picker dropdown next to the **Agent mode** control (where you choose **Ask for approval** or **Edit files automatically**).

* In [Studio IDE](../dbt-ai/wizard-ide.md) and the [dbt Wizard home tab](./wizard-home.md), open the model picker dropdown next to the **Agent mode** control in the dbt Wizard panel, then select a model.
* The picker lists the managed models available to you. Refer to the [Model Provider Rate table](https://www.getdbt.com/legal/dbt-wizard-token-costs-by-model) for the available models and their token rates.
* If you [bring your own key (BYOK)](../dbt-ai/wizard-byok.md), dbt Wizard uses the provider and model you configured with your key rather than the managed model picker.

![The model picker dropdown next to the Agent mode control in the Wizard panel.](/img/docs/dbt-platform/wizard-model-picker.png?v=2 "The model picker dropdown next to the Agent mode control in the Wizard panel.")The model picker dropdown next to the Agent mode control in the Wizard panel.

## Related docs

* [dbt Wizard in Studio IDE](../dbt-ai/wizard-ide.md)
* [About dbt Wizard in the dbt platform](./wizard-platform.md)
* [How dbt Wizard works](../dbt-ai/wizard-how-it-works.md)
* [Prompt cookbook](../../guides/prompt-cookbook.md)
* [dbt AI FAQs](../dbt-ai/dbt-ai-faqs.md)
