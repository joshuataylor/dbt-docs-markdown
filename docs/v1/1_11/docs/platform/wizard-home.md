# dbt Wizard home tab [Beta](https://docs.getdbt.com/docs/dbt-versions/product-lifecycles "Go to https://docs.getdbt.com/docs/dbt-versions/product-lifecycles")[Starter](https://www.getdbt.com/pricing "Go to https://www.getdbt.com/pricing")[Enterprise](https://www.getdbt.com/pricing "Go to https://www.getdbt.com/pricing")[Enterprise +](https://www.getdbt.com/pricing "Go to https://www.getdbt.com/pricing")

The dbt Wizard home tab is an agent-native development experience in the dbt platform. It centers your workflow around collaborating with the agent itself — iterating through natural language, reviewing generated changes, and validating outcomes — without the overhead of a traditional IDE environment.

See it in action and share your feedback

Want to see dbt Wizard in action? Check out the [demo video](https://www.youtube.com/watch?v=-lIzh1xQWMA).

We'd love to hear how dbt Wizard is working for you. Share your feedback by either running the `/feedback` slash command in your interactive terminal session or by going to the [#dbt-wizard](https://getdbt.slack.com/archives/C0B6KLW6T26) channel in the [dbt Community Slack](https://docs.getdbt.com/community/join?version=2.0).

Thanks so much for your help in improving dbt Wizard and dbt data development!

[![dbt Wizard home tab — empty state with quick-start prompts](/img/docs/dbt-platform/wizard-home-empty.png?v=2 "dbt Wizard home tab — empty state with quick-start prompts")](#)dbt Wizard home tab — empty state with quick-start prompts

[![dbt Wizard agent refactoring a docs github model for tech writers :) ](/img/docs/dbt-platform/wizard-home-agent.png?v=2 "dbt Wizard agent refactoring a docs github model for tech writers :) ")](#)dbt Wizard agent refactoring a docs github model for tech writers :)

The dbt Wizard home tab is complementary to the [dbt Wizard experience in Studio IDE](https://docs.getdbt.com/docs/dbt-ai/wizard-ide.md). Where the Studio IDE supports users working directly within a traditional IDE environment, the home tab is purpose-built for agent-native development — it reduces overhead and keeps you focused on supervising and validating agent-generated work.

## Prerequisites[​](#prerequisites "Direct link to Prerequisites")

* A Starter, Enterprise, or Enterprise+ plan
* A [dbt account](https://www.getdbt.com/signup) and [Developer seat license](https://docs.getdbt.com/docs/platform/manage-access/seats-and-users.md).
* A [development environment](https://docs.getdbt.com/docs/platform/studio-ide/develop-in-studio.md#get-started-with-the-studio-ide) and credentials set up in the Studio IDE.
* [Enabled AI features](https://docs.getdbt.com/docs/platform/enable-dbt-ai.md#enable-ai-features) for your account.
* If you're using dbt Wizard in the home tab, you need to [enable experimental features](https://docs.getdbt.com/docs/dbt-versions/experimental-features.md) for your account.

## What you can do[​](#what-you-can-do "Direct link to What you can do")

Use dbt Wizard in the home tab to:

* **Answer project-aware questions**: Ask about lineage, dependencies, model logic, and project context.
* **Debug failed jobs**: Investigate run and job failures using dbt Agent Skills with full project context.
* **Create and manage branches**: Initiate and switch branches directly from the agent workflow.
* **Make model and project changes**: Refactor SQL, update YAML, and modify project configuration through natural language.
* **Generate and refine transformation logic**: Build or rewrite models, tests, documentation, and semantic definitions from plain-language prompts.
* **Run validation workflows**: Execute compile and build checks to validate proposed changes before they're persisted.

Best practices for using dbt Wizard

Refer to [How to use dbt Wizard in your dbt project](https://docs.getdbt.com/best-practices/how-to-use-wizard/wizard-1-intro.md) for recommended workflows — including [debugging a failed job](https://docs.getdbt.com/best-practices/how-to-use-wizard/wizard-5-debug-failed-job.md), which applies directly to the home tab.

tip

Always review AI-generated content before applying it. For prompt best practices, refer to the [Prompt cookbook](https://docs.getdbt.com/guides/prompt-cookbook.md).

## Inline preview mode[​](#inline-preview-mode "Direct link to Inline preview mode")

A core part of the home tab experience is **inline preview mode**, which gives you multiple ways to review and validate agent-generated changes directly in the workflow without switching to a separate tool.

The preview experience includes:

* **Enhanced SQL diffs**: Review proposed code changes side by side before accepting them.
* **Structural DAG visualizations**: Inspect transformations as operator-level DAG views that break logic into familiar patterns — joins, filters, aggregations, and projections — making it easier to understand how a model changed beyond the raw SQL.
* **Execution-aware validation feedback**: See results from compile and build checks inline, so you can assess both the proposed implementation and how the transformation behaves in practice.
* **Jump to related surfaces**: From the preview, open a model directly in Catalog to explore metadata and lineage, or open the file in Studio IDE to iterate manually when needed.

## Wizard home tab vs Studio IDE[​](#wizard-home-tab-vs-studio-ide "Direct link to Wizard home tab vs Studio IDE")

The home tab and Studio IDE support different parts of the development workflow:

|                      | Home tab                                                         | Studio IDE                                                       |
| -------------------- | ---------------------------------------------------------------- | ---------------------------------------------------------------- |
| **Primary workflow** | Supervise and validate agent-generated work                      | Write and edit code directly in the editor                       |
| **Interface**        | Streamlined agent-native chat                                    | Full IDE with file explorer, editor, console                     |
| **Best for**         | Natural language iteration, reviewing diffs, running validations | Manual file edits, direct SQL authoring, complex multi-file work |
| **Inline preview**   | ✅ SQL diffs, DAG visualizations, build feedback                 | File diffs shown before changes are persisted                    |

For most development workflows, you can move between the two surfaces freely. Use the home tab to investigate and generate, and drop into Studio IDE when you need direct control.

## Related docs[​](#related-docs "Direct link to Related docs")

* [dbt Wizard in Studio IDE](https://docs.getdbt.com/docs/dbt-ai/wizard-ide.md)
* [About dbt Wizard in the dbt platform](https://docs.getdbt.com/docs/platform/wizard-platform.md)
* [How dbt Wizard works](https://docs.getdbt.com/docs/dbt-ai/wizard-how-it-works.md)
* [Prompt cookbook](https://docs.getdbt.com/guides/prompt-cookbook.md)
* [dbt AI FAQs](https://docs.getdbt.com/docs/dbt-ai/dbt-ai-faqs.md)
