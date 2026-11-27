# How do I delete a user in dbt?

To delete a user in dbt, you must be an account owner or have admin privileges. If the user has a `developer` license type, this will open up their seat for another user or allow the admins to lower the total number of seats.

1. From dbt, click on your account name in the left side menu and, select **Account settings**.

![Open Account settings from the account menu in the left sidebar](/img/docs/dbt-platform/account-settings-menu.png?v=2 "Open Account settings from the account menu in the left sidebar")Open Account settings from the account menu in the left sidebar

2. Under **Settings**, select **Users**.
3. Select the user you want to delete, then click **Edit**.
4. Click **Delete** in the upper right. Click **Confirm Delete** to immediately delete the user without additional password prompts. This action cannot be undone. However, you can re-invite the user with the same information if the deletion was made in error.

![Deleting a user](/img/docs/dbt-platform/delete_user.png?v=2 "Deleting a user")Deleting a user

If you are on a **Starter** plan and you're deleting users to reduce the number of billable seats, lower the seat count so you aren't billed for unused seats.

After you delete a developer user (or switch them to Read-Only), you may see a **Change developer seats?** prompt that offers to match your seat count to the developers you still have. Click **Confirm** to accept, or **No thanks** to leave the seat count unchanged.

To change the seat count yourself from billing:

1. Click your account name in the left sidebar and select **Account settings**.
2. Under **Settings**, select **Billing & Usage** (labeled **Billing** on some accounts).
3. Open **Overview** if it appears, then open the **Billing details** tab.
4. Under **Payment information**, go to the **Seat purchases** section.
5. From the **Number of developer seats** dropdown, select the lower seat count. Options below the number of seats in use are unavailable, so delete or reassign users first if needed.
6. Click **Update payment information**.
7. In the **Confirm removed seats** modal, click **Remove seats**.

## Related docs

* [dbt licenses](../../docs/platform/manage-access/seats-and-users.md#licenses)
