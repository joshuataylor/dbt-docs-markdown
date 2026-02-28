# Develop with dbt Copilot [Starter](https://www.getdbt.com/pricing "Go to https://www.getdbt.com/pricing")[Enterprise](https://www.getdbt.com/pricing "Go to https://www.getdbt.com/pricing")[Enterprise +](https://www.getdbt.com/pricing "Go to https://www.getdbt.com/pricing")

This page describes how to use Copilot in the Studio IDE to improve your development workflow.

Use [Copilot](https://docs.getdbt.com/docs/cloud/dbt-copilot.md) in the [Studio IDE](https://docs.getdbt.com/docs/cloud/studio-ide/develop-in-studio.md) to generate documentation, tests, semantic models, metrics, and SQL code from scratch — making it easier for you to build your dbt project, accelerate your development, and focus on high-level tasks. For information about using Copilot in the [Canvas](https://docs.getdbt.com/docs/cloud/canvas.md), see [Build with Copilot](https://docs.getdbt.com/docs/cloud/build-canvas-copilot.md).

<!-- -->

## Generate resources[​](#generate-resources "Direct link to Generate resources")

Generate documentation, tests, metrics, and semantic models [resources](https://docs.getdbt.com/docs/build/projects.md) with the click-of-a-button in the [Studio IDE](https://docs.getdbt.com/docs/cloud/studio-ide/develop-in-studio.md) using dbt Copilot, saving you time. To access and use this AI feature:

1. Navigate to the Studio IDE and select a SQL model file under the **File Explorer**.

2. In the **Console** section (under the **File Editor**), click **dbt Copilot** to view the available AI options.

3. Select the available options to generate the YAML config: **Generate Documentation**, **Generate Tests**, **Generate Semantic Model**, or **Generate Metrics**. To generate multiple YAML configs for the same model, click each option separately. dbt Copilot intelligently saves the YAML config in the same file.

   note

   [dbt Copilot](https://docs.getdbt.com/docs/cloud/dbt-copilot.md) doesn't yet support generating semantic models with the latest YAML spec.

   * To generate metrics, you need to first have semantic models defined.
   * Once defined, click **dbt Copilot** and select **Generate Metrics**.
   * Write a prompt describing the metrics you want to generate and press enter.
   * **Accept** or **Reject** the generated code.

4. Verify the AI-generated code. You can update or fix the code as needed.

5. Click **Save As**. You should see the file changes under the **Version control** section.

[![Example of using dbt Copilot to generate documentation in the IDE](/img/docs/dbt-cloud/cloud-ide/dbt-copilot-doc.gif?v=2 "Example of using dbt Copilot to generate documentation in the IDE")](#)Example of using dbt Copilot to generate documentation in the IDE

## Generate and edit code[​](#generate-and-edit-code "Direct link to Generate and edit code")

Copilot also allows you to generate SQL code directly within the SQL file in the [Studio IDE](https://docs.getdbt.com/docs/cloud/studio-ide/develop-in-studio.md), using natural language prompts. This means you can rewrite or add specific portions of the SQL file without needing to edit the entire file.

This intelligent AI tool streamlines SQL development by reducing errors, scaling effortlessly with complexity, and saving valuable time. Copilot's [prompt window](#use-the-prompt-window), accessible by keyboard shortcut, handles repetitive or complex SQL generation effortlessly so you can focus on high-level tasks.

Use Copilot's prompt window for use cases like:

* Writing advanced transformations
* Performing bulk edits efficiently
* Crafting complex patterns like regex

### Use the prompt window[​](#use-the-prompt-window "Direct link to Use the prompt window")

Access Copilot's AI prompt window using the keyboard shortcut Cmd+B (Mac) or Ctrl+B (Windows) to:

#### 1. Generate SQL from scratch[​](#1-generate-sql-from-scratch "Direct link to 1. Generate SQL from scratch")

* Use the keyboard shortcuts Cmd+B (Mac) or Ctrl+B (Windows) to generate SQL from scratch.
* Enter your instructions to generate SQL code tailored to your needs using natural language.
* Ask Copilot to fix the code or add a specific portion of the SQL file.

[![dbt Copilot's prompt window accessible by keyboard shortcut Cmd+B (Mac) or Ctrl+B (Windows)](/img/docs/dbt-cloud/cloud-ide/copilot-sql-generation-prompt.png?v=2 "dbt Copilot's prompt window accessible by keyboard shortcut Cmd+B (Mac) or Ctrl+B (Windows)")](#)dbt Copilot's prompt window accessible by keyboard shortcut Cmd+B (Mac) or Ctrl+B (Windows)

#### 2. Edit existing SQL code[​](#2-edit-existing-sql-code "Direct link to 2. Edit existing SQL code")

* Highlight a section of SQL code and press Cmd+B (Mac) or Ctrl+B (Windows) to open the prompt window for editing.
* Use this to refine or modify specific code snippets based on your needs.
* Ask Copilot to fix the code or add a specific portion of the SQL file.

#### 3. Review changes with the diff view to quickly assess the impact of the changes before making changes[​](#3-review-changes-with-the-diff-view-to-quickly-assess-the-impact-of-the-changes-before-making-changes "Direct link to 3. Review changes with the diff view to quickly assess the impact of the changes before making changes")

* When a suggestion is generated, Copilot displays a visual "diff" view to help you compare the proposed changes with your existing code:

  <!-- -->

  * **Green**: Means new code that will be added if you accept the suggestion.
  * **Red**: Highlights existing code that will be removed or replaced by the suggested changes.

#### 4. Accept or reject suggestions[​](#4-accept-or-reject-suggestions "Direct link to 4. Accept or reject suggestions")

* **Accept**: If the generated SQL meets your requirements, click the **Accept** button to apply the changes directly to your `.sql` file directly in the IDE.
* **Reject**: If the suggestion don’t align with your request/prompt, click **Reject** to discard the generated SQL without making changes and start again.

#### 5. Regenerate code[​](#5-regenerate-code "Direct link to 5. Regenerate code")

* To regenerate, press the **Escape** button on your keyboard (or click the Reject button in the popup). This will remove the generated code and puts your cursor back into the prompt text area.
* Update your prompt and press **Enter** to try another generation. Press **Escape** again to close the popover entirely.

Once you've accepted a suggestion, you can continue to use the prompt window to generate additional SQL code and commit your changes to the branch.

[![Edit existing SQL code using dbt Copilot's prompt window accessible by keyboard shortcut Cmd+B (Mac) or Ctrl+B (Windows)](/img/docs/dbt-cloud/cloud-ide/copilot-sql-generation.gif?v=2 "Edit existing SQL code using dbt Copilot's prompt window accessible by keyboard shortcut Cmd+B (Mac) or Ctrl+B (Windows)")](#)Edit existing SQL code using dbt Copilot's prompt window accessible by keyboard shortcut Cmd+B (Mac) or Ctrl+B (Windows)

## Was this page helpful?

YesNo

[Privacy policy](https://www.getdbt.com/cloud/privacy-policy)[Create a GitHub issue](https://github.com/dbt-labs/docs.getdbt.com/issues)

This site is protected by reCAPTCHA and the Google [Privacy Policy](https://policies.google.com/privacy) and [Terms of Service](https://policies.google.com/terms) apply.
