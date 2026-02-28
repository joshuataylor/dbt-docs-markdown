# Migrating to Auth0 for SSO [Enterprise](https://www.getdbt.com/pricing "Go to https://www.getdbt.com/pricing")[Enterprise +](https://www.getdbt.com/pricing "Go to https://www.getdbt.com/pricing")

dbt Labs is partnering with Auth0 to bring enhanced features to dbt's single sign-on (SSO) capabilities. Auth0 is an identity and access management (IAM) platform with advanced security features, and it will be leveraged by dbt. These changes will require some action from customers with SSO configured in dbt today, and this guide will outline the necessary changes for each environment.

If you have not yet configured SSO in dbt, refer instead to our setup guides for [SAML](https://docs.getdbt.com/docs/cloud/manage-access/set-up-sso-saml-2.0.md), [Okta](https://docs.getdbt.com/docs/cloud/manage-access/set-up-sso-okta.md), [Google Workspace](https://docs.getdbt.com/docs/cloud/manage-access/set-up-sso-google-workspace.md), or [Microsoft Entra ID (formerly Azure AD)](https://docs.getdbt.com/docs/cloud/manage-access/set-up-sso-microsoft-entra-id.md) single sign-on services.

## Start the migration[​](#start-the-migration "Direct link to Start the migration")

The Auth0 migration feature is being rolled out incrementally to customers who have SSO features already enabled. When the migration option has been enabled on your account, you will see **SSO Update Required** on the right side of the menu bar, near the settings icon.

Alternatively, you can start the process by clicking your account name at the bottom left-hand menu and going to **Account settings** > **SSO & SCIM**. Click the **Begin Migration** button to start.

vanity urls

Don't use vanity URLs when configuring the SSO settings. You need to use the generic URL provided in the SSO settings for your environment. For example, if your vanity URL is `cloud.MY_COMPANY.getdbt.com`, configure `auth.cloud.getdbt.com` as `<YOUR_AUTH0_URI>`.

There are two fields in the SSO settings that you need for the migration:

* **Single sign-on URL:** This will be in the format of your login URL `https://<YOUR_AUTH0_URI>/login/callback?connection=<SLUG>`
* **Audience URI (SP Entity ID):** This will be in the format `urn:auth0:<YOUR_AUTH0_ENTITYID>:<SLUG>`

Once you have opted to begin the migration process, the following steps will vary depending on the configured identity provider. You can just skip to the section that's right for your environment. These steps only apply to customers going through the migration; new setups will use the existing [setup instructions](https://docs.getdbt.com/docs/cloud/manage-access/sso-overview.md).

## SAML 2.0[​](#saml-20 "Direct link to SAML 2.0")

SAML 2.0 users must update a few fields in the SSO app configuration to match the new Auth0 URL and URI. You can approach this by editing the existing SSO app settings or creating a new one to accommodate the Auth0 settings. One approach isn't inherently better, so you can choose whichever works best for your organization.

### SAML 2.0 and Okta[​](#saml-20-and-okta "Direct link to SAML 2.0 and Okta")

The Okta fields that will be updated are:

* Single sign-on URL — `https://<YOUR_AUTH0_URI>/login/callback?connection=<SLUG>`
* Audience URI (SP Entity ID) — `urn:auth0:<YOUR_AUTH0_ENTITYID>:<SLUG>`

Below are sample steps to update. You must complete all of them to ensure uninterrupted access to dbt and you should coordinate with your identity provider admin when making these changes.

1. Replace `<SLUG>` with your account’s login URL slug.

Here is an example of an updated SAML 2.0 setup in Okta.

[![Okta configuration with new URL](/img/docs/dbt-cloud/access-control/new-okta-config.png?v=2 "Okta configuration with new URL")](#)Okta configuration with new URL

2. Save the configuration, and your SAML settings will look something like this:

[![New Okta configuration completed](/img/docs/dbt-cloud/access-control/new-okta-completed.png?v=2 "New Okta configuration completed")](#)New Okta configuration completed

3. Toggle the `Enable new SSO authentication` option to ensure the traffic is routed correctly. *The new SSO migration action is final and cannot be undone.*

4. Save the settings and test the new configuration using the SSO login URL provided on the settings page.

### SAML 2.0 and Entra ID[​](#saml-20-and-entra-id "Direct link to SAML 2.0 and Entra ID")

The Entra ID fields that will be updated are:

* Single sign-on URL — `https://<YOUR_AUTH0_URI>/login/callback?connection=<SLUG>`
* Audience URI (SP Entity ID) — `urn:auth0:<YOUR_AUTH0_ENTITYID>:<SLUG>`

The new values for these fields can be found in dbt by navigating to **Account settings** > **SSO & SCIM**.

1. Replace `<SLUG>` with your organization’s login URL slug.

2. Locate your dbt SAML2.0 app in the **Enterprise applications** section of Azure. Click **Single sign-on** on the left side menu.

3. Edit the **Basic SAML configuration** tile and enter the values from your account:

   * Entra ID **Identifier (Entity ID)** = dbt **Audience URI (SP Entity ID)**
   * Entra ID **Reply URL (Assertion Consumer Service URL)** = dbt **Single sign-on URL**

   [![Editing the SAML configuration window in Entra ID](/img/docs/dbt-cloud/access-control/edit-entra-saml.png?v=2 "Editing the SAML configuration window in Entra ID")](#)Editing the SAML configuration window in Entra ID

4. Save the fields and the completed configuration will look something like this:

   [![Completed configuration of the SAML fields in Entra ID](/img/docs/dbt-cloud/access-control/entra-id-saml.png?v=2 "Completed configuration of the SAML fields in Entra ID")](#)Completed configuration of the SAML fields in Entra ID

5. Toggle the `Enable new SSO authentication` option to ensure the traffic is routed correctly. *The new SSO migration action is final and cannot be undone.*

6. Save the settings and test the new configuration using the SSO login URL provided on the settings page.

## Microsoft Entra ID[​](#microsoft-entra-id "Direct link to Microsoft Entra ID")

Microsoft Entra ID admins using OpenID Connect (ODIC) will need to make a slight adjustment to the existing authentication app in the Azure portal. This migration does not require that the entire app be deleted or recreated; you can edit the existing app. Start by opening the Azure portal and navigating to the Microsoft Entra ID overview.

Below are steps to update. You must complete all of them to ensure uninterrupted access to dbt and you should coordinate with your identity provider admin when making these changes.

1. Click **App Registrations** on the left side menu.

[![Select App Registrations](/img/docs/dbt-cloud/access-control/aad-app-registration.png?v=2 "Select App Registrations")](#)Select App Registrations

2. Select the proper **dbt** app (name may vary) from the list. From the app overview, click on the hyperlink next to **Redirect URI**

[![Click the Redirect URI hyperlink](/img/docs/dbt-cloud/access-control/app-overview.png?v=2 "Click the Redirect URI hyperlink")](#)Click the Redirect URI hyperlink

3. In the **Web** pane with **Redirect URIs**, click **Add URI** and enter the appropriate `https://<YOUR_AUTH0_URI>/login/callback`. Save the settings and verify it is counted in the updated app overview.

[![Enter new redirect URI](/img/docs/dbt-cloud/access-control/redirect-URI.png?v=2 "Enter new redirect URI")](#)Enter new redirect URI

4. Navigate to the dbt environment and open the **Account settings**. Click the **SSO & SCIM** option from the left-side menu and click the **Edit** option from the right side of the SSO pane. The **domain** field is the domain your organization uses to login to Microsoft Entra ID. Toggle the **Enable New SSO Authentication** option and **Save**. *Once this option is enabled, it cannot be undone.*

Domain authorization

You must complete the domain authorization before you toggle `Enable New SSO Authentication`, or the migration will not complete successfully.

## Google Workspace[​](#google-workspace "Direct link to Google Workspace")

Google Workspace admins updating their SSO APIs with the Auth0 URL won't have to do much if it is an existing setup. This can be done as a new project or by editing an existing SSO setup. No additional scopes are needed since this is migrating from an existing setup. All scopes were defined during the initial configuration.

Below are steps to update. You must complete all of them to ensure uninterrupted access to dbt and you should coordinate with your identity provider admin when making these changes.

1. Open the [Google Cloud console](https://console.cloud.google.com/) and select the project with your dbt single sign-on settings. From the project page **Quick Access**, select **APIs and Services**

[![Google Cloud Console](/img/docs/dbt-cloud/access-control/google-cloud-sso.png?v=2 "Google Cloud Console")](#)Google Cloud Console

2. Click **Credentials** from the left side pane and click the appropriate name from **OAuth 2.0 Client IDs**

[![Select the OAuth 2.0 Client ID](/img/docs/dbt-cloud/access-control/sso-project.png?v=2 "Select the OAuth 2.0 Client ID")](#)Select the OAuth 2.0 Client ID

3. In the **Client ID for Web application** window, find the **Authorized Redirect URIs** field and click **Add URI** and enter `https://<YOUR_AUTH0_URI>/login/callback`.

Click **Save** once you are done.

4. *You will need a person with Google Workspace admin privileges to complete these steps in dbt*. In dbt, navigate to the **Account settings**, click on **SSO & SCIM**, and then click **Edit** on the right side of the **Single sign-on** pane. Toggle the **Enable New SSO Authentication** option and select **Save**. This will trigger an authorization window from Google that will require admin credentials. *The migration action is final and cannot be undone*. Once the authentication has gone through, test the new configuration using the SSO login URL provided on the settings page.

Domain authorization

You must complete the domain authorization before you toggle `Enable New SSO Authentication`, or the migration will not complete successfully.

## Was this page helpful?

YesNo

[Privacy policy](https://www.getdbt.com/cloud/privacy-policy)[Create a GitHub issue](https://github.com/dbt-labs/docs.getdbt.com/issues)

This site is protected by reCAPTCHA and the Google [Privacy Policy](https://policies.google.com/privacy) and [Terms of Service](https://policies.google.com/terms) apply.
