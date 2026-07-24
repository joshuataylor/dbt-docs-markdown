# Set up SSO with Okta

dbt platform | Enterprise, Enterprise+ⓘ

dbt Enterprise-tier plans support single-sign on via Okta (using SAML).

SCIM available for Okta

After setting up single sign-on (SSO), you can [set up System for Cross-Domain Identity Management (SCIM)](https://docs.getdbt.com/docs/platform/manage-access/scim-okta.md) with Okta to automate user and group provisioning, and license assignment.

Currently supported SSO features include:

* IdP-initiated SSO
* SP-initiated SSO
* Just-in-time provisioning

This guide outlines the setup process for authenticating to dbt with Okta.

## Configuration in Okta[​](#configuration-in-okta "Direct link to Configuration in Okta")

### Create a new application[​](#create-a-new-application "Direct link to Create a new application")

Note: You'll need administrator access to your Okta organization to follow this guide.

First, log into your Okta account. Using the Admin dashboard, create a new app.

[![Create a new app](/img/docs/dbt-platform/dbt-platform-enterprise/okta/okta-1-new-app.png?v=2 "Create a new app")](#)Create a new app

On the following screen, select the following configurations:

* **Platform**: Web
* **Sign on method**: SAML 2.0

Click **Create** to continue the setup process.

[![Configure a new app](/img/docs/dbt-platform/dbt-platform-enterprise/okta/okta-1-new-app-create.png?v=2 "Configure a new app")](#)Configure a new app

### Configure the Okta application[​](#configure-the-okta-application "Direct link to Configure the Okta application")

On the **General Settings** page, enter the following details::

* **App name**: dbt
* **App logo** (optional): You can optionally [download the dbt logo](https://cdn.sanity.io/images/wl0ndo6t/main/333fef4fc72db6f1ce4d1bc0789f355b4f0bbaa2-1280x1280.png), and upload it to Okta to use as the logo for this app.

Click **Next** to continue.

[![Configure the app's General Settings](/img/docs/dbt-platform/dbt-platform-enterprise/okta/okta-2-general-settings.png?v=2 "Configure the app's General Settings")](#)Configure the app's General Settings

### Configure SAML Settings[​](#configure-saml-settings "Direct link to Configure SAML Settings")

The SAML Settings page configures how Okta and dbt communicate. You will want to use an [appropriate Access URL](https://docs.getdbt.com/docs/platform/about-platform/access-regions-ip-addresses.md) for your region and plan.

<!-- -->

To complete this section, you will need your login URL slug. This slug controls the URL where users on your account can log into your application. dbt automatically generates login URL slugs, which can't be altered. It will contain only letters, numbers, and dashes. For example, the login URL slug for dbt Labs would look something like `dbt-labs-afk123`. Login URL slugs are unique across all dbt accounts.

Users can also sign in at <https://login.dbt.com> to see accounts they have access to across instances. The enterprise login URL that includes your slug remains the URL used for IdP-initiated SSO flows with your identity provider.

The following steps use `YOUR_AUTH0_URI` and `YOUR_AUTH0_ENTITYID`. Replace these placeholders with the [appropriate Auth0 URI and Auth0 Entity ID](https://docs.getdbt.com/docs/platform/manage-access/sso-overview.md#auth0-uris) for your region. You can find these values in **Account settings** > **SSO & SCIM** > **Edit** or **Get started** after selecting your identity provider.

* **Single sign on URL**: `https://YOUR_AUTH0_URI/login/callback?connection=<login URL slug>`
* **Audience URI (SP Entity ID)**: `urn:auth0:<YOUR_AUTH0_ENTITYID>:{login URL slug}`
* **Relay State**: `<login URL slug>`
* **Name ID format**: `Unspecified`
* **Application username**: `Custom` / `user.getInternalProperty("id")`
* **Update Application username on**: `Create and update`

[![Configure the app's SAML Settings](/img/docs/dbt-platform/dbt-platform-enterprise/okta/okta-3-saml-settings-top.png?v=2 "Configure the app's SAML Settings")](#)Configure the app's SAML Settings

Application username configuration

The **Application username** setting depends on whether you plan to use SCIM or not:

* **SSO only:** Use a unique value such as `Custom` / `user.getInternalProperty("id")` (recommended in earlier steps).
* **SSO and SCIM:** Use **Email** format. SCIM requires the username to be in email address format and to be the same value as the email attribute.

Use the **Attribute Statements** and **Group Attribute Statements** forms to map your organization's Okta User and Group Attributes to the format that dbt expects.

Expected **User Attribute Statements**:

| Name         | Name format | Value            | Description                |
| ------------ | ----------- | ---------------- | -------------------------- |
| `email`      | Unspecified | `user.email`     | *The user's email address* |
| `first_name` | Unspecified | `user.firstName` | *The user's first name*    |
| `last_name`  | Unspecified | `user.lastName`  | *The user's last name*     |

Search table...

|                  |   |   |   |   |
| ---------------- | - | - | - | - |
| Loading table... |   |   |   |   |

Expected **Group Attribute Statements**:

| Name     | Name format | Filter        | Value | Description                           |
| -------- | ----------- | ------------- | ----- | ------------------------------------- |
| `groups` | Unspecified | Matches regex | `.*`  | *The groups that the user belongs to* |

Search table...

|                  |   |   |   |   |
| ---------------- | - | - | - | - |
| Loading table... |   |   |   |   |

**Note:** You may use a more restrictive Group Attribute Statement than the example shown above. For example, if all of your dbt groups start with `DBT_CLOUD_`, you may use a filter like `Starts With: DBT_CLOUD_`. **Okta only returns 100 groups for each user, so if your users belong to more than 100 IdP groups, you will need to use a more restrictive filter**. Please contact support if you have any questions.

[![Configure the app's User and Group Attribute Statements](/img/docs/dbt-platform/dbt-platform-enterprise/okta/okta-3-saml-settings-bottom.png?v=2 "Configure the app's User and Group Attribute Statements")](#)Configure the app's User and Group Attribute Statements

Click **Next** to continue.

### Finish Okta setup[​](#finish-okta-setup "Direct link to Finish Okta setup")

Select *I'm an Okta customer adding an internal app*, and select *This is an internal app that we have created*. Click **Finish** to finish setting up the app.

[![Finishing setup in Okta](/img/docs/dbt-platform/dbt-platform-enterprise/okta/okta-4-feedback.png?v=2 "Finishing setup in Okta")](#)Finishing setup in Okta

### View setup instructions[​](#view-setup-instructions "Direct link to View setup instructions")

On the next page, click **View Setup Instructions**. In the steps below, you'll supply these values in your dbt Account Settings to complete the integration between Okta and dbt.

[![Viewing the configured application](/img/docs/dbt-platform/dbt-platform-enterprise/okta/okta-5-view-instructions.png?v=2 "Viewing the configured application")](#)Viewing the configured application

[![Application setup instructions](/img/docs/dbt-platform/dbt-platform-enterprise/okta/okta-5-instructions.png?v=2 "Application setup instructions")](#)Application setup instructions

## Configuration in dbt[​](#configuration-in-dbt "Direct link to Configuration in dbt")

To complete setup, follow the steps below in dbt.

### Supplying credentials[​](#supplying-credentials "Direct link to Supplying credentials")

1. Navigate to **Account settings** > **SSO & SCIM**.
2. Click **Get started** if SSO has not been configured, or **Edit** if it has already been set up.
3. Supply the following SSO details:

| Field                         | Value                                                                                                                                                                                                                        |
| ----------------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **Log in with**               | Okta                                                                                                                                                                                                                         |
| **Identity Provider SSO Url** | Paste the **Identity Provider Single Sign-On URL** shown in the Okta setup instructions                                                                                                                                      |
| **Identity Provider Issuer**  | Paste the **Identity Provider Issuer** shown in the Okta setup instructions                                                                                                                                                  |
| **X.509 Certificate**         | Paste the **X.509 Certificate** shown in the Okta setup instructions;<br />**Note:** When the certificate expires, an Okta admin will have to generate a new one to be pasted into dbt for uninterrupted application access. |

Search table...

|                  |   |   |   |   |
| ---------------- | - | - | - | - |
| Loading table... |   |   |   |   |

[![Configuring the application in dbt](/img/docs/dbt-platform/dbt-platform-enterprise/okta/okta-6-setup-integration.png?v=2 "Configuring the application in dbt")](#)Configuring the application in dbt

4. Click **Save** to complete setup for the Okta integration. From here, you can navigate to the URL generated for your account's *slug* to test logging in with Okta. Additionally, users added the Okta app will be able to log in to dbt from Okta directly.

Logging in

Users can sign in at <https://login.dbt.com> to view accounts they have access to across instances and choose where to open dbt platform. This is the recommended entry point for most users.

For SSO through your identity provider, you can also use the following URL format with your account login URL slug. Replace `LOGIN_SLUG` with the value from the previous steps and `YOUR_ACCESS_URL` with the [appropriate Access URL](https://docs.getdbt.com/docs/platform/about-platform/access-regions-ip-addresses.md) for your region and plan:

`https://YOUR_ACCESS_URL/enterprise-login/LOGIN-SLUG`

Account administrators can turn account discovery on or off with **Enable global account discovery** in [Account settings](https://docs.getdbt.com/docs/platform/account-settings.md#enable-global-account-discovery).

## Setting up RBAC[​](#setting-up-rbac "Direct link to Setting up RBAC")

Now you have completed setting up SSO with Okta, the next steps will be to set up [RBAC groups](https://docs.getdbt.com/docs/platform/manage-access/about-user-access.md#role-based-access-control-) to complete your access control configuration.

Set up SCIM

Now that you've set up SSO with Okta, you can [set up SCIM](https://docs.getdbt.com/docs/platform/manage-access/scim-okta.md) to automate user and group provisioning (and license assignment for Okta).

## FAQ and troubleshooting[​](#faq-and-troubleshooting "Direct link to FAQ and troubleshooting")

For common questions and troubleshooting guidance — including issues with email verification, MFA prompts, and SSO group mapping behavior — refer to [SSO FAQs and troubleshooting](https://docs.getdbt.com/docs/platform/manage-access/sso-faq.md).

## Learn more[​](#learn-more "Direct link to Learn more")
