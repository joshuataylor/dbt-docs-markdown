# Copilot style guide

This guide provides an overview of the Copilot `dbt-styleguide.md` file, outlining its structure, recommended usage, and best practices for effective implementation in your dbt projects.

The `dbt-styleguide.md` is a template for creating a style guide for dbt projects. It includes:

* SQL style guidelines (for example, using lowercase keywords and trailing commas)
* Model organization and naming conventions
* Model configurations and testing practices
* Recommendations for using pre-commit hooks to enforce style rules

This guide helps ensure consistency and clarity in dbt projects.

## `dbt-styleguide.md` for Copilot[​](#dbt-styleguidemd-for-copilot "Direct link to dbt-styleguidemd-for-copilot")

Using Copilot in the Studio IDE, you can automatically generate a style guide template called `dbt-styleguide.md`. If the style guide is manually added or edited, it must also follow this naming convention. Any other file name cannot be used with Copilot.

Add the `dbt-styleguide.md` file to the root of your project. Copilot will use it as context for the large language model (LLM) when generating [data tests](https://docs.getdbt.com/docs/build/data-tests.md), [metrics](https://docs.getdbt.com/docs/build/metrics-overview.md), [semantic models](https://docs.getdbt.com/docs/build/semantic-models.md), and [documentation](https://docs.getdbt.com/docs/build/documentation.md).

Note, by creating a `dbt-styleguide.md` for Copilot, you are overriding dbt's default style guide.

## Creating `dbt-styleguide.md` in the Studio IDE[​](#creating-dbt-styleguidemd-in-the-studio-ide "Direct link to creating-dbt-styleguidemd-in-the-studio-ide")

1. Open a file in the Studio IDE.
2. Click **Copilot** in the toolbar.
3. Select **Generate ... Style guide** from the menu.

[![Generate styleguide in Copilot](/img/docs/dbt-cloud/generate-styleguide.png?v=2 "Generate styleguide in Copilot")](#)Generate styleguide in Copilot

4. The style guide template appears in the Studio IDE. Click **Save**. `dbt-styleguide.md` is added at the root level of your project.

If you haven't previously generated a style guide file, the latest version will be automatically sourced from dbt platform.

## If `dbt-styleguide.md` already exists[​](#if-dbt-styleguidemd-already-exists "Direct link to if-dbt-styleguidemd-already-exists")

If there is an existing `dbt-styleguide.md` file and you attempt to generate a new style guide, a modal appears with the following options:

* **Cancel** — Exit without making changes.
* **Restore** — Revert to the latest version from dbt platform.
* **Edit** — Modify the existing style guide manually.

[![Styleguide exists](/img/docs/dbt-cloud/styleguide-exists.png?v=2 "Styleguide exists")](#)Styleguide exists

## Further reading[​](#further-reading "Direct link to Further reading")

* [About dbt Copilot](https://docs.getdbt.com/docs/cloud/dbt-copilot.md)
* [How we style our dbt projects](https://docs.getdbt.com/best-practices/how-we-style/0-how-we-style-our-dbt-projects.md)

## Was this page helpful?

YesNo

[Privacy policy](https://www.getdbt.com/cloud/privacy-policy)[Create a GitHub issue](https://github.com/dbt-labs/docs.getdbt.com/issues)

This site is protected by reCAPTCHA and the Google [Privacy Policy](https://policies.google.com/privacy) and [Terms of Service](https://policies.google.com/terms) apply.
