# dbt VS Code extension features [Preview](https://docs.getdbt.com/docs/dbt-versions/product-lifecycles "Go to https://docs.getdbt.com/docs/dbt-versions/product-lifecycles")

Local developmentⓘ

The dbt VS Code extension requires the dbt Fusion engine and uses a dynamic Language Server Protocol (LSP) to provide a fast, intelligent, and cost-efficient dbt development experience with enhanced workflows and easy navigation.

Registration for advanced features

All dbt VS Code extension features are available to all users for 14 days. After the 14-day trial period, most features remain available without registration. To keep using advanced features, [register](https://docs.getdbt.com/docs/sign-in-dbt-extension.md) for a free dbt platform account. Existing registered users keep access automatically.

See the [feature availability](#feature-availability) table for the full list of features and what each needs.

In dbt v2.0 and later, authentication for registration and features that require sign-in is handled by [`dbt login`](https://docs.getdbt.com/reference/commands/login.md), so your login state is shared across dbt tools like the dbt VS Code extension and, in supported versions, dbt State.

<!-- -->

## Feature availability[​](#feature-availability "Direct link to Feature availability")

The dbt VS Code extension is free to install. All features work for 14 days with no login; after that, the vast majority keep working, and a few advanced features need a free dbt platform account (sign in or register with your email, or run `dbt login`).

| Feature                                                    | Works without login/registration | Register or login<br />Any dbt platform account, free or paid |
| ---------------------------------------------------------- | -------------------------------- | ------------------------------------------------------------- |
| Error diagnostics for Jinja, YAML, and SQL syntax          | ✅                               | ✅                                                            |
| Jinja LSP go-to ref, source, and macro                     | ✅                               | ✅                                                            |
| Linter warning diagnostics                                 | ✅                               | ✅                                                            |
| Table-level lineage                                        | ✅                               | ✅                                                            |
| Basic dbt command UI (run, build, test, and query results) | ✅                               | ✅                                                            |
| Ref autocomplete                                           | ✅                               | ✅                                                            |
| Refactor ref and column names                              | ✅                               | ✅                                                            |
| Dialect-aware function autocomplete                        | ✅                               | ✅                                                            |
| SQL type and schema error diagnostics                      | -                                | ✅                                                            |
| Preview CTE                                                | -                                | ✅                                                            |
| Query cache for faster incremental compiles                | -                                | ✅                                                            |
| Model docs tab with platform metadata                      | -                                | ✅                                                            |
| Column-level lineage                                       | -                                | ✅                                                            |
| Compare changes                                            | -                                | ✅                                                            |
| SQL LSP go-to column and CTE                               | -                                | ✅                                                            |
| SQL LSP hover to see the schema for `select *`             | -                                | ✅                                                            |

Search table...

|                  |   |   |   |   |
| ---------------- | - | - | - | - |
| Loading table... |   |   |   |   |

## Lightning-fast parse times[​](#lightning-fast-parse-times "Direct link to Lightning-fast parse times")

Parse even the largest projects up to 30x faster than with dbt Core.

**Requires registration** — LSP query cache (for faster incremental compiles).

[](/img/docs/extension/zoomzoom.mp4)

## View compiled code[​](#view-compiled-code "Direct link to View compiled code")

Get a live view of the SQL code your models will build — right alongside your dbt code.

Usage:

* Click the **code icon** to view compiled code side-by-side with source code.
* Compiled code will update as you save your source code.
* Clicking on a dbt macro will focus the corresponding compiled code.
* Clicking on a compiled code block will focus the corresponding source code.

[](/img/docs/extension/compiled-code.mp4)

## Build flexibly[​](#build-flexibly "Direct link to Build flexibly")

Use the command palette to quickly build models using complex selectors.

Usage:

* Click the **dbt icon** or use keyboard shortcut `cmd+shift+enter` (macOS) / `ctrl+shift+enter` (Windows/Linux) to launch a quickpick menu.
* Select a command to run.

[](/img/docs/extension/build-flexibly.mp4)

## Live error detection[​](#live-error-detection "Direct link to Live error detection")

Automatically validate your SQL code to detect errors and surface warnings without hitting the warehouse.

**Available to all users:**

Syntax-tree diagnostics for Jinja, YAML, and SQL syntax errors (L1):

* Syntax errors (missing commas, misspelled keywords, and more)
* Hover over red squiggles to display errors
* Full diagnostic information is available in the **Problems** panel

**Requires registration:**

L2 Fusion SQL comprehension diagnostics (depends on strict static analysis):

* Missing `group by` clauses, or columns that are neither grouped nor aggregated
* Invalid function names or arguments
* SQL type and schema errors
* Linter warning diagnostics

[](/img/docs/extension/live-error-detection.mp4)

## Powerful IntelliSense[​](#powerful-intellisense "Direct link to Powerful IntelliSense")

Autocomplete SQL functions, model names, macros, and more.

**Available to all users:**

* Autocomplete `ref`s and `source` calls. For example, type `{{ ref(` or `{{ source(` and you will see a list of available resources and their type complete the function call. Autocomplete doesn't trigger when replacing existing model names inside parentheses.
* Dialect-aware SQL function autocomplete

[![Example of the VS Code extension IntelliSense](/img/docs/extension/vsce-intellisense.gif?v=2 "Example of the VS Code extension IntelliSense")](#)Example of the VS Code extension IntelliSense

## Instant refactoring[​](#instant-refactoring "Direct link to Instant refactoring")

Rename models or columns and see references update project-wide.

**Available to all users:**

Renaming models:

* Right-click on a file in the file tree and select **Rename**.
* After renaming the file, you'll get a prompt asking if you want to make refactoring changes.
  <!-- -->
  * Select **OK** to apply the changes, or **Show Preview** to display a preview of refactorings.
* After applying your changes, `ref`s should be updated to use the updated model name.

**Requires registration:**

Renaming columns (depends on strict static analysis):

Column renaming depends on strict static analysis, which validates column references across your project before the extension updates downstream models.

* Right-click on a column alias and select **Rename Symbol**.
* After renaming the column, you'll get a prompt asking if you want to make refactoring changes.
  <!-- -->
  * Select **OK** to apply the changes, or **Show Preview** to show a preview of refactorings.
* After applying your changes, downstream references to the column should be updated to use the new column name.

Note: Renaming models and columns is not yet supported for snapshots, or any resources defined in a .yml file.

[](/img/docs/extension/refactor.mp4)

## Go-to-definition and reference[​](#go-to-definition-and-reference "Direct link to Go-to-definition and reference")

Jump to the definition of any `ref`, macro, model, or column with a single click. Particularly useful in large projects with many models and macros. Excludes definitions from installed packages.

**Available to all users:**

* Command or Ctrl-click to go to the definition for an identifier.
* Right-click an identifier and select **Go to Definition** or **Go to References**.
* Jinja LSP go-to-definition for `ref()`, `source()`, and macros.

**Requires registration:**

Column and CTE go-to-definition (depends on `strict` static analysis):

* Go-to-definition for column names
* Go-to-definition for CTE names

[](/img/docs/extension/go-to-definition.mp4)

## Rich lineage in context[​](#rich-lineage-in-context "Direct link to Rich lineage in context")

See lineage at the column or table level as you develop — no context switching or breaking flow.

**Available to all users:**

Table-level lineage:

Using the lineage tab in Cursor

If you're using the dbt VS Code extension in Cursor, the lineage tab works best in Editor mode and doesn't render in Agent mode. If you're in Agent mode and the lineage tab isn't rendering, just switch to Editor mode to view your project's table and column lineage.

View table lineage:

* Open the **Lineage** tab in your editor. It will reflect table lineage focused on the currently-open file.
* Double-click nodes to open the files in your editor.
* The lineage pane updates as you navigate the files in your dbt project.
* Right-click on a node to update the DAG, or view column lineage for a node.

**Requires registration:**

Column-level lineage (depends on strict static analysis):

View column lineage:

* Right-click on a filename, or in the SQL contents of a model file.
* Select **dbt: View Lineage** --> **Show column lineage**.
* Select the column to view lineage for.
* Double-click on a node to update the DAG selector.
* You can also use column selectors in the lineage window by adding the `column:` prefix and appending the column name.

[](/img/docs/extension/lineage.mp4)

## Hover insights[​](#hover-insights "Direct link to Hover insights")

See context on tables, columns, and functions without leaving your code. Simply hover over any SQL element to see details like column names and data types.

**Requires registration:**

Hover insights depend on strict static analysis, which lets the extension understand column types and function signatures across your project.

Usage:

* Hover over `*` to see expanded list of columns and their types.
* Hover over column name or alias to see its type.

[](/img/docs/extension/hover-insights.mp4)

## Live preview for models and CTEs[​](#live-preview-for-models-and-ctes "Direct link to Live preview for models and CTEs")

Preview query output directly from inside your editor for faster validation and debugging.

**Available to all users:**

* Click the **table icon** or use keyboard shortcut `cmd+enter` (macOS) / `ctrl+enter` (Windows/Linux) to preview query results for a model or selected SQL snippet.
* Results are displayed in the **Query Results** tab in the bottom panel.
* The preview table is sortable and results are stored until the tab is closed.

**Requires registration:**

CTE preview:

* Click the **Preview CTE** codelens to preview CTE results.

[](/img/docs/extension/preview-cte.mp4)

## Explore your catalog [Beta](https://docs.getdbt.com/docs/dbt-versions/product-lifecycles "Go to https://docs.getdbt.com/docs/dbt-versions/product-lifecycles")[​](#explore-your-catalog- "Direct link to explore-your-catalog-")

**Requires registration:**

Open the **Catalog** tab to see information for the model you're working on — enriched by your dbt platform account — without leaving your editor.

For the current model, the catalog tab surfaces:

* The build status, last build time, and run duration from the dbt platform.
* The model's **Description**.
* The model's **Columns**, including each column's type, description, and test results. Sort columns alphabetically or by test name.
* A **View in dbt platform** link to open the resource in the dbt platform.

The **Catalog** tab is an advanced feature. Before your 14-day trial expires, [register](https://docs.getdbt.com/docs/sign-in-dbt-extension.md) for a dbt platform account to continue using it.

[![Example of the Catalog tab in the dbt VS Code extension](/img/docs/extension/vsce-catalog-tab.png?v=2 "Example of the Catalog tab in the dbt VS Code extension")](#)Example of the Catalog tab in the dbt VS Code extension

## Generate a system report[​](#generate-a-system-report "Direct link to Generate a system report")

Generate a system report to collect your VS Code extension logs and system information into a zip file. This is useful when troubleshooting issues with the dbt VS Code extension. You can share the zip file with dbt Labs support to help diagnose problems.

To generate and download a system report:

1. Open the Command Palette (`Cmd+Shift+P` on macOS, `Ctrl+Shift+P` on Windows/Linux).
2. Search for and select **dbt: Generate System Report**.
3. Choose a location to save the .zip file when prompted.
4. A notification will confirm where the file was saved.

<!-- -->

## Compare changes in development [Beta](https://docs.getdbt.com/docs/dbt-versions/product-lifecycles "Go to https://docs.getdbt.com/docs/dbt-versions/product-lifecycles")[Enterprise](https://www.getdbt.com/pricing "Go to https://www.getdbt.com/pricing")[Enterprise +](https://www.getdbt.com/pricing "Go to https://www.getdbt.com/pricing")[​](#compare-changes-in-development- "Direct link to compare-changes-in-development-")

Requires registration

Advanced capabilities are available to all users for 14 days. After the 14-day trial, [sign in or register](https://docs.getdbt.com/docs/sign-in-dbt-extension.md) for a dbt platform account to keep using advanced features. Existing registered dbt VS Code extension users keep access automatically.

Authentication is handled by [`dbt login`](https://docs.getdbt.com/reference/commands/login.md), so your login state is shared across the CLI, dbt VS Code extension, and <!-- -->.

You can use compare changes, powered by the dbt Fusion engine, in your local development environment to compare your current working copy against your `manifest.json` (for example, your last production state) directly in your editor.

For more details on how to use this feature, refer to [Compare changes in local development](https://docs.getdbt.com/docs/fusion/vs-compare-changes.md).

[![Example of the Compare tab](/img/docs/extension/vs-compare-changes.png?v=2 "Example of the Compare tab")](#)Example of the Compare tab
