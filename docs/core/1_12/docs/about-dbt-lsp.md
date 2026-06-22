# About dbt LSP

The dbt Fusion engine offers benefits beyond the speed and power of the framework. The dbt VS Code extension, Studio IDE, and Insights all contain a powerful set of features backed by our Language Server Protocol (LSP) that enable fast, efficient development workflows. The following features are supported across these tools:

<!-- -->

|                              | VS Code extension | Studio IDE | Insights |
| ---------------------------- | ----------------- | ---------- | -------- |
| Autocomplete function names  | ✅                | ✅         | ❌       |
| Autocomplete ref/source args | ✅                | ✅         | ✅       |
| CTE Preview                  | ✅                | ✅         | ✅       |
| Column-level lineage         | ✅                | ❌         | ❌       |
| Compare changes locally      | ✅\*              | ❌         | ❌       |
| Command palette              | ✅                | N/A        | ❌       |
| Error detection              | ✅                | ✅         | ✅       |
| Go-to definition             | ✅                | ✅         | ❌       |
| Go-to reference              | ✅                | ✅         | ❌       |
| Incremental compilation      | ✅                | ✅         | ❌       |
| Lazy compilation             | ✅                | ✅         | ❌       |
| Preview query results        | ✅                | N/A        | ❌       |
| Problems tab                 | ✅                | ✅         | ❌       |
| Propagate column renames     | ✅                | ❌         | ❌       |
| Propagate model renames      | ✅                | ❌         | ❌       |
| Show column type on hover    | ✅                | ✅         | ✅       |
| Show compiled SQL            | ✅                | ✅         | ❌       |
| View table lineage           | ✅                | N/A        | ❌       |
| Warning detection            | ✅                | ✅         | ❌       |

Search table...

|                  |   |   |   |   |
| ---------------- | - | - | - | - |
| Loading table... |   |   |   |   |

* "Compare changes locally" requires a [dbt Enterprise or Enterprise+](https://www.getdbt.com/pricing) account. *All other dbt VS Code extension LSP features listed above are available for free.*

## Lazy compilation[​](#lazy-compilation "Direct link to Lazy compilation")

The dbt language server uses on-demand compilation, also called lazy compilation. Lazy compilation starts automatically when you open a model file, you don't need to run `dbt compile` to trigger it. It compiles only the nodes it needs to answer questions about the file you are working in, instead of blocking on a full project compile first. That improves performance because you get editor features for your active file much sooner.

### What compiles first[​](#what-compiles-first "Direct link to What compiles first")

When you open or focus on a model, the server determines a minimal set of nodes to compile so it can produce up-to-date LSP results for that model. That set includes the current model and its upstream dependencies (ancestors in the DAG), because rendered SQL and analysis depend on `ref`, sources, and inherited context from parents.

Nodes you are not actively working on remain `not compiled` until the background compilation pass reaches them. How long that takes depends on the size of your project. Until a node is compiled, LSP results for that node are not available.

When you switch to another file, the server reuses results from any compilations that already finished. If a compilation was still in progress when you switched files, it is cancelled and that partial work is discarded; the server then schedules a fresh compile for the newly focused model and its dependencies.

### Background compilation[​](#background-compilation "Direct link to Background compilation")

After the minimal compile for your active file, the server continues with a background compile of the rest of the project. That pass fills in project-wide state without preventing you from using tooling on models that already finished compiling.

Background compilation enables full project analysis once it completes. Until then, some features that need the full graph may be limited. You can monitor compilation progress in your editor's status bar. When the progress notifications clear, the background compile is complete.

The Fusion CLI and the language server run independently. Running a command like `dbt run` or `dbt compile` from the terminal does not interrupt or affect LSP compilation.

## Was this page helpful?

YesNo

[Privacy policy](https://www.getdbt.com/cloud/privacy-policy)[Create a GitHub issue](https://github.com/dbt-labs/docs.getdbt.com/issues)

This site is protected by reCAPTCHA and the Google [Privacy Policy](https://policies.google.com/privacy) and [Terms of Service](https://policies.google.com/terms) apply.
