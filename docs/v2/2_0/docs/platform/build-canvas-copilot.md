# Build with dbt Copilot [Enterprise](https://www.getdbt.com/pricing "Go to https://www.getdbt.com/pricing")[Enterprise +](https://www.getdbt.com/pricing "Go to https://www.getdbt.com/pricing")

Use dbt Copilot to build visual models in the Canvas with natural language prompts.

dbt Copilot integrates with [Canvas](https://docs.getdbt.com/docs/platform/canvas.md), a drag-and-drop experience for building visual models with natural language prompts.

## Considerations[​](#considerations "Direct link to Considerations")

dbt Copilot in Canvas has setup and AI requirements. Confirm the following before you begin.

* dbt Copilot is available in the Canvas interface. Refer to [About Canvas](https://docs.getdbt.com/docs/platform/canvas.md) for setup instructions.
* Natural language prompts in Canvas are available on [Enterprise and Enterprise+](https://www.getdbt.com/pricing) plans.
* dbt Copilot requires AI features to be [enabled](https://docs.getdbt.com/docs/platform/enable-dbt-ai.md) for your account. AI terms and conditions apply. Refer to [dbt AI FAQs](https://docs.getdbt.com/docs/dbt-ai/dbt-ai-faqs.md#does-dbt-labs-have-terms-in-place) for details.
* Your development environment must be on a supported [release track](https://docs.getdbt.com/docs/dbt-versions/dbt-release-tracks.md).
* Refer to [Billing](https://docs.getdbt.com/docs/platform/billing.md) for dbt Copilot usage limits.

<!-- -->

info

dbt Wizard is the new and recommended AI agent for governed data development in dbt. It handles the full development lifecycle — investigation, building, validation, and shipping — grounded in your dbt project's lineage, tests, contracts, and metric definitions.

dbt Copilot is separate from dbt Wizard and is dbt's inline AI assistance experience, providing single-click generation of SQL, documentation, tests, and semantic models in Studio IDE, Canvas, and Insights.

Refer to [dbt AI FAQs](https://docs.getdbt.com/docs/dbt-ai/dbt-ai-faqs.md#is-dbt-wizard-the-same-as-dbt-copilot), [Billing](https://docs.getdbt.com/docs/platform/billing.md), and [dbt's Terms of Use](https://www.getdbt.com/terms-of-use) for more information.

To begin building models with natural language prompts in the Canvas:

1. Click on the **dbt Copilot** icon in Canvas menu.

2. In the dbt Copilot prompt box, enter your prompt in natural language for dbt Copilot to build the model(s) you want. You can also reference existing models using the `@` symbol. For example, to build a model that calculates the total price of orders, you can enter `@orders` in the prompt and it'll pull in and reference the `orders` model.

3. Click **Generate** and dbt Copilot generates a summary of the model(s) you want to build.

   <!-- -->

   * To start over, click on the **+** icon. To close the prompt box, click **X**.

   [![Enter a prompt in the dbt Copilot prompt box to build models using natural language](/img/docs/dbt-platform/copilot-generate.jpg?v=2 "Enter a prompt in the dbt Copilot prompt box to build models using natural language")](#)Enter a prompt in the dbt Copilot prompt box to build models using natural language

4. Click **Apply** to generate the model(s) in the Canvas.

5. dbt Copilot displays a visual "diff" view to help you compare the proposed changes with your existing code. Review the diff view in the canvas to see the generated operators built by dbt Copilot:

   <!-- -->

   * White: Located in the top of the canvas and means existing set up or blank canvas that will be removed or replaced by the suggested changes.
   * Green: Located in the bottom of the canvas and means new code that will be added if you accept the suggestion.
     <br />

   [![Visual diff view of proposed changes](/img/docs/dbt-platform/copilot-diff.jpg?v=2 "Visual diff view of proposed changes")](#)Visual diff view of proposed changes

6. Reject or accept the suggestions

7. In the **generated** operator box, click the play icon to preview the data

8. Confirm the results or continue building your model.
   <!-- -->
   [![Use the generated operator with play icon to preview the data](/img/docs/dbt-platform/copilot-output.jpg?v=2 "Use the generated operator with play icon to preview the data")](#)Use the generated operator with play icon to preview the data

9. To edit the generated model, open **dbt Copilot** prompt box and type your edits.

10. Click **Submit** and dbt Copilot will generate the revised model. Repeat steps 5-8 until you're happy with the model.
