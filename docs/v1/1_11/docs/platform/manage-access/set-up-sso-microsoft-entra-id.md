# Set up SSO with Microsoft Entra ID

dbt platform | Enterprise, Enterprise+

dbt Enterprise-tier plans support single-sign on via Microsoft Entra ID (formerly Azure AD).

SCIM available for Entra ID

After setting up single sign-on (SSO), you can [set up System for Cross-Domain Identity Management (SCIM)](./scim-entra-id.md) with Entra ID to automate user and group provisioning.

Currently supported SSO features include:

* IdP-initiated SSO
* SP-initiated SSO
* Just-in-time provisioning

## Configuration

dbt supports both single tenant and multi-tenant Microsoft Entra ID (formerly Azure AD) SSO Connections. For most Enterprise purposes, you will want to use the single-tenant flow when creating a Microsoft Entra ID Application.

### Creating an application

Log into the Azure portal for your organization. Using the [**Microsoft Entra ID**](https://portal.azure.com/#home) page, you will need to select the appropriate directory and then register a new application.

1. Under **Manage**, select **App registrations**.
2. Click **+ New Registration** to begin creating a new application registration.

[![Creating a new app registration](/img/docs/dbt-platform/dbt-platform-enterprise/azure/azure-app-registration-empty.png?v=2 "Creating a new app registration")](#)Creating a new app registration

3. Supply configurations for the **Name** and **Supported account types** fields. Choose the **Supported account types** value based on which tenants you want to allow, and note the matching **Microsoft Entra ID Domain** value you'll need later when [supplying credentials](#supplying-credentials) in dbt:

| Customer need                                                          | Supported account types (Azure)                                          | Value to enter in dbt's Microsoft Entra ID Domain field |
| ---------------------------------------------------------------------- | ------------------------------------------------------------------------ | ------------------------------------------------------- |
| One tenant only *(default, recommended for most enterprise use-cases)* | Accounts in this organizational directory only                           | The primary domain name for your Azure directory        |
| Multiple specific Entra ID tenants                                     | Accounts in organizational directories set by Entra admin                | `organizations`                                         |
| Any org tenant plus personal Microsoft accounts                        | Accounts in any organizational directory and personal Microsoft accounts | `common`                                                |
| Personal Microsoft accounts only                                       | Personal Microsoft accounts only                                         | `consumers`                                             |

4. (Optional) To ensure your multi-tenant setup works correctly, you’ll need to make two key adjustments beyond just selecting “Multi-tenant” in your Azure account settings:

   * Update the Microsoft Entra ID Domain: In the dbt “Microsoft Entra ID Domain:” field, enter the specific authority string (`organizations`, `common`, or `consumers`) rather than the domain name for your Azure directory. For more details, see the [Supplying credentials](#supplying-credentials)
   * Grant Admin Consent for Each Tenant: Because this is an Entra (formerly Azure AD) requirement, each separate tenant will need its own administrator to grant consent. If users from other tenants attempt to log in before this is done, they will see an “admin approval required” screen. An admin can resolve this by visiting the specific consent URL provided by Microsoft for their tenant (for example,`https://login.microsoftonline.com/{TENANT_ID}/adminconsent?client_id={CLIENT_ID}`)

5. Configure the **Redirect URI**. Set the type to **Web** and reference the table below for the appropriate Redirect URI values for single-tenant and multi-tenant Entra ID app deployments. For most enterprise use-cases, you will want to use the single-tenant Redirect URI. Replace `YOUR_AUTH0_URI` with the [appropriate Auth0 URI](./sso-overview.md#auth0-uris) for your region and plan.

**Note:** Your dbt platform tenancy has no bearing on this setting. This Entra ID app setting controls app access:

* **Single-tenant:** Only users from your Entra ID tenant can access the app.
* **Multi-tenant:** Users from *any* Entra ID tenant can access the app.

| Application Type              | Redirect URI                            |
| ----------------------------- | --------------------------------------- |
| Single-tenant *(recommended)* | `https://YOUR_AUTH0_URI/login/callback` |
| Multi-tenant                  | `https://YOUR_AUTH0_URI/login/callback` |

[![Configuring a new app registration](/img/docs/dbt-platform/dbt-platform-enterprise/azure/azure-new-application-alternative.png?v=2 "Configuring a new app registration")](#)Configuring a new app registration

6. Save the App registration to continue setting up Microsoft Entra ID SSO.

Configuration with the new Microsoft Entra ID interface (optional)

Depending on your Microsoft Entra ID settings, your App Registration page might look different than the screenshots shown earlier. If you are *not* prompted to configure a Redirect URI on the **New Registration** page, then follow steps 7 - 8 below after creating your App Registration. If you were able to set up the Redirect URI in the steps above, then skip ahead to [step 8](#adding-users-to-an-enterprise-application).

7. After registering the new application without specifying a Redirect URI, click on **App registration** and then navigate to the **Authentication** tab for the new application.

8. Click **+ Add platform** and enter a Redirect URI for your application. See step 4 above for more information on the correct Redirect URI value for your dbt application.

   Platform type

   When selecting the platform type, choose **Web**, not **Single-page application (SPA)**. The dbt SSO integration redeems the authorization code from the server using a client secret. So, if you add the **Redirect URI** under **SPA**, Entra ID enforces PKCE and rejects the server-side token exchange, causing sign-in to fail with the error `AADSTS9002325: Proof Key for Code Exchange is required for cross-origin authorization code redemption.`

[![Configuring a Redirect URI](/img/docs/dbt-platform/dbt-platform-enterprise/azure/azure-redirect-uri.png?v=2 "Configuring a Redirect URI")](#)Configuring a Redirect URI

### Azure <-> dbt User and Group mapping

info

There is a [limitation](https://learn.microsoft.com/en-us/entra/identity/hybrid/connect/how-to-connect-fed-group-claims#important-caveats-for-this-functionality) on the number of groups Azure will emit (capped at 150) via the SSO token, meaning if a user belongs to more than 150 groups, it will appear as though they belong to none. To prevent this, configure [group assignments](https://learn.microsoft.com/en-us/entra/identity/enterprise-apps/assign-user-or-group-access-portal?pivots=portal) with the dbt app in Azure and set a [group claim](https://learn.microsoft.com/en-us/entra/identity/hybrid/connect/how-to-connect-fed-group-claims#add-group-claims-to-tokens-for-saml-applications-using-sso-configuration) so Azure emits only the relevant groups.

The Azure users and groups you will create in the following steps are mapped to groups created in dbt based on the group name. Reference the docs on [enterprise permissions](./enterprise-permissions.md) for additional information on how users, groups, and permission sets are configured in dbt.

The dbt platform uses the **User principal name** (UPN) in Microsoft Entra ID to identify and match users logging in to dbt through SSO. The UPN is usually formatted as an email address.

### Adding users to an Enterprise application

Once you've registered the application, the next step is to assign users to it. Add the users you want to be viewable to dbt with the following steps:

8. Navigate back to the [**Default Directory**](https://portal.azure.com/#home) (or **Home**) and click **Enterprise Applications**.
9. Click the name of the application you created earlier.
10. Click **Assign Users and Groups**.
11. Click **Add User/Group**.
12. Assign additional users and groups as needed.

[![Adding Users to an Enterprise Application a Redirect URI](/img/docs/dbt-platform/dbt-platform-enterprise/azure/azure-enterprise-app-users.png?v=2 "Adding Users to an Enterprise Application a Redirect URI")](#)Adding Users to an Enterprise Application a Redirect URI

User assignment required?

Under **Properties** check the toggle setting for **User assignment required?** and confirm it aligns to your requirements. Most customers will want this toggled to **Yes** so that only users/groups explicitly assigned to dbt will be able to sign in. If this setting is toggled to **No** any user will be able to access the application if they have a direct link to the application per [Microsoft Entra ID Documentation](https://docs.microsoft.com/en-us/azure/active-directory/manage-apps/assign-user-or-group-access-portal#configure-an-application-to-require-user-assignment)

### Configuring permissions

13. Navigate back to [**Default Directory**](https://portal.azure.com/#home) (or **Home**) and then **App registration**.
14. Select your application and then select **API permissions**.
15. Click **+Add a permission** and add the permissions shown in the following table:

| API Name        | Type      | Permission             | Required?                                                        |
| --------------- | --------- | ---------------------- | ---------------------------------------------------------------- |
| Microsoft Graph | Delegated | `User.Read`            | Yes                                                              |
| Microsoft Graph | Delegated | `GroupMember.Read.All` | Yes                                                              |
| Microsoft Graph | Delegated | `Directory.Read.All`   | Optional — may be required if users are assigned to > 200 groups |

The default scope only requires `User.Read` and `GroupMember.Read.All`. If you assign a user to more than 200 groups, you may need to grant additional permissions such as `Directory.Read.All`.

SSO before December 2025

If you set up SSO before December 2025, your existing configuration may request `Directory.Read.All` instead of `GroupMember.Read.All`. To use the updated scopes, delete and re-create your SSO [configuration](#configuring-permissions).

16. Save these permissions, then click **Grant admin consent** to grant admin consent for this directory on behalf of all of your users.

[![Configuring application permissions](/img/docs/dbt-platform/dbt-platform-enterprise/azure/azure-permissions-overview.png?v=2 "Configuring application permissions")](#)Configuring application permissions

### Creating a client secret

17. Under **Manage**, click **Certificates & secrets**.
18. Click **+New client secret**.
19. Name the client secret "dbt" (or similar) to identify the secret.
20. Select **730 days (24 months)** as the expiration value for this secret (recommended).
21. Click **Add** to finish creating the client secret value (not the client secret ID).
22. Record the generated client secret somewhere safe. Later in the setup process, we'll use this client secret in dbt to finish configuring the integration.

[![Configuring certificates & secrets](/img/docs/dbt-platform/dbt-platform-enterprise/azure/azure-secret-config.png?v=2 "Configuring certificates & secrets")](#)Configuring certificates & secrets

[![Recording the client secret](/img/docs/dbt-platform/dbt-platform-enterprise/azure/azure-secret-saved.png?v=2 "Recording the client secret")](#)Recording the client secret

### Collect client credentials

23. Navigate to the **Overview** page for the app registration.
24. Note the **Application (client) ID** and **Directory (tenant) ID** shown in this form and record them along with your client secret. We'll use these keys in the steps below to finish configuring the integration in dbt.

[![Collecting credentials. Store these somewhere safe](/img/docs/dbt-platform/dbt-platform-enterprise/azure/azure-overview.png?v=2 "Collecting credentials. Store these somewhere safe")](#)Collecting credentials. Store these somewhere safe

## Configuring dbt

To complete setup, follow the steps below in the dbt application.

### Supplying credentials

To complete this section, you will need your login URL slug. This slug controls the URL where users on your account can log into your application. dbt automatically generates login URL slugs, which can't be altered. It will contain only letters, numbers, and dashes. For example, the login URL slug for dbt Labs would look something like `dbt-labs-afk123`. Login URL slugs are unique across all dbt accounts.

Users can also sign in at <https://login.dbt.com> to see accounts they have access to across instances. The enterprise login URL that includes your slug remains the URL used for IdP-initiated SSO flows with your identity provider.

25. From dbt, click on your account name in the left side menu and select **Account settings**.
26. Click **SSO & SCIM** from the menu.
27. Click **Get started** if SSO has not been configured, or **Edit** if it has already been set up.
28. Supply the following SSO details:

| Field                         | Value                                                                                                                                                                                                                                                                                                                                                       |
| ----------------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **Log in with**               | Microsoft Entra ID Single Tenant, or Microsoft Entra ID Multi Tenant if you configured a multi-tenant **Supported account types** value                                                                                                                                                                                                                     |
| **Client ID**                 | Paste the **Application (client) ID** recorded in the steps above                                                                                                                                                                                                                                                                                           |
| **Client Secret**             | Paste the **Client Secret** (remember to use the Secret Value instead of the Secret ID) from the steps above;<br />**Note:** When the client secret expires, an Entra ID admin will have to generate a new one to be pasted into dbt for uninterrupted application access.                                                                                  |
| **Tenant ID**                 | Paste the **Directory (tenant) ID** recorded in the steps above. (This field only appears when you select **Microsoft Entra ID Single Tenant**; it is not needed for multi-tenant).                                                                                                                                                                         |
| **Microsoft Entra ID Domain** | For single tenant, enter the domain name for your Azure directory (such as `fishtownanalytics.com`). Only use the primary domain; this won't block access for other domains. For multi-tenant, enter the matching authority string (`organizations`, `common`, or `consumers`) instead. Refer to [Supported account types table](#creating-an-application). |

[![Configuring Entra ID AD SSO in dbt](/img/docs/dbt-platform/dbt-platform-enterprise/azure/azure-cloud-sso.png?v=2 "Configuring Entra ID AD SSO in dbt")](#)Configuring Entra ID AD SSO in dbt

29. Click **Save** to complete setup for the Microsoft Entra ID SSO integration. From here, you can navigate to the login URL generated for your account's *slug* to test logging in with Entra ID.

Logging in

Users can sign in at <https://login.dbt.com> to view accounts they have access to across instances and choose where to open dbt platform. This is the recommended entry point for most users.

For SSO through your identity provider, you can also use the following URL format with your account login URL slug. Replace `LOGIN_SLUG` with the value from the previous steps and `YOUR_ACCESS_URL` with the [appropriate Access URL](../about-platform/access-regions-ip-addresses.md) for your region and plan:

`https://YOUR_ACCESS_URL/enterprise-login/LOGIN-SLUG`

Account administrators can turn account discovery on or off with **Enable global account discovery** in [Account settings](../account-settings.md#enable-global-account-discovery).

### Additional configuration options

The **Single sign-on** section also contains additional configuration options which are located after the credentials fields.

* **Include all groups:** Retrieve all groups to which a user belongs from your identity provider. If a user is a member of nested groups, it will also include the parent groups. When this option is disabled, only groups where the user has direct membership will be supplied. This option is enabled by default.

* **Maximum number of groups to retrieve:** Provides a configurable limit to the number of groups to retrieve for users. By default, this is set to 250 groups, but this number can be increased if users' group memberships exceed that amount.

## Setting up RBAC

Now you have completed setting up SSO with Entra ID, the next steps will be to set up [RBAC groups](./enterprise-permissions.md) to complete your access control configuration.

Set up SCIM

Now that you've set up SSO with Entra ID, you can [set up SCIM](./scim-entra-id.md) to automate user and group provisioning.

## Troubleshooting tips

 Receiving a 'AADSTS90094: Admin consent is required' error

If you set up SSO before December 2025, your existing configuration may request `Directory.Read.All` instead of `GroupMember.Read.All`. To use the updated scopes, delete and re-create your SSO [configuration](#configuring-permissions).

 Domain name mismatch

Ensure that the domain name under which user accounts exist in Azure matches the domain you supplied in [Supplying credentials](#supplying-credentials) when you configured SSO.

[![Obtaining the user domain from Azure](/img/docs/dbt-platform/dbt-platform-enterprise/azure/azure-get-domain.png?v=2 "Obtaining the user domain from Azure")](#)Obtaining the user domain from Azure

For additional troubleshooting — including "Admin consent required" prompts for new users, "Access Denied" after SAML authentication, and issues with Entity ID or ACS URL changes — refer to [SSO FAQs and troubleshooting](./sso-faq.md).

## Learn more by video

The following video explains how to set up SSO with Microsoft Entra ID:

[](/img/docs/dbt-platform/dbt-platform-enterprise/access-control/sso-dbt-entra-id.mp4)
