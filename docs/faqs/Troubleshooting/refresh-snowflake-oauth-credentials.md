# Reconnecting to Snowflake OAuth after authentication expires

When you connect Snowflake to dbt platform using [OAuth](https://docs.getdbt.com/docs/cloud/manage-access/set-up-snowflake-oauth.md), dbt stores a refresh token. This allows your development credentials to remain usable in tools like the Studio IDE and the dbt Semantic Layer without needing to re-authenticate each time.

If you see an `authentication has expired` error when you try to run queries, you must renew your connection between Snowflake and the dbt platform.

To resolve the issue, complete the following steps:

1. Go to your **Profile settings** page, accessible from the navigation menu.
2. Navigate to **Credentials** and then choose the project where you're experiencing the issue.
3. Under **Development credentials**, click the **Reconnect Snowflake Account** button. This will guide you through re-authenticating using your SSO workflow.

Your Snowflake administrator can [configure the refresh token validity period](https://docs.getdbt.com/docs/cloud/manage-access/set-up-snowflake-oauth.md#create-a-security-integration), up to the maximum 90 days.

If you've tried these step and are still getting this error, please contact the Support team at <support@getdbt.com> for further assistance.

## Was this page helpful?

YesNo

[Privacy policy](https://www.getdbt.com/cloud/privacy-policy)[Create a GitHub issue](https://github.com/dbt-labs/docs.getdbt.com/issues)

This site is protected by reCAPTCHA and the Google [Privacy Policy](https://policies.google.com/privacy) and [Terms of Service](https://policies.google.com/terms) apply.
