# I'm seeing a Gitlab authentication out of date error loop

If you're seeing a 'GitLab Authentication is out of date' 500 server error page - this usually occurs when the deploy key in the repository settings in both dbt and GitLab do not match.

No worries - this is a current issue the dbt Labs team is working on and we have a few workarounds for you to try:

### 1st Workaround[​](#1st-workaround "Direct link to 1st Workaround")

1. Disconnect repo from project in dbt.
2. Go to Gitlab and click on Settings > Repository.
3. Under Repository Settings, remove/revoke active dbt deploy tokens and deploy keys.
4. Attempt to reconnect your repository via dbt.
5. You would then need to check Gitlab to make sure that the new deploy key is added.
6. Once confirmed that it's added, refresh dbt and try developing once again.

### 2nd Workaround[​](#2nd-workaround "Direct link to 2nd Workaround")

1. Keep repo in project as is -- don't disconnect.
2. Copy the deploy key generated in dbt.
3. Go to Gitlab and click on Settings > Repository.
4. Under Repository Settings, manually add to your Gitlab project deploy key repo (with `Grant write permissions` box checked).
5. Go back to dbt, refresh your page and try developing again.

If you've tried the workarounds above and are still experiencing this behavior - reach out to the Support team at <support@getdbt.com> and we'll be happy to help!

## Was this page helpful?

YesNo

[Privacy policy](https://www.getdbt.com/cloud/privacy-policy)[Create a GitHub issue](https://github.com/dbt-labs/docs.getdbt.com/issues)

This site is protected by reCAPTCHA and the Google [Privacy Policy](https://policies.google.com/privacy) and [Terms of Service](https://policies.google.com/terms) apply.
