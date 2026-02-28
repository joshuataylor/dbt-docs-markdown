# Invite users to dbt

dbt makes it easy to invite new users to your environment out of the box. This feature is available to all dbt customers on Starter, Enterprise, and Enterprise+ plans.

## Prerequisites[​](#prerequisites "Direct link to Prerequisites")

You must have proper permissions to invite new users:

* [**Starter accounts**](https://docs.getdbt.com/docs/cloud/manage-access/self-service-permissions.md) — must have `member` or `owner` permissions.
* [**Enterprise-tier accounts**](https://docs.getdbt.com/docs/cloud/manage-access/enterprise-permissions.md) — must have `admin`, `account admin`, `project creator`, or `security admin` permissions.
* The admin inviting the users must have a `developer` or `IT` license.

## Invite new users[​](#invite-new-users "Direct link to Invite new users")

1. In your dbt account, select your account name in the bottom left corner. Then select **Account settings**.
2. Under **Settings**, select **Users**.
3. Click on **Invite users**.

[![The invite users pane](/img/docs/dbt-cloud/access-control/invite-users.png?v=2 "The invite users pane")](#)The invite users pane

4. In the **Email Addresses** field, enter the email addresses of the users you want to invite separated by a comma, semicolon, or a new line.
5. Select the license type for the batch of users from the **License** dropdown.
6. Select the group(s) you want the invitees to belong to.
7. Click **Send invitations**.
   <!-- -->
   * If the list of invitees exceeds the number of licenses your account has available, you will receive a warning when you click **Send Invitations** and the invitations will not be sent.

## User experience[​](#user-experience "Direct link to User experience")

Email verification

Email verification is mandatory for all new users in dbt, including using Single Sign-On (SSO)⁠⁠. Automatic provisioning without email verification is not allowed. This is a security requirement that cannot be bypassed.

dbt generates and sends emails from `support@getdbt.com` to the specified addresses. Make sure that traffic from the `support@getdbt.com` email is allowed in your settings to avoid emails from going to spam or being blocked. This is the originating email address for all [instances worldwide](https://docs.getdbt.com/docs/cloud/about-cloud/access-regions-ip-addresses.md).

The email contains a link to create an account. When the user clicks on this link, they will be brought to one of two screens depending on whether SSO is configured or not.

[![Example or an email invitation](/img/docs/dbt-cloud/access-control/email-invite.png?v=2 "Example or an email invitation")](#)Example or an email invitation

* Local user
* SSO user

The default settings send the email, the user clicks the link, and is prompted to create their account:

[![Default user invitation](/img/docs/dbt-cloud/access-control/default-user-invite.png?v=2 "Default user invitation")](#)Default user invitation

If SSO is configured for the environment, the user must:

1. Click the link in the verification email.
2. Click the option to join the account.
3. A confirmation screen appears, with a link to authenticate against the company's identity provider. Click **Authenticate with your enterprise login**.

Complete the SSO flow

Accepting the invite doesn't fully complete the process. The user *must* log in using SSO to redeem the invite and access the account.

[![User invitation with SSO configured](/img/docs/dbt-cloud/access-control/sso-user-invite.png?v=2 "User invitation with SSO configured")](#)User invitation with SSO configured

Once the user completes this process, their email and user information will populate in the **Users** screen in dbt.

## FAQ[​](#faq "Direct link to FAQ")

 Is there a limit to the number of users I can invite?

Your ability to invite users is limited to the number of licenses you have available.

 What happens if I reinstate a deleted user?

If you invite a previously deleted user back to your account with the same email address, their personal profile information, including linked accounts, will persist (unless the user deleted the connection from the source account, like GitHub).

Any previously assigned permissions and access settings are reset, and you will have to reassign them.

 Why are users clicking the invitation link and getting an 'Invalid Invitation Code' error?

We have seen scenarios where embedded secure link technology (such as enterprise Outlook's [Safe Link](https://learn.microsoft.com/en-us/microsoft-365/security/office-365-security/safe-links-about?view=o365-worldwide) feature) can result in errors when clicking on the email link. Be sure to include the `getdbt.com` URL in the allowlists for these services.

 Can I have a mixture of users with SSO and username/password authentication?

Once SSO is enabled, you will no longer be able to add local users. If you have contractors or similar contingent workers, we recommend you add them to your SSO service.

 What happens if I need to resend the invitation?

From the **Users** page, click on the invite record, and you will be presented with the option to resend the invitation.

 What can I do if I entered an email address incorrectly?

From the **Users** page, click on the invite record, and you will be presented with the option to revoke it. Once revoked, generate a new invitation to the correct email address.

[![Resend or revoke the users invitation](/img/docs/dbt-cloud/access-control/resend-invite.png?v=2 "Resend or revoke the users invitation")](#)Resend or revoke the users invitation

## Was this page helpful?

YesNo

[Privacy policy](https://www.getdbt.com/cloud/privacy-policy)[Create a GitHub issue](https://github.com/dbt-labs/docs.getdbt.com/issues)

This site is protected by reCAPTCHA and the Google [Privacy Policy](https://policies.google.com/privacy) and [Terms of Service](https://policies.google.com/terms) apply.
