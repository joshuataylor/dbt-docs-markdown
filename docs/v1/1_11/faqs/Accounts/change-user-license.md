# How do I change a user license type to read-only in dbt?

To change the license type for a user from `developer` to `read-only` or `IT` in dbt, you must be an account owner or have admin privileges. You might make this change to free up a billable seat but retain the user’s access to view the information in the dbt account.

1. From dbt, click on your account name in the left side menu and, select **Account settings**.

![Open Account settings from the account menu in the left sidebar](/img/docs/dbt-platform/account-settings-menu.png?v=2 "Open Account settings from the account menu in the left sidebar")Open Account settings from the account menu in the left sidebar

2. Under **Settings**, select **Users**.
3. Select the user you want to modify and click **Edit** in the upper right.
4. In the **License** field, select **Read-only** or **IT** (from **Developer**), and click **Save**.

![Change the license type on the User details page](/img/docs/dbt-platform/change-user-license-details.png?v=2 "Change the license type on the User details page")Change the license type on the User details page

License types override group permissions

**User license types always override their assigned group permission sets.** For example, a user with a Read-Only license cannot perform administrative actions, even if they belong to an Account Admin group.

This ensures that license restrictions are always enforced, regardless of group membership.
