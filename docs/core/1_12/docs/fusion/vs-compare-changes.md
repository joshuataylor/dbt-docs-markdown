# Compare changes in local development [Beta](https://docs.getdbt.com/docs/dbt-versions/product-lifecycles "Go to https://docs.getdbt.com/docs/dbt-versions/product-lifecycles")

info

This feature is in beta. All dbt VS Code extension users can use compare changes during the 14-day trial. After the trial, [sign in or register](https://docs.getdbt.com/docs/sign-in-dbt-extension.md) for a dbt platform account to keep using it.

The dbt VS Code extension previews and compares how your local edits affect your data in your dbt platform account — including added/removed rows and join verification — without waiting on CI.

Use compare changes to check impact early and validate changes before you open a PR or run a [CI job](https://docs.getdbt.com/docs/deploy/ci-jobs.md):

* Validate outputs are correct when refactoring logic, adding or removing columns, or implementing join modifications.
* It compares your current working copy against your `manifest.json` (for example, your last production state) and shows changes to primary keys, rows, and columns in the **Compare** tab.

The dbt VS Code extension's compare changes feature is different from the [Advanced CI compare changes feature](#how-this-differs-from-advanced-ci), which runs at the PR stage in deployment rather than locally during development.

info

Compare changes in development is available for models only. Support for seeds, snapshots, ephemeral models, and Python models is coming soon.

[![Make changes to a model and see the changes in the Compare tab](/img/docs/extension/vs-compare-changes.png?v=2 "Make changes to a model and see the changes in the Compare tab")](#)Make changes to a model and see the changes in the Compare tab

## Prerequisites[​](#prerequisites "Direct link to Prerequisites")

To use the dbt VS Code extension compare changes feature, you need:

* A Fusion [supported data platform](https://docs.getdbt.com/docs/fusion/supported-features.md?version=2.0#requirements) (BigQuery, Databricks, Redshift, or Snowflake)
* The [dbt VS Code extension](https://docs.getdbt.com/docs/install-dbt-extension.md) installed with a local installation of the [dbt Fusion engine](https://docs.getdbt.com/docs/fusion/get-started-fusion.md)
* A baseline state to compare your changes against — refer to [How it works](#how-it-works) to choose between [automatic deferral](https://docs.getdbt.com/docs/platform/about-defer.md) or [`manifest.json`](https://docs.getdbt.com/reference/artifacts/manifest-json.md?version=2.0) manual setup.

**Sign-in and authentication**

Compare changes is available to all dbt VS Code extension users during the 14-day trial. After the trial:

* **If you use dbt platform deferral (recommended):** [Sign in or register](https://docs.getdbt.com/docs/sign-in-dbt-extension.md) for a free dbt platform account. You'll also need a [`dbt_cloud.yml`](https://docs.getdbt.com/reference/dbt_cloud.yml.md) file in your local `.dbt` directory (`~/.dbt/dbt_cloud.yml` on macOS/Linux) so the extension can fetch the deferred manifest from your environment.
* **If you set the baseline manually with a `manifest.json`:** No dbt platform account is required. Point the extension at a local `manifest.json` and compare changes runs against it.

## How it works[​](#how-it-works "Direct link to How it works")

Compare changes in development works by comparing two materialized models in your warehouse. Specifically, it compares the model built in your dev schema (determined by your active profile) against the model referenced in your `manifest.json` (for example, your last production state). Both sides of the comparison are always warehouse tables; it does not compare SQL file contents.

The path you choose determines whether you need a dbt platform account:

* **dbt platform deferral (recommended):** Requires a dbt platform account and at least one successful job run in the environment you're deferring to (usually staging or production). Fusion auto-downloads the deferred manifest and uses it as your baseline state.
* **Manual `state` directory:** Point the extension at a local `manifest.json` (for example, copied from another environment). No dbt platform account is required; no job run is required.
  <br />

## Use compare changes[​](#use-compare-changes "Direct link to Use compare changes")

To use compare changes in development, follow these steps:

1. Open a SQL model file in your editor.

2. Make some changes to the model, like adding a new column or modifying an existing one (for example, `left_join_customers` to `right_join_customers`).

3. Run compare changes in the [VS Code interface](https://code.visualstudio.com/docs/getstarted/userinterface#_command-palette):

   <!-- -->

   * Command palette: Open the VS Code command palette (Shift + Command + P (Mac) / Ctrl + Shift + P (Windows/Linux)) and search for the [**dbt: Compare changes**](vscode://dbtLabsInc/dbt.compareModel)
   * Bottom panel: Click the **Compare** tab and then click the **Compare** button.

   [![Compare changes in development](/img/docs/extension/vs-compare-changes-options.png?v=2 "Compare changes in development")](#)Compare changes in development

4. Once you click the **Compare** button, the extension will execute a `dbt build` command to build the model you're working on and then runs the comparison. The [**Compare** tab](#compare-tab) displays the changes to the data's primary keys, rows, and columns. Clicking the tabs will display more details about the changes, like specific columns that were added or modified.

5. Once you've compared changes and see the changes in the **Compare** tab, you can then decide to commit your changes or continue editing.

## Compare tab results[​](#compare-tab-results "Direct link to Compare tab results")

The **Compare** tab displays the changes to the data's primary keys, rows, and columns. Clicking the tabs will display more details about the changes, like specific columns that were added or modified.

[![Example of the Compare tab](/img/docs/extension/vs-compare-changes-tab.png?v=2 "Example of the Compare tab")](#)Example of the Compare tab

* **Overview tab**: High-level summary about the changes to the models, such as the number of primary keys that were added or removed, rows modified, and so on. It will also include the relation between models that were added or modified.
* **Primary keys tab**: Details about the changes to the records.
* **Modified rows tab**: Details about the modified rows.
* **Columns tab**: Details about the changes to the columns.

## FAQs[​](#faqs "Direct link to FAQs")

 Are queries run on behalf of the developer?

Yes. All comparison queries in development run using your local development credentials, directly from the dbt VS Code extension. If you authenticated with a [`dbt_cloud.yml`](https://docs.getdbt.com/reference/dbt_cloud.yml.md) file, that's the credential the extension uses; otherwise it uses the credentials from your active dbt profile. The dbt Fusion engine uses your credentials to execute comparison queries in your warehouse. The results are stored in memory, so that we can keep them populated into the Compare tab for that file until you re-run.

 Is this using my warehouse credits?

Yes. Because the comparison runs in your development environment using your dev credentials, it will use your warehouse’s compute.

 Do I need to run dbt build every time I make a change?

No. When you click **Compare**, the extension builds the model into your development schema automatically, so you don’t need to run `dbt build` yourself. It then compares that development build against the version in your deferred environment (usually staging or production).

If the model hasn’t been built yet in your deferred environment, the comparison can’t run. For more details, see [Defer to production](https://docs.getdbt.com/docs/platform/about-defer.md).

 How is this different from Advanced CI compare changes?

The dbt VS Code extension's compare changes feature applies only to your local development environment. If you're looking to compare changes between your production environment and the pull request's latest commit, check out [Advanced CI compare changes](https://docs.getdbt.com/docs/deploy/advanced-ci.md#compare-changes).

| Aspect              | In development (compare changes)                                              | In deployment (Advanced CI)                                                                                     |
| ------------------- | ----------------------------------------------------------------------------- | --------------------------------------------------------------------------------------------------------------- |
| **Affects**         | Development for one modified model at a time                                  | Deployment for all modified models in a project                                                                 |
| **Trigger**         | On-demand in editor                                                           | PR open/update and CI job                                                                                       |
| **Scope**           | Your working copy and local target                                            | Branch head versus prod state in CI                                                                             |
| **Output** location | Compare panel in VS Code/Cursor. Does not create a PR comment in Git provider | Deployment job compare tab and PR summary comment in Git provider                                               |
| **Data caching**    | Editor-side                                                                   | dbt platform [caches](https://docs.getdbt.com/docs/deploy/advanced-ci.md#about-the-cached-data) limited samples |
| **Governance**      | Local development credentials                                                 | Production credentials                                                                                          |

## Related docs[​](#related-docs "Direct link to Related docs")

* [Advanced CI compare changes](https://docs.getdbt.com/docs/deploy/advanced-ci.md#compare-changes)
