# How do I transfer account ownership to another user?

You can transfer your dbt [access control](https://docs.getdbt.com/docs/cloud/manage-access/about-user-access.md) to another user by following the steps below, depending on your dbt account plan:

| Account plan                               | Steps                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                               |
| ------------------------------------------ | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **Developer**                              | You can transfer ownership by changing the email directly on your dbt profile page, which you can access using this URL when you replace `YOUR_ACCESS_URL` with the [appropriate Access URL](https://docs.getdbt.com/docs/cloud/about-cloud/access-regions-ip-addresses.md) for your region and plan: `https://YOUR_ACCESS_URL/settings/profile`. Before doing this, please ensure that you unlink your GitHub profile. The email address of the new account owner cannot be associated with another dbt account.                                   |
| **Starter**                                | Existing account admins with account access can add users to, or remove users from the owner group.                                                                                                                                                                                                                                                                                                                                                                                                                                                 |
| **Enterprise or Enterprise+**              | Account admins can add users to, or remove users from a group with Account Admin permissions.                                                                                                                                                                                                                                                                                                                                                                                                                                                       |
| **If all account owners left the company** | If the account owner has left your organization, you will need to work with *your* IT department to have incoming emails forwarded to the new account owner. Once your IT department has redirected the emails, you can request to reset the user password. Once you log in, you can change the email on the Profile page when you replace `YOUR_ACCESS_URL` with the [appropriate Access URL](https://docs.getdbt.com/docs/cloud/about-cloud/access-regions-ip-addresses.md) for your region and plan: `https://YOUR_ACCESS_URL/settings/profile`. |

Search table...

|                  |   |   |   |   |
| ---------------- | - | - | - | - |
| Loading table... |   |   |   |   |

When you make any account owner and email changes:

* The new email address *must* be verified through our email verification process.
* You can update any billing email address or [Notifications Settings](https://docs.getdbt.com/docs/deploy/job-notifications.md) to reflect the new account owner changes, if applicable.
* When transferring account ownership, please ensure you [unlink](https://docs.getdbt.com/faqs/Accounts/git-account-in-use.md) your GitHub account in dbt. This is because you can only have your Git account linked to one dbt user account.

## Was this page helpful?

YesNo

[Privacy policy](https://www.getdbt.com/cloud/privacy-policy)[Create a GitHub issue](https://github.com/dbt-labs/docs.getdbt.com/issues)

This site is protected by reCAPTCHA and the Google [Privacy Policy](https://policies.google.com/privacy) and [Terms of Service](https://policies.google.com/terms) apply.
