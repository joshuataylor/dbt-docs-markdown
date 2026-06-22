# Fix deprecation warnings

You can address deprecation warnings in the dbt platform by finding and fixing them using the autofix tool in the Studio IDE. You can run the autofix tool on the [Compatible or Latest release track](https://docs.getdbt.com/docs/dbt-versions/dbt-release-tracks.md) of dbt Core before you upgrade to Fusion!

To find and fix deprecations:

1. Navigate to the Studio IDE by clicking **Studio** in the left menu.

2. Make sure to save and commit your work before proceeding. The autofix tool may overwrite any unsaved changes.

3. Click the three-dot menu located at the bottom right corner of the Studio IDE.

4. Select **Check & fix deprecations**.

   [![Access the Studio IDE options menu to autofix deprecation warnings](/img/docs/dbt-platform/platform-ide/ide-options-menu-with-save.png?v=2 "Access the Studio IDE options menu to autofix deprecation warnings")](#)Access the Studio IDE options menu to autofix deprecation warnings

   The tool performs a `dbt parse —show-all-deprecations —no-partial-parse` to find the deprecations in your project.

5. If you don't see the deprecations and the **Autofix warnings** button, click the command history in the bottom left:

   [![Access recent commands to see the autofix button](/img/docs/dbt-platform/platform-ide/command-history.png?v=2 "Access recent commands to see the autofix button")](#)Access recent commands to see the autofix button

6. When the command history opens, click the **Autofix warnings** button:

   [![Learn what deprecations need to be auto fixed](/img/docs/dbt-platform/platform-ide/autofix-button.png?v=2 "Learn what deprecations need to be auto fixed")](#)Learn what deprecations need to be auto fixed

7. When the **Proceed with autofix** dialog opens, click **Continue** to begin resolving project deprecations and start a follow-up parse to show remaining deprecations.

   [![Proceed with autofix](/img/docs/dbt-platform/platform-ide/proceed-with-autofix.png?v=2 "Proceed with autofix")](#)Proceed with autofix

8. Once complete, a success message appears.

   Click **Review changes** to verify the changes.

   [![Success](/img/docs/dbt-platform/platform-ide/autofix-success.png?v=2 "Success")](#)Success

   <!-- -->

9. Click **Commit and sync** in the top left of Studio IDE to commit these changes to the project repository.

10. You are now ready to enable Fusion if you [meet the requirements](https://docs.getdbt.com/docs/fusion/supported-features.md#requirements)!

## Related docs[​](#related-docs "Direct link to Related docs")

* [Quickstart guide](https://docs.getdbt.com/guides.md)
* [About dbt](https://docs.getdbt.com/docs/platform/about-platform/dbt-platform-features.md)
* [Develop in the Cloud](https://docs.getdbt.com/docs/platform/studio-ide/develop-in-studio.md)
