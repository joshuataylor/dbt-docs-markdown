# How do I change a user license type to read-only in dbt?

To change the license type for a user from `developer` to `read-only` or `IT` in dbt, you must be an account owner or have admin privileges. You might make this change to free up a billable seat but retain the user’s access to view the information in the dbt account.

1. From dbt, click on your account name in the left side menu and, select **Account settings**.

[![Navigate to account settings](/img/docs/dbt-cloud/Navigate-to-account-settings.png?v=2 "Navigate to account settings")](#)Navigate to account settings

2. In **Account Settings**, select **Users** under **Teams**.
3. Select the user you want to remove and click **Edit** in the bottom of their profile.
4. For the **License** option, choose **Read-only** or **IT** (from **Developer**), and click **Save**.

[![Change user's license type](/img/docs/dbt-cloud/change_user_to_read_only_20221023.gif?v=2 "Change user's license type")](#)Change user's license type

<!-- -->

License types override group permissions

**User license types always override their assigned group permission sets.** For example, a user with a Read-Only license cannot perform administrative actions, even if they belong to an Account Admin group.

This ensures that license restrictions are always enforced, regardless of group membership.

## Was this page helpful?

YesNo

[Privacy policy](https://www.getdbt.com/cloud/privacy-policy)[Create a GitHub issue](https://github.com/dbt-labs/docs.getdbt.com/issues)

This site is protected by reCAPTCHA and the Google [Privacy Policy](https://policies.google.com/privacy) and [Terms of Service](https://policies.google.com/terms) apply.
