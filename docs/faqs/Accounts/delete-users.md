# How do I delete a user in dbt?

To delete a user in dbt, you must be an account owner or have admin privileges. If the user has a `developer` license type, this will open up their seat for another user or allow the admins to lower the total number of seats.

1. From dbt, click on your account name in the left side menu and, select **Account settings**.

[![Navigate to account settings](/img/docs/dbt-cloud/Navigate-to-account-settings.png?v=2 "Navigate to account settings")](#)Navigate to account settings

2. In **Account settings**, select **Users** under **Teams**.
3. Select the user you want to delete, then click **Edit**.
4. Click **Delete** in the bottom left. Click **Confirm Delete** to immediately delete the user without additional password prompts. This action cannot be undone. However, you can re-invite the user with the same information if the deletion was made in error.

[![Deleting a user](/img/docs/dbt-cloud/delete_user.png?v=2 "Deleting a user")](#)Deleting a user

<!-- -->

If you are on a **Starter** plan and you're deleting users to reduce the number of billable seats, follow these steps to lower the license count to avoid being overcharged:

1. In **Account Settings**, select **Billing**.
2. Under **Billing details**, enter the number of developer seats you want and make sure you fill in all the payment details, including the **Billing address** section. If you leave any field blank, you won't be able to save your changes.
3. Click **Update Payment Information** to save your changes.

[![Navigate to Account settings -> Users to modify dbt users](/img/docs/dbt-cloud/faq-account-settings-billing.png?v=2 "Navigate to Account settings -> Users to modify dbt users")](#)Navigate to Account settings -> Users to modify dbt users

## Related docs[​](#related-docs "Direct link to Related docs")

* [dbt licenses](https://docs.getdbt.com/docs/cloud/manage-access/seats-and-users.md#licenses)

## Was this page helpful?

YesNo

[Privacy policy](https://www.getdbt.com/cloud/privacy-policy)[Create a GitHub issue](https://github.com/dbt-labs/docs.getdbt.com/issues)

This site is protected by reCAPTCHA and the Google [Privacy Policy](https://policies.google.com/privacy) and [Terms of Service](https://policies.google.com/terms) apply.
