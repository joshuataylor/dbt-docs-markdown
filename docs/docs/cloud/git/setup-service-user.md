# Set up Azure DevOps with Service User

## Service user overview[​](#service-user-overview "Direct link to Service user overview")

important

Service users are no longer a recommended method for authentication and dbt is rolling out a new [Entra ID service principal](https://learn.microsoft.com/en-us/entra/identity-platform/app-objects-and-service-principals) option. Once the option is available in your account settings, you should plan to [migrate from service user to service principal](https://docs.getdbt.com/docs/cloud/git/setup-service-principal.md#migrate-to-service-principal). Service principals are the [Microsoft recommended service account type](https://learn.microsoft.com/en-us/entra/architecture/secure-service-accounts#types-of-microsoft-entra-service-accounts) for app authentication.

To use our native integration with Azure DevOps in dbt, an account admin needs to set up an Microsoft Entra ID app. We recommend setting up a separate [Entra ID application than used for SSO](https://docs.getdbt.com/docs/cloud/manage-access/set-up-sso-microsoft-entra-id.md).

1. [Register an Entra ID app](#register-a-microsoft-entra-id-app).
2. [Add permissions to your new app](#add-permissions-to-your-new-app).
3. [Add another redirect URI](#add-another-redirect-uri).
4. [Connect Azure DevOps to your new app](#connect-azure-devops-to-your-new-app).
5. [Add your Entra ID app to dbt](#add-your-azure-ad-app-to-dbt-cloud).

Once the Microsoft Entra ID app is added to dbt, an account admin must also [connect a service user](#connecting-a-service-user) via OAuth, which will be used to power headless actions in dbt such as deployment runs and CI.

Once the Microsoft Entra ID app is added to dbt and the service user is connected, then dbt developers can personally authenticate in dbt from Azure DevOps. For more on this, see [Authenticate with Azure DevOps](https://docs.getdbt.com/docs/cloud/git/authenticate-azure.md).

The following personas are required to complete the steps on this page:

* Microsoft Entra ID admin
* Azure DevOps admin
* dbt account admin
* Azure admin (if your Entra ID and Azure DevOps environments are not connected)

## Register a Microsoft Entra ID app[​](#register-a-microsoft-entra-id-app "Direct link to Register a Microsoft Entra ID app")

A Microsoft Entra ID admin needs to perform the following steps:

1. Sign into your Azure portal and click **Microsoft Entra ID**.

2. Select **App registrations** in the left panel.

3. Select **New registration**. The form for creating a new Entra ID app opens.

4. Provide a name for your app. We recommend using, "dbt Labs Azure DevOps app".

5. Select **Accounts in any organizational directory (Any Entra ID directory - Multitenant)** as the Supported Account Types. Many customers ask why they need to select Multitenant instead of Single tenant, and they frequently get this step wrong. Microsoft considers Azure DevOps (formerly called Visual Studio) and Microsoft Entra ID as separate tenants, and in order for this Entra ID application to work properly, you must select Multitenant.

6. Add a redirect URI.

   <!-- -->

   1. Select **Web** as the platform.
   2. In the field, enter `https://YOUR_ACCESS_URL/complete/azure_active_directory`. Make sure to replace `YOUR_ACCESS_URL` with the [appropriate Access URL](https://docs.getdbt.com/docs/cloud/about-cloud/access-regions-ip-addresses.md) for your region and plan.

7. Click **Register**.

[![Navigating to the Entra ID app registrations](/img/docs/dbt-cloud/connecting-azure-devops/ADnavigation.gif?v=2 "Navigating to the Entra ID app registrations")](#)Navigating to the Entra ID app registrations

Here's what your app should look like before registering it:

[![Registering a Microsoft Entra ID app](</img/docs/dbt-cloud/connecting-azure-devops/AD app.png?v=2> "Registering a Microsoft Entra ID app")](#)Registering a Microsoft Entra ID app

## Add permissions to your new app[​](#add-permissions-to-your-new-app "Direct link to Add permissions to your new app")

An Entra ID admin needs to provide your new app access to Azure DevOps:

1. Select **API permissions** in the left navigation panel.
2. Remove the **Microsoft Graph / User Read** permission.
3. Click **Add a permission**.
4. Select **Azure DevOps**.
5. Select the **user\_impersonation** permission. This is the only permission available for Azure DevOps.

[![Adding permissions to the app](/img/docs/dbt-cloud/connecting-azure-devops/user-impersonation.gif?v=2 "Adding permissions to the app")](#)Adding permissions to the app

## Add another redirect URI[​](#add-another-redirect-uri "Direct link to Add another redirect URI")

A Microsoft Entra ID admin needs to add another redirect URI to your Entra ID application. This redirect URI will be used to authenticate the service user for headless actions in deployment environments.

Before adding another redirect URI, make sure you selected **Web** as the platform when you [registered the Microsoft Entra ID app](#register-a-microsoft-entra-id-app).

1. Navigate to your Microsoft Entra ID application.

2. Select the link next to **Redirect URIs**.

3. Click **Add URI** and add the URI, replacing `YOUR_ACCESS_URL` with the [appropriate Access URL](https://docs.getdbt.com/docs/cloud/about-cloud/access-regions-ip-addresses.md) for your region and plan: `https://YOUR_ACCESS_URL/complete/azure_active_directory_service_user`

4. Click **Save**.

[![Adding the Service User redirect URI](/img/docs/dbt-cloud/connecting-azure-devops/redirect-uri.gif?v=2 "Adding the Service User redirect URI")](#)Adding the Service User redirect URI

## Create a client secret[​](#create-a-client-secret "Direct link to Create a client secret")

A Microsoft Entra ID admin needs to complete the following steps:

1. Navigate to your Microsoft Entra ID application.
2. Select **Certificates and Secrets** from the left navigation panel.
3. Select **Client secrets** and click **New client secret**
4. Give the secret a description and select the expiration time. Click **Add**.
5. Copy the **Value** field and securely share it with the dbt account admin who will complete the setup.

## Connect Azure DevOps to your new app[​](#connect-azure-devops-to-your-new-app "Direct link to Connect Azure DevOps to your new app")

An Azure admin will need one of the following permissions in both the Microsoft Entra ID and Azure DevOps environments:

* Azure Service Administrator
* Azure Co-administrator

If your Azure DevOps account is connected to Entra ID, then you can proceed to [Connecting a service user](#connecting-a-service-user). However, if you're just getting set up, connect Azure DevOps to the Microsoft Entra ID app you just created:

1. From your Azure DevOps account, select **Organization settings** in the bottom left.
2. Navigate to Microsoft Entra ID.
3. Click **Connect directory**.
4. Select the directory you want to connect.
5. Click **Connect**.

[![Connecting Azure DevOps and Microsoft Entra ID](</img/docs/dbt-cloud/connecting-azure-devops/connect AD to Azure DevOps.gif?v=2> "Connecting Azure DevOps and Microsoft Entra ID")](#)Connecting Azure DevOps and Microsoft Entra ID

## Add your Microsoft Entra ID app to dbt[​](#add-your-microsoft-entra-id-app-to-dbt "Direct link to Add your Microsoft Entra ID app to dbt")

A dbt account admin needs to perform the following steps.

Once you connect your Microsoft Entra ID app and Azure DevOps, you need to provide dbt information about the app:

1. Navigate to your account settings in dbt.

2. Select **Integrations**.

3. Scroll to the Azure DevOps section and click on the pencil icon to edit the integration.

4. Complete the form:

   <!-- -->

   * **Azure DevOps Organization:** Must match the name of your Azure DevOps organization exactly. Do not include the `dev.azure.com/` prefix in this field. ✅ Use `my-devops-org` ❌ Avoid `dev.azure.com/my-devops-org`
   * **Application (client) ID:** Found in the Microsoft Entra ID app.
   * **Client Secrets:** Copy the **Value** field in the Microsoft Entra ID app client secrets and paste it in the **Client Secret** field in dbt. Entra ID admins are responsible for the Entra ID app secret expiration and dbt Admins should note the expiration date for rotation.
   * **Directory(tenant) ID:** Found in the Microsoft Entra ID app.
     <!-- -->
     [![Adding a Microsoft Entra ID app to dbt](/img/docs/dbt-cloud/connecting-azure-devops/AzureDevopsAppdbtCloud.gif?v=2 "Adding a Microsoft Entra ID app to dbt")](#)Adding a Microsoft Entra ID app to dbt

Your Microsoft Entra ID app should now be added to your dbt Account. People on your team who want to develop in the Studio IDE or dbt CLI can now personally [authorize Azure DevOps from their profiles](https://docs.getdbt.com/docs/cloud/git/authenticate-azure.md).

## Connect a service user[​](#connect-a-service-user "Direct link to Connect a service user")

A service user is a pseudo user set up in the same way an admin would set up a real user, but it's given permissions specifically scoped for service to service interactions. You should avoid linking authentication to a real Azure DevOps user because if this person leaves your organization, dbt will lose privileges to the dbt Azure DevOps repositories, causing production runs to fail.

Service user authentication expiration

dbt will refresh the authentication for the service user on each run triggered by the scheduler, API, or CI. If your account does not have any active runs for over 90 days, an admin will need to manually refresh the authentication of the service user by disconnecting and reconnecting the service user's profile via the OAuth flow described above in order to resume headless interactions like project set up, deployment runs, and CI.

### Service users permissions[​](#service-users-permissions "Direct link to Service users permissions")

A service user account must have the following Azure DevOps permissions for all Azure DevOps projects and repos you want accessible in dbt. Read more about how dbt uses each permission in the following paragraphs.

* **Project Reader**
* **ViewSubscriptions**
* **EditSubscriptions**
* **DeleteSubscriptions** \*
* **PullRequestContribute**
* **GenericContribute**

\* Note: **DeleteSubscriptions** permission might be included in **EditSubscriptions** depending on your version of Azure.

Some of these permissions are only accessible via the [Azure DevOps API](https://docs.microsoft.com/en-us/azure/devops/organizations/security/namespace-reference?view=azure-devops) or [CLI](https://learn.microsoft.com/en-us/cli/azure/devops?view=azure-cli-latest). We’ve also detailed more information on Azure DevOps API usage below to help accelerate the setup. Alternatively, you can use the Azure DevOps UI to enable permissions, but you cannot get the least permissioned set.

* Required permissions for service users
* Turn off MFA for service user

The service user's permissions will also power which repositories a team can select from during dbt project set up, so an Azure DevOps admin must grant at minimum Project Reader access to the service user *before* creating a new project in dbt. If you are migrating an existing dbt project to use the native Azure DevOps integration, the dbt account's service user must have proper permissions on the repository before migration.

While it's common to enforce multi-factor authentication (MFA) for normal user accounts, service user authentication must not need an extra factor. If you enable a second factor for the service user, this can interrupt production runs and cause a failure to clone the repository. In order for the OAuth access token to work, the best practice is to remove any more burden of proof of identity for service users.

As a result, MFA must be explicity disabled in the Office 365 or Microsoft Entra ID administration panel for the service user. Just having it "un-connected" will not be sufficient, as dbt will be prompted to set up MFA instead of allowing the credentials to be used as intended.

**To disable MFA for a single user using the Office 365 Administration console:**

* Go to Microsoft 365 admin center -> Users -> Active users -> Select the user -> Manage multifactor authentication -> Select the user -> Disable multi-factor authentication.

**To use the Microsoft Entra ID interface:**

Note, this procedure involves disabling Security Defaults in your Entra ID environment.

1. Go to the Azure Admin Center. Open Microsoft Entra ID and under the **Manage** section of the left navigation, click **Properties**, scroll down to **Manage Security defaults**, and then select **No** in "Enable Security Defaults" and click **Save**.
2. Go to **Microsoft Entra ID** -> Manage -> Users ->click on the ellipsis (...) and then the **Multi-Factor Authentication** link. If the link is grayed out, you need to make sure you disable **Security Defaults** from the previous step.
3. If MFA is enabled for users, select the user(s) and select **Disable** under **Quick steps**.
4. Select **Yes** to confirm your changes.

To re-enable MFA for the user, select them again and click **Enable**. Note, you may have to go through MFA setup for that user after enabling it.

**ViewSubscriptions**

<br />

**Security Namespace ID:** cb594ebe-87dd-4fc9-ac2c-6a10a4c92046

**Namespace:** ServiceHooks

**Permission:**

```json
{
    "bit": 1,
    "displayName": "View Subscriptions",
    "name": "ViewSubscriptions"
}
```

**Uses:** To view existing Azure DevOps service hooks subscriptions

**Token (where applicable - API only):**

* PublisherSecurity for access to all projects
* PublisherSecurity/\<azure\_devops\_project\_object\_id> for per project access

**UI/API/CLI:** API/CLI only

**Sample CLI code snippet**

```bash
az devops security permission update --organization https://dev.azure.com/<org_name> --namespace-id cb594ebe-87dd-4fc9-ac2c-6a10a4c92046 --subject <service_account>@xxxxxx.onmicrosoft.com --token PublisherSecurity/<azure_devops_project_object_id> --allow-bit 1
```

**EditSubscriptions**

<br />

**Security Namespace ID:** cb594ebe-87dd-4fc9-ac2c-6a10a4c92046

**Namespace:** ServiceHooks

**Permission:**

```json
{
    "bit": 2,
    "displayName": "Edit Subscription",
    "name": "EditSubscriptions"
}
```

**Uses:** To add or update existing Azure DevOps service hooks subscriptions

**Token (where applicable - API only):**

* PublisherSecurity for access to all projects
* PublisherSecurity/\<azure\_devops\_project\_object\_id> for per project access

**UI/API/CLI:** API/CLI only

**Sample CLI code snippet**

```bash
az devops security permission update --organization https://dev.azure.com/<org_name> --namespace-id cb594ebe-87dd-4fc9-ac2c-6a10a4c92046 --subject <service_account>@xxxxxx.onmicrosoft.com --token PublisherSecurity/<azure_devops_project_object_id> --allow-bit 2
```

**DeleteSubscriptions**

<br />

**Security Namespace ID:** cb594ebe-87dd-4fc9-ac2c-6a10a4c92046

**Namespace:** ServiceHooks

**Permission:**

```json
{
    "bit": 4,
    "displayName": "Delete Subscriptions",
    "name": "DeleteSubscriptions"
}
```

**Uses:** To delete any redundant Azure DevOps service hooks subscriptions

**Token (where applicable - API only):**

* PublisherSecurity for access to all projects
* PublisherSecurity/\<azure\_devops\_project\_object\_id> for per project access

**UI/API/CLI:** API/CLI only

**Sample CLI code snippet**

```bash
az devops security permission update --organization https://dev.azure.com/<org_name> --namespace-id cb594ebe-87dd-4fc9-ac2c-6a10a4c92046 --subject <service_account>@xxxxxx.onmicrosoft.com --token PublisherSecurity/<azure_devops_project_object_id> --allow-bit 4
```

**Additional Notes:** This permission has been deprecated in recent Azure DevOps versions. Edit Subscriptions (bit 2) has Delete permissions.

**PullRequestContribute**

<br />

**Security Namespace ID:** 2e9eb7ed-3c0a-47d4-87c1-0ffdd275fd87

**Namespace:** Git Repositories

**Permission:**

```json
{ 	
    "bit": 16384,  
    "displayName": "Contribute to pull requests",
    "name": "PullRequestContribute"
}
```

**Uses:** To post Pull Request statuses to Azure DevOps

**Token (where applicable - API only):**

* repoV2 for access to all projects
* repoV2/\<azure\_devops\_project\_object\_id> for per project access
* repoV2/\<azure\_devops\_project\_object\_id>/\<azure\_devops\_repository\_object\_id> for per repo access

**UI/API/CLI:** UI, API, and CLI

**Sample CLI code snippet**

```bash
az devops security permission update --organization https://dev.azure.com/<org_name> --namespace-id 2e9eb7ed-3c0a-47d4-87c1-0ffdd275fd87 --subject <service_account>@xxxxxx.onmicrosoft.com --token repoV2/<azure_devops_project_object_id>/<azure_devops_repository_object_id> --allow-bit 16384
```

**Additional Notes:** This permission is automatically inherited if Project Reader/Contributor/Administrator is set in the UI.

**GenericContribute**

<br />

**Security Namespace ID:** 2e9eb7ed-3c0a-47d4-87c1-0ffdd275fd87

**Namespace:** Git Repositories

**Permission:**

```json
{
    "bit": 4,
    "displayName": "Contribute",
    "name": "GenericContribute"
}
```

**Uses:** To post commit statuses to Azure DevOps

**Token (where applicable - API only):**

* repoV2 for access to all projects
* repoV2/\<azure\_devops\_project\_object\_id> for access to a single project at a time
* repoV2/\<azure\_devops\_project\_object\_id>/\<azure\_devops\_repository\_object\_id> for access to a single repo at a time

**UI/API/CLI:** UI, API, and CLI

**Sample CLI code snippet**

```bash
az devops security permission update --organization https://dev.azure.com/<org_name> --namespace-id 2e9eb7ed-3c0a-47d4-87c1-0ffdd275fd87 --subject <service_account>@xxxxxx.onmicrosoft.com --token repoV2/<azure_devops_project_object_id>/<azure_devops_repository_object_id> --allow-bit 4
```

**Additional Notes:** This permission is automatically inherited if Project Contributor/Administrator is set in the UI.

You must connect your service user before setting up a dbt project, as the service user's permissions determine which projects dbt can import.

A dbt account admin with access to the service user's Azure DevOps account must complete the following to connect the service user:

1. Sign in to the service user's Azure DevOps account.
2. In dbt, go to **Account settings** > **Integrations**.
3. Go to the **Azure DevOps** section and select **Service User**.
4. Enter values for the required fields.
5. Click **Save**.
6. Click **Link Azure service user**.
7. You will be directed to Azure DevOps and you must accept the Microsoft Entra ID app's permissions.
8. Finally, you will be redirected to dbt, and the service user will be connected.

[![Connecting an Azure Service User](/img/docs/dbt-cloud/connecting-azure-devops/azure-service-user.png?v=2 "Connecting an Azure Service User")](#)Connecting an Azure Service User

Once connected, dbt displays the email address of the service user so you know which user's permissions are enabling headless actions in deployment environments. To change which account is connected, disconnect the profile in dbt, sign into the alternative Azure DevOps service account, and re-link the account in dbt.

Personal Access Tokens (PATs)

dbt leverages the service user to generate temporary access tokens called [PATs](https://learn.microsoft.com/en-us/azure/devops/organizations/accounts/use-personal-access-tokens-to-authenticate?toc=%2Fazure%2Fdevops%2Fmarketplace-extensibility%2Ftoc.json\&view=azure-devops\&tabs=Windows).

These tokens are limited in scope, are only valid for 5 minutes, and become invalid after a single API call.

These tokens are limited to the following [scopes](https://learn.microsoft.com/en-us/azure/devops/integrate/get-started/authentication/oauth?view=azure-devops):

* `vso.code_full`: Grants full access to source code and version control metadata (commits, branches, and so on). Also grants the ability to create and manage code repositories, create and manage pull requests and code reviews, and receive notifications about version control events with service hooks. Also includes limited support for Client OM APIs.
* `vso.project`: Grants the ability to read projects and teams.
* `vso.build_execute`: Grants the ability to access build artifacts, including build results, definitions, and requests, and the ability to queue a build, update build properties, and the ability to receive notifications about build events with service hooks.

## Was this page helpful?

YesNo

[Privacy policy](https://www.getdbt.com/cloud/privacy-policy)[Create a GitHub issue](https://github.com/dbt-labs/docs.getdbt.com/issues)

This site is protected by reCAPTCHA and the Google [Privacy Policy](https://policies.google.com/privacy) and [Terms of Service](https://policies.google.com/terms) apply.
