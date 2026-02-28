# Build with dbt Copilot [Enterprise](https://www.getdbt.com/pricing "Go to https://www.getdbt.com/pricing")[Enterprise +](https://www.getdbt.com/pricing "Go to https://www.getdbt.com/pricing")

Use Copilot to build visual models in the Canvas with natural language prompts.

[Copilot](https://docs.getdbt.com/docs/cloud/dbt-copilot.md) seamlessly integrates with [Canvas](https://docs.getdbt.com/docs/cloud/canvas.md), a drag-and-drop experience that helps you with build your visual models using natural language prompts. Before you begin, make sure you can access [Canvas](https://docs.getdbt.com/docs/cloud/use-canvas.md#access-canvas).

<!-- -->

To begin building models with natural language prompts in the Canvas:

1. Click on the **dbt Copilot** icon in Canvas menu.

2. In the dbt Copilot prompt box, enter your prompt in natural language for Copilot to build the model(s) you want. You can also reference existing models using the `@` symbol. For example, to build a model that calculates the total price of orders, you can enter `@orders` in the prompt and it'll pull in and reference the `orders` model.

3. Click **Generate** and dbt Copilot generates a summary of the model(s) you want to build.

   <!-- -->

   * To start over, click on the **+** icon. To close the prompt box, click **X**.

   [![Enter a prompt in the dbt Copilot prompt box to build models using natural language](/img/docs/dbt-cloud/copilot-generate.jpg?v=2 "Enter a prompt in the dbt Copilot prompt box to build models using natural language")](#)Enter a prompt in the dbt Copilot prompt box to build models using natural language

4. Click **Apply** to generate the model(s) in the Canvas.

5. dbt Copilot displays a visual "diff" view to help you compare the proposed changes with your existing code. Review the diff view in the canvas to see the generated operators built byCopilot:

   <!-- -->

   * White: Located in the top of the canvas and means existing set up or blank canvas that will be removed or replaced by the suggested changes.
   * Green: Located in the bottom of the canvas and means new code that will be added if you accept the suggestion.
     <br />

   [![Visual diff view of proposed changes](/img/docs/dbt-cloud/copilot-diff.jpg?v=2 "Visual diff view of proposed changes")](#)Visual diff view of proposed changes

6. Reject or accept the suggestions

7. In the **generated** operator box, click the play icon to preview the data

8. Confirm the results or continue building your model.
   <!-- -->
   [![Use the generated operator with play icon to preview the data](/img/docs/dbt-cloud/copilot-output.jpg?v=2 "Use the generated operator with play icon to preview the data")](#)Use the generated operator with play icon to preview the data

9. To edit the generated model, open **Copilot** prompt box and type your edits.

10. Click **Submit** and Copilot will generate the revised model. Repeat steps 5-8 until you're happy with the model.

## Was this page helpful?

YesNo

[Privacy policy](https://www.getdbt.com/cloud/privacy-policy)[Create a GitHub issue](https://github.com/dbt-labs/docs.getdbt.com/issues)

This site is protected by reCAPTCHA and the Google [Privacy Policy](https://policies.google.com/privacy) and [Terms of Service](https://policies.google.com/terms) apply.
