# Semantic structure

tip

Note that this best practices guide doesn't yet use the [new YAML specification](https://docs.getdbt.com/docs/build/latest-metrics-spec.md). We're working on updating this guide to use the new spec and file structure soon!

To read more about the new spec, see [Creating metrics](https://docs.getdbt.com/docs/build/metrics-overview.md).

## Files and Folders[​](#files-and-folders "Direct link to Files and Folders")

The first thing you need to establish is how you’re going to consistently structure your code. There are two recommend best practices to choose from:

* 🏡 **Co-locate your semantic layer code** in a one-YAML-file-per-marts-model system.

  <!-- -->

  * Puts documentation, data tests, unit tests, semantic models, and metrics into a unified file that corresponds to a dbt-modeled mart.
  * Trades larger file size for less clicking between files.
  * Simpler for greenfield projects that are building the Semantic Layer alongside dbt models.

* 🏘️**Create a sub-folder** called `models/semantic_models/`.

  <!-- -->

  * Create a parallel file and folder structure within that specifically for semantic layer code.
  * Gives you more targeted files, but may involves switching between files more often.
  * Better for migrating large existing projects, as you can quickly see what marts have been codified into the Semantic Layer.

It’s not terribly difficult to shift between these (it can be done with some relatively straightforward shell scripting), and this is purely a decision based on your developers’ preference (i.e. it has no impact on execution or performance), so don’t feel locked in to either path. Just pick the one that feels right and you can always shift down the road if you change your mind.

tip

Make sure to save all semantic models and metrics under the directory defined in the [`model-paths`](https://docs.getdbt.com/reference/project-configs/model-paths.md) (or a subdirectory of it, like `models/semantic_models/`). If you save them outside of this path, it will result in an empty `semantic_manifest.json` file, and your semantic models or metrics won't be recognized.

## Naming[​](#naming "Direct link to Naming")

Next, establish your system for consistent file naming:

* 1️⃣ If you’re doing **one-YAML-file-per-mart** then you’d have an `orders.sql` and an `orders.yml`.
* 📛 If you’re using a **parallel subfolder approach**, for the sake of unique file names it’s recommended to use the **prefix `sem_` e.g. `sem_orders.yml`** for the dedicated semantic model and metrics that build on `orders.sql` and `orders.yml`.

## Can't decide?[​](#cant-decide "Direct link to Can't decide?")

Start with a dedicated subfolder for your semantic models and metrics, and then if you find that you’re spending a lot of time clicking between files, you can always shift to a one-YAML-file-per-mart system. Our internal data team has found that the dedicated subfolder approach is more manageable for migrating existing projects, and this is the approach our documentation uses, so if you can't pick go with that.

## Was this page helpful?

YesNo

[Privacy policy](https://www.getdbt.com/cloud/privacy-policy)[Create a GitHub issue](https://github.com/dbt-labs/docs.getdbt.com/issues)

This site is protected by reCAPTCHA and the Google [Privacy Policy](https://policies.google.com/privacy) and [Terms of Service](https://policies.google.com/terms) apply.
