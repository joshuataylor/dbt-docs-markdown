# Are the results of freshness stored anywhere?

Yes!

The `dbt source freshness` command will output a pass/warning/error status for each table selected in the freshness snapshot.

Additionally, dbt will write the freshness results to a file in the `target/` directory called `sources.json` by default. You can also override this destination, use the `-o` flag to the `dbt source freshness` command.

After enabling source freshness within a job, configure [Artifacts](https://docs.getdbt.com/docs/deploy/artifacts.md) in your **Project Details** page, which you can find by selecting your account name on the left side menu in dbt and clicking **Account settings**. You can see the current status for source freshness by clicking **View Sources** in the job page.

## Was this page helpful?

YesNo

[Privacy policy](https://www.getdbt.com/cloud/privacy-policy)[Create a GitHub issue](https://github.com/dbt-labs/docs.getdbt.com/issues)

This site is protected by reCAPTCHA and the Google [Privacy Policy](https://policies.google.com/privacy) and [Terms of Service](https://policies.google.com/terms) apply.
