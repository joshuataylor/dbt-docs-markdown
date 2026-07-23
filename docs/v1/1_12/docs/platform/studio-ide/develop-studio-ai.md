# Develop with AI in the Studio IDE [Starter](https://www.getdbt.com/pricing "Go to https://www.getdbt.com/pricing")[Enterprise](https://www.getdbt.com/pricing "Go to https://www.getdbt.com/pricing")[Enterprise +](https://www.getdbt.com/pricing "Go to https://www.getdbt.com/pricing")

Leverage AI to develop dbt projects in the Studio IDE.

[dbt Wizard](https://docs.getdbt.com/docs/platform/wizard-platform.md) in [Studio IDE](https://docs.getdbt.com/docs/platform/studio-ide/develop-in-studio.md) is the new and recommended way to develop governed dbt projects with AI. It uses your project context to help you develop governed dbt changes faster. Think of it like a smart AI agent that has a map of your project: instead of reading through each file to understand context, it can answer questions and help you develop and validate changes faster.

dbt also supports dbt Copilot, a separate inline AI assistance experience for single-click generation of SQL, documentation, tests, and semantic models in Studio IDE, Canvas, and Insights.

## Prerequisites[​](#prerequisites "Direct link to Prerequisites")

* A Starter, Enterprise, or Enterprise+ plan
* A [dbt account](https://www.getdbt.com/signup) and [Developer seat license](https://docs.getdbt.com/docs/platform/manage-access/seats-and-users.md).
* A [development environment](https://docs.getdbt.com/docs/platform/studio-ide/develop-in-studio.md#get-started-with-the-studio-ide) and credentials set up in the Studio IDE.
* [Enabled AI features](https://docs.getdbt.com/docs/platform/enable-dbt-ai.md#enable-ai-features) for your account.

## dbt Wizard in Studio IDE [Preview](https://docs.getdbt.com/docs/dbt-versions/product-lifecycles "Go to https://docs.getdbt.com/docs/dbt-versions/product-lifecycles")[Starter](https://www.getdbt.com/pricing "Go to https://www.getdbt.com/pricing")[Enterprise](https://www.getdbt.com/pricing "Go to https://www.getdbt.com/pricing")[Enterprise +](https://www.getdbt.com/pricing "Go to https://www.getdbt.com/pricing")[​](#dbt-wizard-in-studio-ide- "Direct link to dbt-wizard-in-studio-ide-")

Use [dbt Wizard](https://docs.getdbt.com/docs/dbt-ai/wizard-ide.md) for autonomous model generation, refactoring, and multi-step workflows in the Studio IDE. It's grounded in dbt's native metadata engine — a structured index of your project's lineage, tests, model health, and semantic definitions — so it knows your data, not just your code.

dbt Wizard is accessible from the sidebar panel or in the Console section of the Studio IDE.

#### Availability and considerations[​](#availability-and-considerations "Direct link to Availability and considerations")

* **Where it runs:** Supported in the [Studio IDE](https://docs.getdbt.com/docs/platform/studio-ide/develop-in-studio.md) only, all [deployment types](https://docs.getdbt.com/docs/platform/about-platform/tenancy.md?version=2.0). Not supported in VS Code or the dbt CLI.
* **Engines:** Works with dbt Fusion engine and dbt Core.
* **Conversations:** In the conversation list, open **More actions** menu (three dots) of the conversation you want to delete, then click **Delete** to remove one thread. Deleting the open thread clears the panel.
* **Sessions:** Refreshing the same browser tab keeps your active session. A new tab, or returning after closing the tab, starts empty.
* **Chat history:** Retained for 90 days only. Chat history isn't supported yet on single-tenant deployments, so save anything important before closing.
* **Plan mode:** Not supported yet. The agent doesn't show a separate plan before applying changes, however you can use the **Ask for approval** mode to approve each file.
* **New chat:** Click **Start new dbt Wizard chat** (top right of the dbt Wizard panel) to begin a new session.

### Using dbt Wizard[​](#using-dbt-wizard "Direct link to Using dbt Wizard")

Use the dbt Wizard panel to generate resources with quick actions, or use the agent to build and refactor models end-to-end with natural language prompts.

To use the dbt Wizard, follow these steps:

1. Open your dbt project in the [Studio IDE](https://docs.getdbt.com/docs/platform/studio-ide/develop-in-studio.md), then click **dbt Wizard** in the command palette.

2. Start a prompt in several ways in the [dbt Wizard panel](https://docs.getdbt.com/docs/dbt-ai/wizard-ide.md):

   <!-- -->

   * **Quick actions**: Use [quick-action resource generation](https://docs.getdbt.com/docs/dbt-ai/wizard-ide.md#quick-action-resource-generation) at the top of the panel to generate documentation, tests, semantic models, and metrics.
   * **Plain text**: Type directly into the text field to describe what you want to build or change.
   * **Model context**: Type `@` to select a model as context. This scopes the agent's changes to that resource.

3. Select the [**Agent mode** button](https://docs.getdbt.com/docs/dbt-ai/wizard-ide.md) to specify the mode for the dbt Wizard. Available modes are **Ask for approval** (default) and **Edit files automatically**.

4. [Review the agent's suggestions](https://docs.getdbt.com/docs/dbt-ai/wizard-ide.md) and approve or reject the changes. You can also use the **Start new dbt Wizard chat** button to start a new chat session.

5. [Approve dbt commands](https://docs.getdbt.com/docs/dbt-ai/wizard-ide.md) when the dbt Wizard requests to run commands like `dbt compile` or `dbt build`.

6. Repeat the process to build or change more models.

7. Commit the changes to your dbt project and open a pull request.

Your browser does not support the video tag.

Example of using the dbt Wizard to refactor a model in the Studio IDE.

For more details on the dbt Wizard and how it works, expand the following sections to open additional information.

 Panel controls

The dbt Wizard panel contains:

1. **Quick actions** (center): Buttons at the top of the panel for common tasks like generating documentation, tests, semantic models, and metrics. When selected, the text field is pre-filled with a prompt.
2. **Agent mode button** (bottom left): Switch between **Ask for approval** and **Edit files automatically** mode. Click the button to change modes.
3. **dbt model context** (bottom left): Shows the currently open file. Use `@` in the text field to reference a different dbt model. Click **x** to remove the dbt model context.
4. **Text input field** (bottom left): Type your prompt in the text field to describe what you want to build or change. Type `@` to select a dbt model as context. This scopes the agent's changes to that resource.
5. **Start new dbt Wizard chat** (top right): Starts a new chat session.
6. **Stop** or **Enter** (bottom right): Press **Enter** to submit your prompt. Press **Stop** to stop the current session and agent processing. You cannot undo this action.

[![The Wizard panel in the Studio IDE showing quick-action buttons, text input field, and agent mode controls.](/img/docs/dbt-platform/dev-agent-copilot-panel.png?v=2 "The Wizard panel in the Studio IDE showing quick-action buttons, text input field, and agent mode controls.")](#)The Wizard panel in the Studio IDE showing quick-action buttons, text input field, and agent mode controls.

also has a simplified wayfinder bar above the text input field. The wayfinder bar shows your current project and branch and guides you through Git tasks, such as committing files or creating a branch.

 Agent modes

The dbt Wizard operates in two modes:

| Mode                           | Behavior                                                                                                                                                    |
| ------------------------------ | ----------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **Ask for approval** (default) | The agent drafts edits to files. You approve each file change before it is persisted. Best when you want tight control over what gets saved to your branch. |
| **Edit files automatically**   | The agent drafts and automatically saves file edits without per-file approval. Best for faster iteration when you're confident in the prompt.               |

You can switch between modes at any time by clicking the **Agent mode** button in the dbt Wizard panel.

[![dbt Wizard in Ask for approval mode, requesting approval before making file edits.](/img/docs/dbt-platform/dev-agent-ask-mode.png?v=2 "dbt Wizard in Ask for approval mode, requesting approval before making file edits.")](#)dbt Wizard in Ask for approval mode, requesting approval before making file edits.

 Reviewing agent suggestions

When the dbt Wizard proposes code changes, you can review them before they are saved to your project:

* **View the diff**: The agent displays a diff of the proposed changes. Click **Show all X lines** to expand and view the full suggestion.
* **Line indicators**: Added and removed lines are highlighted with line number indicators so you can see exactly what changed.
* **Copy or open in editor**: Use the options in the top-right corner of the diff view to copy the suggestion or open it directly in the editor.

[![dbt Wizard displaying a diff of proposed YAML changes with line indicators and copy/open options.](/img/docs/dbt-platform/dev-agent-code-suggestion.png?v=2 "dbt Wizard displaying a diff of proposed YAML changes with line indicators and copy/open options.")](#)dbt Wizard displaying a diff of proposed YAML changes with line indicators and copy/open options.

 Granting command permissions

To validate or run models during a session, the agent may request to run dbt commands such as `dbt compile` or `dbt build`. You'll be prompted to approve each request before it executes. For example, the agent might request to run:

```bash
dbt compile --select model_name
```

You can select one of the following options:

| Option                                                | Behavior                                                                                         |
| ----------------------------------------------------- | ------------------------------------------------------------------------------------------------ |
| **Yes, run once**                                     | Grants permission to run this specific command one time.                                         |
| **Yes, and allow `dbt_command_name` for the session** | Grants permission to run dbt commands for the remainder of your session without prompting again. |
| **No**                                                | Denies the request. The agent will not run the command.                                          |

After you run a command, dbt Wizard adds an icon and a tooltip to the Studio IDE [**Commands** tab](https://docs.getdbt.com/docs/platform/studio-ide/ide-user-interface.md#console-section) results. This helps you distinguish agent-run commands from manually run commands in the run results and logs.

[![Commands run by dbt Wizard appear in the Studio IDE Commands tab with a dbt Wizard icon and 'Run by dbt Wizard' tooltip.](/img/docs/dbt-platform/dev-agent-cmd-icon.png?v=2 "Commands run by dbt Wizard appear in the Studio IDE Commands tab with a dbt Wizard icon and 'Run by dbt Wizard' tooltip.")](#)Commands run by dbt Wizard appear in the Studio IDE Commands tab with a dbt Wizard icon and 'Run by dbt Wizard' tooltip.

### Bringing your own skills[​](#bringing-your-own-skills "Direct link to Bringing your own skills")

You can extend dbt Wizard with custom skills to encode your team's SQL conventions, naming rules, and modeling workflows — so you don't repeat them in every prompt. See [Skills](https://docs.getdbt.com/docs/dbt-ai/wizard-platform-skills.md) for the full reference, including how to create, structure, and invoke skills.

### Debug job failures[​](#debug-job-failures "Direct link to Debug job failures")

The dbt Wizard can investigate and troubleshoot dbt job and run failures directly from the Studio IDE. This capability is powered by the `troubleshooting-dbt-job-errors` [dbt Agent Skill](https://github.com/dbt-labs/dbt-agent-skills), which comes pre-configured with the agent — no setup required.

You can ask the agent questions and issue commands like:

* "What jobs have failed recently?"
* "What is the root cause of the job failure?"
* "How can I fix the recent job failure?"
* "Fix the job failure."

The agent notes when your local project state may differ from the job — for example, if you're on a different branch or have uncommitted changes — so you have full context before acting on any suggested fixes.

### Timeout handling[​](#timeout-handling "Direct link to Timeout handling")

When a dbt command run by dbt Wizard runs for more than 5 minutes, the agent automatically attempts to stop the command on the server before returning control to you.

Instead of hanging or showing a generic error, the agent returns a clear message that explains the command timed out and was aborted. The message also tells you whether the cancellation request succeeded. If cancellation fails, it's possible the command may still be running on the server.

You can then choose whether to retry the command, narrow the request, or take another action.

### Fusion migration workflow[​](#fusion-migration-workflow "Direct link to Fusion migration workflow")

<!-- -->

If you have access to [dbt Wizard](https://docs.getdbt.com/docs/dbt-ai/wizard-ide.md) with [AI features](https://docs.getdbt.com/docs/platform/enable-dbt-ai.md?version=2.0#enable-dbt-wizard) enabled, you can use the [Fusion migration workflow](https://docs.getdbt.com/docs/dbt-ai/wizard-ide.md#fusion-migration-workflow) skill. This skill can help you fix compatibility errors directly from the Studio IDE using dbt Wizard — no manual log investigation needed. It classifies every error, applies validated fixes automatically, and surfaces what's blocked.

info

The Fusion migration workflow is accessible through the dbt Wizard in the Studio IDE. If you're using VS Code or the dbt CLI, use the [autofix tool](https://docs.getdbt.com/guides/fusion-package-compat?step=4) instead.

1. From the job list, click the **Review job** button for a job with a successful run.
   <!-- -->
   * If you don't see the **Review job** button, enable the **Show Fusion eligibility** toggle in the job list.

2. In the **Fusion eligibility unknown for this job** pop-up, click **Debug in Studio with dbt Wizard**.

3. dbt redirects you to the Studio IDE and sets your personal development environment to Fusion.

4. dbt Wizard opens and automatically triggers the Fusion migration skill with this prompt:

   <!-- -->

   ```text
   I need help fixing Fusion compatibility issues in this project. Please investigate and resolve any deprecation warnings or incompatibilities. Please use the migrating-dbt-core-to-fusion skill to guide this.
   ```

5. Review and approve dbt Wizard's permission requests so it can run the commands it needs.

6. The dbt Wizard iteratively runs `dbt compile`, reads the results, and applies fixes until it reaches a successful compile or encounters an error it can't resolve. If it gets blocked, it exits cleanly, explains what it could not fix, and creates and links to a markdown file summarizing all changes made.

7. When the project compiles with no warnings or errors, commit and publish your changes.

8. After you merge the changes, wait for the job to run again or run it manually on Fusion.

[![The Developer Agent's fusion migration workflow triaging and fixing Fusion compatibility errors in the Studio IDE.](/img/docs/dbt-platform/fusion-migration-workflow.gif?v=2 "The Developer Agent's fusion migration workflow triaging and fixing Fusion compatibility errors in the Studio IDE.")](#)The Developer Agent's fusion migration workflow triaging and fixing Fusion compatibility errors in the Studio IDE.

For more on how to prepare your project for Fusion and what to do when you hit compatibility errors, see the [Fusion readiness checklist](https://docs.getdbt.com/docs/fusion/fusion-readiness.md) and the [Upgrade to Fusion guides](https://docs.getdbt.com/guides/prepare-fusion-upgrade.md).

### Writing effective prompts[​](#writing-effective-prompts "Direct link to Writing effective prompts")

Good prompts include the *scope* (which models or area of the project), the *intent* (the transformation or business logic you want), and any *constraints* (naming conventions, materialization, tests). Here are a few examples:

| Task                       | Example prompt                                                                                                                                  |
| -------------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------- |
| Build a new model          | "Create a model called `fct_daily_revenue` that joins `stg_orders` and `stg_payments`, aggregates revenue by day, and materializes as a table." |
| Refactor an existing model | "Refactor `fct_orders` to use incremental materialization. Keep existing tests and follow our naming conventions."                              |
| Generate tests and docs    | "Add `not_null` and `unique` tests to the primary key of `dim_customers`, and generate documentation for all columns."                          |

For detailed guidance, patterns, and more examples across SQL, documentation, tests, and semantic models, see the [Prompt cookbook](https://docs.getdbt.com/guides/prompt-cookbook.md).

## dbt Copilot in Studio IDE[​](#dbt-copilot-in-studio-ide "Direct link to dbt Copilot in Studio IDE")

[dbt Copilot](https://docs.getdbt.com/docs/dbt-ai/copilot-overview.md) provides single-click generation of SQL, documentation, tests, and semantic models in Studio IDE, Canvas, and Insights.

1. Navigate to the Studio IDE and select a SQL model file under the **File Explorer**.
2. In the **Console** section, click the **dbt Copilot** button to view the available AI options.
3. Select the available options to [generate resources](#generate-resources) such as using the quick-action buttons to generate documentation, tests, semantic models, or metrics.

The following sections describe how to use dbt Copilot in the Studio IDE.

### Generate resources[​](#generate-resources "Direct link to Generate resources")

To generate resources with dbt Copilot, follow these steps:

Generate documentation, tests, metrics, and semantic models [resources](https://docs.getdbt.com/docs/build/projects.md) with the click-of-a-button in the [Studio IDE](https://docs.getdbt.com/docs/platform/studio-ide/develop-in-studio.md) using dbt Copilot, saving you time. To access and use this AI feature:

1. Navigate to the Studio IDE and select a SQL model file under the **File Explorer**.

2. In the **Console** section (under the **File Editor**), click **dbt Copilot** to view the available AI options.

3. Select the available options to generate the YAML config: **Generate Documentation**, **Generate Tests**, **Generate Semantic Model**, or **Generate Metrics**. To generate multiple YAML configs for the same model, click each option separately. dbt Copilot intelligently saves the YAML config in the same file.

   note

   dbt Copilot doesn't yet support generating semantic models with the latest YAML spec.

   * To generate metrics, you need to first have semantic models defined.
   * Once defined, click **dbt Copilot** and select **Generate Metrics**.
   * Write a prompt describing the metrics you want to generate and press enter.
   * **Accept** or **Reject** the generated code.

4. Verify the AI-generated code. You can update or fix the code as needed.

5. Click **Save As**. You should see the file changes under the **Version control** section.

### Inline SQL editing[​](#inline-sql-editing "Direct link to Inline SQL editing")

dbt Copilot supports a quick inline prompt window for targeted SQL edits directly in the editor. Press **Cmd+B** (Mac) or **Ctrl+B** (Windows) to open the prompt window within a model file, describe what you want to generate or change, and dbt Copilot displays a diff of the proposed changes — click **Accept** to apply or **Reject** to discard.

This is useful for scoped edits within a single SQL file. For larger or multi-step changes — like building a new model, refactoring across files, or generating YAML, review how [dbt Wizard](https://docs.getdbt.com/docs/dbt-ai/wizard-ide.md) can help instead.
