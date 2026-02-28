# About the dbt VS Code extension [Preview](https://docs.getdbt.com/docs/dbt-versions/product-lifecycles "Go to https://docs.getdbt.com/docs/dbt-versions/product-lifecycles")

The dbt VS Code extension brings a hyper-fast, intelligent, and cost-efficient dbt development experience to VS Code. This is the only way to enjoy all the power of the dbt Fusion engine while developing locally.

* *Save time and resources* with near-instant parsing, live error detection, powerful IntelliSense capabilities, and more.
* *Stay in flow* with a seamless, end-to-end dbt development experience designed from scratch for local dbt development.

The dbt VS Code extension is available in the [VS Code Marketplace](https://marketplace.visualstudio.com/items?itemName=dbtLabsInc.dbt). *Note, this is a public preview release. Behavior may change ahead of the broader generally available (GA) release.*

The dbt VS Code extension is only compatible with the dbt Fusion engine, but not with dbt Core.

Try out the Fusion quickstart guide

Check out the [Fusion quickstart guide](https://docs.getdbt.com/guides/fusion.md?step=1) to try the dbt VS Code extension in action.

## Navigating the dbt extension[​](#navigating-the-dbt-extension "Direct link to Navigating the dbt extension")

Once the dbt VS Code extension has been installed, several visual enhancements will be added to your IDE to help you navigate the features and functionality.

Check out the following video to see the features and functionality of the dbt VS Code extension:

[dbt Fusion + VS Code extension walkthrough](https://app.storylane.io/share/a1rkqx0mbd7a)

### The dbt extension menu[​](#the-dbt-extension-menu "Direct link to The dbt extension menu")

The dbt logo on the sidebar (or the **dbt Extension** text on the bottom tray) launches the main menu for the extension. This menu contains helpful information and actions you can take:

* **Get started button:** Launches the [Fusion upgrade](https://docs.getdbt.com/docs/install-dbt-extension.md#upgrade-to-fusion) workflow.
* **Extension info:** Information about the extension, Fusion, and your dbt project. Includes configuration options and actions.
* **Help:** Quick links to support, bug submissions, and documentation.

[![dbt VS Code extension welcome screen.](/img/docs/extension/sidebar-menu.png?v=2 "dbt VS Code extension welcome screen.")](#)dbt VS Code extension welcome screen.

### Caching[​](#caching "Direct link to Caching")

The dbt extension caches important schema information from your data warehouse to improve speed and performance. This will automatically update over time, but if recent changes have been made that aren't reflected in your project, you can manually update the schema information:

1. Click the **dbt logo** on the sidebar to open the menu.
2. Expand the **Extension info** section and location the **Actions** subsection.
3. Click **Clear Cache** to update.

### Productivity features[​](#productivity-features "Direct link to Productivity features")

This section has moved

We've moved productivity features to their own page! Check out their [new location](https://docs.getdbt.com/docs/dbt-extension-features.md).

## Using the extension[​](#using-the-extension "Direct link to Using the extension")

Your dbt environment must be using the dbt Fusion engine in order to use this extension. See [the Fusion documentation](https://docs.getdbt.com/docs/fusion.md) for more on eligibility and upgrading.

Once installed, the dbt extension automatically activates when you open any `.sql` or `.yml` file inside of a dbt project directory.

## Configuration[​](#configuration "Direct link to Configuration")

After installation, you may want to configure the extension to better fit your development workflow:

1. Open the VS Code settings by pressing `Ctrl+,` (Windows/Linux) or `Cmd+,` (Mac).
2. Search for `dbt`. On this page, you can adjust the extension’s configuration options to fit your needs.

[![dbt extension settings within the VS Code settings.](/img/docs/extension/dbt-extension-settings.png?v=2 "dbt extension settings within the VS Code settings.")](#)dbt extension settings within the VS Code settings.

## Known limitations[​](#known-limitations "Direct link to Known limitations")

The following are currently known limitations of the dbt extension:

* **Remote development:** The dbt extension does not yet support remote development sessions over SSH. Support will be added in a future release. For more information on remote development, refer to [Supporting Remote Development and GitHub Codespaces](https://code.visualstudio.com/api/advanced-topics/remote-extensions) and [Visual Studio Code Server](https://code.visualstudio.com/docs/remote/vscode-server).

* **Working with YAML files:** Today, the dbt extension has the following limitations with operating on YAML files:

  * Go-to-definition is not supported for nodes defined in YAML files (like snapshots).
  * Renaming models and columns will not update references in YAML files.
  * Future releases of the dbt extension will address these limitations.

* **Renaming models:** When you rename a model file, the dbt extension applies edits to update all `ref()` calls that reference the renamed model. Due to limitations of VS Code's Language Server Client, the extension can't auto-save these edited files. As a result, renaming a model file may cause compiler errors in your project. To fix these errors, either manually save each file that the dbt extension edited, or click **File** --> **Save All** to save all edited files.

* **Using Cursor's Agent mode:** When using the dbt extension in Cursor, lineage visualization works best in Editor mode and doesn't render in Agent mode. If you're working in Agent mode and need to view lineage, switch to Editor mode to access the full lineage tab functionality.

### Extension conflicts[​](#extension-conflicts "Direct link to Extension conflicts")

The extension may occasionally conflict with other VS Code extensions that provide similar services (such as code validation). You may need to disable these third-party extensions while working with the dbt extension.

**YAML by Red Hat:**

The YAML extension by Red Hat may erroneously flag some keys (such as `static_analysis`) in dbt YAML files as invalid in the IDE.

[![Static analysis erroneously tagged as invalid](/img/docs/extension/false-yaml-error.png?v=2 "Static analysis erroneously tagged as invalid")](#)Static analysis erroneously tagged as invalid

To solve this issue, do one of the following:

* (Recommended) Disable the Red Hat YAML extension while working with the dbt extension.

* Add the following configuration to your VS Code `settings.json` file:

  <!-- -->

  ```json
  "yaml.schemas": {
      "Core/dbtschema.json": "data/dbt/models/**/schema.yml",
      "": "data/dbt/dbt_project.yml"
  },
  ```

  <!-- -->

  This could disable *all* use of the schema store, resulting in unintended consequences.

## Support[​](#support "Direct link to Support")

dbt platform customers can contact dbt Labs support at <support@getdbt.com>. You can also get in touch with us by reaching out to your Account Manager directly.

For organizations that are not customers of the dbt platform, the best place for questions and discussion is the [dbt Community Slack](https://www.getdbt.com/community/join-the-community).

We welcome feedback as we work to continuously improve the extension, and would love to hear from you!

For more information regarding support and acceptable use of the dbt VS Code extension, refer to our [Acceptable Use Policy](https://www.getdbt.com/dbt-assets/vscode-plugin-aup).

<!-- -->

## More information about Fusion[​](#more-information-about-fusion "Direct link to More information about Fusion")

Fusion marks a significant update to dbt. While many of the workflows you've grown accustomed to remain unchanged, there are a lot of new ideas, and a lot of old ones going away. The following is a list of the full scope of our current release of the Fusion engine, including implementation, installation, deprecations, and limitations:

* [About the dbt Fusion engine](https://docs.getdbt.com/docs/fusion/about-fusion.md)
* [About the dbt extension](https://docs.getdbt.com/docs/about-dbt-extension.md)
* [New concepts in Fusion](https://docs.getdbt.com/docs/fusion/new-concepts.md)
* [Supported features matrix](https://docs.getdbt.com/docs/fusion/supported-features.md)
* [Installing Fusion CLI](https://docs.getdbt.com/docs/fusion/install-fusion-cli.md)
* [Installing VS Code extension](https://docs.getdbt.com/docs/install-dbt-extension.md)
* [Fusion release track](https://docs.getdbt.com/docs/dbt-versions/upgrade-dbt-version-in-cloud.md#dbt-fusion-engine)
* [Quickstart for Fusion](https://docs.getdbt.com/guides/fusion.md?step=1)
* [Upgrade guide](https://docs.getdbt.com/docs/dbt-versions/core-upgrade/upgrading-to-fusion.md)
* [Fusion licensing](http://www.getdbt.com/licenses-faq)

## Was this page helpful?

YesNo

[Privacy policy](https://www.getdbt.com/cloud/privacy-policy)[Create a GitHub issue](https://github.com/dbt-labs/docs.getdbt.com/issues)

This site is protected by reCAPTCHA and the Google [Privacy Policy](https://policies.google.com/privacy) and [Terms of Service](https://policies.google.com/terms) apply.
