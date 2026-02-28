# Set up external OAuth with Snowflake [Enterprise](https://www.getdbt.com/pricing "Go to https://www.getdbt.com/pricing")[Enterprise +](https://www.getdbt.com/pricing "Go to https://www.getdbt.com/pricing")

<!-- -->

note

This feature is currently only available for Okta and Entra ID identity providers.

dbt Enterprise and Enterprise+ plans support OAuth authentication with external providers. When **External OAuth** is enabled, users can authorize their Development credentials using single sign-on (SSO) via the identity provider (IdP). External OAuth authorizes users to access multiple applications, including dbt, without sharing their static credentials with the service. This makes the process of authenticating for development environments easier for the user and provides an additional layer of security to your dbt account.

## Getting started[​](#getting-started "Direct link to Getting started")

The process of setting up external OAuth will require a little bit of back-and-forth between your dbt, IdP, and data warehouse accounts, and having them open in multiple browser tabs will help speed up the configuration process:

* **dbt:** You’ll primarily be working in the **Account settings** —> **Integrations** page. You will need [proper permission](https://docs.getdbt.com/docs/cloud/manage-access/enterprise-permissions.md) to set up the integration and create the connections.

**Identity providers:**

* **Okta:** You’ll be working in multiple areas of the Okta account, but you can start in the **Applications** section. You will need permissions to [create an application](https://help.okta.com/en-us/content/topics/security/custom-admin-role/about-role-permissions.htm#Application_permissions) and an [authorization server](https://help.okta.com/en-us/content/topics/security/custom-admin-role/about-role-permissions.htm#Authorization_server_permissions).
* **Entra ID** An admin with access to create [Entra ID apps](https://learn.microsoft.com/en-us/entra/identity/role-based-access-control/custom-available-permissions) who is also a user in the data warehouse is required.

**Data warehouse:**

* **Snowflake:** Open a worksheet in an account that has permissions to [create a security integration](https://docs.snowflake.com/en/sql-reference/sql/create-security-integration).

If the admins that handle these products are all different people, it’s better to have them coordinating simultaneously to reduce friction.

Snowflake and IdP username matching required

Ensure that the username/email address entered by the IdP admin matches the Snowflake credentials username. If the email address used in the dbt setup is different from the Snowflake email address, the connection will fail or you may run into issues.

## Data warehouse configurations[​](#data-warehouse-configurations "Direct link to Data warehouse configurations")

The following is a template for creating the OAuth configurations in the Snowflake environment:

```sql

create security integration your_integration_name
type = external_oauth
enabled = true
external_oauth_type = okta
external_oauth_issuer = ''
external_oauth_jws_keys_url = ''
external_oauth_audience_list = ('')
external_oauth_token_user_mapping_claim = 'sub'
external_oauth_snowflake_user_mapping_attribute = 'email_address'
external_oauth_any_role_mode = 'ENABLE'
```

The `external_oauth_token_user_mapping_claim` and `external_oauth_snowflake_user_mapping_attribute` can be modified based on the your organizations needs. These values point to the claim in the users’ token. In the example, Snowflake will look up the Snowflake user whose `email` matches the value in the `sub` claim.

**Notes:**

* The Snowflake default roles ACCOUNTADMIN, ORGADMIN, or SECURITYADMIN, are blocked from external OAuth by default and they will likely fail to authenticate. See the [Snowflake documentation](https://docs.snowflake.com/en/sql-reference/sql/create-security-integration-oauth-external) for more information.
* The value for `external_oauth_snowflake_user_mapping_attribute` must map correctly to the Snowflake username. For example, if `email_address` is used, the email in the token from the IdP must match the Snowflake username exactly.

## Identity provider configuration[​](#identity-provider-configuration "Direct link to Identity provider configuration")

Select a supported identity provider (IdP) for instructions on configuring external OAuth in their environment and completing the integration in dbt:

* Okta
* Entra ID

### 1. Initialize the dbt settings[​](#1-initialize-the-dbt-settings "Direct link to 1. Initialize the dbt settings")

1. In your dbt account, navigate to **Account settings** —> **Integrations**.
2. Scroll down to **Custom integrations** and click **Add integrations**
3. Leave this window open. You can set the **Integration type** to Okta and note the **Redirect URI** at the bottom of the page. Copy this to your clipboard for use in the next steps.

[![Copy the callback URI at the bottom of the integration page in dbt.](/img/docs/dbt-cloud/callback-uri.png?v=2 "Copy the callback URI at the bottom of the integration page in dbt.")](#)Copy the callback URI at the bottom of the integration page in dbt.

### 2. Create the Okta app[​](#2-create-the-okta-app "Direct link to 2. Create the Okta app")

1. Expand the **Applications** section from the Okta dashboard and click **Applications.** Click the **Create app integration** button.
2. Select **OIDC** as the sign-in method and **Web applications** as the application type. Click **Next**.

[![The Okta app creation window with OIDC and Web Application selected.](/img/docs/dbt-cloud/create-okta-app.png?v=2 "The Okta app creation window with OIDC and Web Application selected.")](#)The Okta app creation window with OIDC and Web Application selected.

3. Give the application an appropriate name, something like “External OAuth app for dbt,” that will make it easily identifiable.
4. In the **Grant type** section, enable the **Refresh token** option.
5. Scroll down to the **Sign-in redirect URIs** option. You’ll need to paste the redirect URI you gathered from dbt in step 1.3.

[![The Okta app configuration window with the sign-in redirect URI configured to the dbt value.](/img/docs/dbt-cloud/configure-okta-app.png?v=2 "The Okta app configuration window with the sign-in redirect URI configured to the dbt value.")](#)The Okta app configuration window with the sign-in redirect URI configured to the dbt value.

6. Save the app configuration. You’ll come back to it, but move on to the next steps for now.

### 3. Create the Okta API[​](#3-create-the-okta-api "Direct link to 3. Create the Okta API")

1. Expand the **Security** section and click **API** from the Okta sidebar menu.
2. On the API screen, click **Add authorization server**. Give the authorization server a name (a nickname for your data warehouse account would be appropriate). For the **Audience** field, copy and paste your data warehouse login URL (for example, <https://abdc-ef1234.snowflakecomputing.com>). Give the server an appropriate description and click **Save**.

[![The Okta API window with the Audience value set.](/img/docs/dbt-cloud/create-okta-api.png?v=2 "The Okta API window with the Audience value set.")](#)The Okta API window with the Audience value set.

3. On the authorization server config screen, open the **Metadata URI** in a new tab. You’ll need information from this screen in later steps.

[![The Okta API settings page with the metadata URI highlighted.](/img/docs/dbt-cloud/metadata-uri.png?v=2 "The Okta API settings page with the metadata URI highlighted.")](#)The Okta API settings page with the metadata URI highlighted.

[![Sample output of the metadata URI.](/img/docs/dbt-cloud/metadata-example.png?v=2 "Sample output of the metadata URI.")](#)Sample output of the metadata URI.

4. Click on the **Scopes** tab and **Add scope**. In the **Name** field, add `session:role-any`. (Optional) Configure **Display phrase** and **Description** and click **Create**.

[![API scope configured in the Add Scope window.](/img/docs/dbt-cloud/add-api-scope.png?v=2 "API scope configured in the Add Scope window.")](#)API scope configured in the Add Scope window.

5. Open the **Access policies** tab and click **Add policy**. Give the policy a **Name** and **Description** and set **Assign to** as **The following clients**. Start typing the name of the app you created in step 2.3, and you’ll see it autofill. Select the app and click **Create Policy**.

[![Assignment field autofilling the value.](/img/docs/dbt-cloud/add-api-assignment.png?v=2 "Assignment field autofilling the value.")](#)Assignment field autofilling the value.

6. On the **access policy** screen, click **Add rule**.

[![API Add rule button highlighted.](/img/docs/dbt-cloud/add-api-rule.png?v=2 "API Add rule button highlighted.")](#)API Add rule button highlighted.

7. Give the rule a descriptive name and scroll down to **token lifetimes**. Configure the **Access token lifetime is**, **Refresh token lifetime is**, and **but will expire if not used every** settings according to your organizational policies. We recommend the defaults of 1 hour and 90 days. Stricter rules increase the odds of your users having to re-authenticate.

[![Token lifetime settings in the API rule window.](/img/docs/dbt-cloud/configure-token-lifetime.png?v=2 "Token lifetime settings in the API rule window.")](#)Token lifetime settings in the API rule window.

8. Navigate back to the **Settings** tab and leave it open in your browser. You’ll need some of the information in later steps.

### 4. Create the OAuth settings in the data warehouse[​](#4-create-the-oauth-settings-in-the-data-warehouse "Direct link to 4. Create the OAuth settings in the data warehouse")

1. Open up a Snowflake worksheet and copy/paste the following:

```sql

create security integration your_integration_name
type = external_oauth
enabled = true
external_oauth_type = okta
external_oauth_issuer = ''
external_oauth_jws_keys_url = ''
external_oauth_audience_list = ('')
external_oauth_token_user_mapping_claim = 'sub'
external_oauth_snowflake_user_mapping_attribute = 'email_address'
external_oauth_any_role_mode = 'ENABLE'
```

2. Change `your_integration_name` to something appropriately descriptive. For example, `dev_OktaAccountNumber_okta`. Copy the `external_oauth_issuer` and `external_oauth_jws_keys_url` from the metadata URI in step 3.3. Use the same Snowflake URL you entered in step 3.2 as the `external_oauth_audience_list`.

Adjust the other settings as needed to meet your organization's configurations in Okta and Snowflake.

[![The issuer and jws keys URIs in the metadata URL](/img/docs/dbt-cloud/gather-uris.png?v=2 "The issuer and jws keys URIs in the metadata URL")](#)The issuer and jws keys URIs in the metadata URL

3. Run the steps to create the integration in Snowflake.

Username consistency

Ensure that the username (for example, email address) entered in the IdP matches the Snowflake credentials for all users. Mismatched usernames will result in authentication failures.

### 5. Configuring the integration in dbt[​](#5-configuring-the-integration-in-dbt "Direct link to 5. Configuring the integration in dbt")

1. Navigate back to the dbt **Account settings** —> **Integrations** page you were on at the beginning. It’s time to start filling out all of the fields.

   1. `Integration name`: Give the integration a descriptive name that includes identifying information about the Okta environment so future users won’t have to guess where it belongs.
   2. `Client ID` and `Client secrets`: Retrieve these from the Okta application page.

   [![The client ID and secret highlighted in the Okta app.](/img/docs/dbt-cloud/gather-clientid-secret.png?v=2 "The client ID and secret highlighted in the Okta app.")](#)The client ID and secret highlighted in the Okta app.

   3. Authorize URL and Token URL: Found in the metadata URI.

   [![The authorize and token URLs highlighted in the metadata URI.](/img/docs/dbt-cloud/gather-authorization-token-endpoints.png?v=2 "The authorize and token URLs highlighted in the metadata URI.")](#)The authorize and token URLs highlighted in the metadata URI.

2. **Save** the configuration

### 6. Create a new connection in dbt[​](#6-create-a-new-connection-in-dbt "Direct link to 6. Create a new connection in dbt")

1. Navigate to **Account settings** and click **Connections** from the menu. Click **New connection**.
2. Configure the `Account`, `Database`, and `Warehouse` as you normally would, and for the `OAuth method`, select the external OAuth you just created.

[![The new configuration window in dbt with the External OAuth showing as an option.](/img/docs/dbt-cloud/configure-new-connection.png?v=2 "The new configuration window in dbt with the External OAuth showing as an option.")](#)The new configuration window in dbt with the External OAuth showing as an option.

3. Scroll down to the **External OAuth** configurations box and select the config from the list.

[![The new connection displayed in the External OAuth Configurations box.](/img/docs/dbt-cloud/select-oauth-config.png?v=2 "The new connection displayed in the External OAuth Configurations box.")](#)The new connection displayed in the External OAuth Configurations box.

4. **Save** the connection, and you have now configured External OAuth with Okta!

### 1. Initialize the dbt settings[​](#1-initialize-the-dbt-settings-1 "Direct link to 1. Initialize the dbt settings")

1. In your dbt account, navigate to **Account settings** —> **Integrations**.
2. Scroll down to **Custom integrations** and click **Add integrations**.
3. Leave this window open. You can set the **Integration type** to Entra ID and note the **Redirect URI** at the bottom of the page. Copy this to your clipboard for use in the next steps.

### 2. Create the Entra ID apps[​](#2-create-the-entra-id-apps "Direct link to 2. Create the Entra ID apps")

* You’ll create two apps in the Azure portal: A resource server and a client app.
* In your Azure portal, open the **Entra ID** and click **App registrations** from the left menu.

important

* You need both an Entra ID admin and a data warehouse admin to complete the setup. These roles don’t need to be the same person — as long as they collaborate, everything should work smoothly.
  <!-- -->
  * Typically, the Entra ID admin handles app registration and permissions, while the data warehouse admin manages roles, grants, and integrations on the warehouse side.
* The `value` field gathered in these steps is only displayed once. When created, record it immediately.
* Ensure that the username (for example, email address) entered in the IdP matches the data warehouse credentials for all users. Mismatched usernames will result in authentication failures.

### 3. Create a resource server[​](#3-create-a-resource-server "Direct link to 3. Create a resource server")

1. From the app registrations screen, click **New registration**.

   <!-- -->

   1. Give the app a name.
   2. Ensure **Supported account types** are set to “Accounts in this organizational directory only (`Org name` - Single Tenant).”
   3. Click **Register**to see the application’s overview.

2. From the app overview page, click **Expose an API** from the left menu.

3. Click **Add** next to **Application ID URI**. The field will automatically populate. Click **Save**.

4. Record the `value` field for use in a future step. *This is only displayed once. Be sure to record it immediately. Microsoft hides the field when you leave the page and come back.*

5. From the same screen, click **Add scope**.

   <!-- -->

   1. Name the scope `session:role-any`.
   2. Set “Who can consent?” to **Admins and users**.
   3. Set **Admin consent display name** to `session:role-any` and give it a description.
   4. Ensure **State** is set to **Enabled**.
   5. Click **Add scope**.

### 4. Create a client app[​](#4-create-a-client-app "Direct link to 4. Create a client app")

1. From the **App registration page**, click **New registration**.

   <!-- -->

   1. Give the app a name that uniquely identifies it as the client app.
   2. Ensure **Supported account types** are set to “Accounts in this organizational directory only (`Org name` - Single Tenant).”
   3. Set the **Redirect URI** to **Web** and copy/paste the **Redirect URI** from dbt into the field.
   4. Click **Register**.

2. From the app overview page, click **API permissions** from the left menu, and click **Add permission**.

3. From the pop-out screen, click **APIs my organization uses**, search for the resource server name from the previous steps, and click it.

4. Ensure the box for the **Permissions** `session:role-any` is enabled and click **Add permissions**.

5. Click **Grant admin consent** and from the popup modal click **Yes**.

6. From the left menu, click **Certificates and secrets** and click **New client secret**. Name the secret, set an expiration, and click **Add**. **Note**: Microsoft does not allow “forever” as an expiration date. The maximum time is two years. Documenting the expiration date so you can refresh the secret before the expiration or user authorization fails is essential.

7. Record the `value` for use in a future step and record it immediately. **Note**: Entra ID will not display this value again once you navigate away from this screen.

### 5. Data warehouse configuration[​](#5-data-warehouse-configuration "Direct link to 5. Data warehouse configuration")

You'll be switching between the Entra ID site and Snowflake. Keep your Entra ID account open for this process.

Copy and paste the following as a template in a Snowflake worksheet:

```sql

create or replace security integration <whatever you want to name it>
   type = external_oauth
   enabled = true
   external_oauth_type = azure
   external_oauth_issuer = '<AZURE_AD_ISSUER>'
   external_oauth_jws_keys_url = '<AZURE_AD_JWS_KEY_ENDPOINT>'
   external_oauth_audience_list = ('<SNOWFLAKE_APPLICATION_ID_URI>')
   external_oauth_token_user_mapping_claim = 'upn'
   external_oauth_any_role_mode = 'ENABLE'
   external_oauth_snowflake_user_mapping_attribute = 'login_name';
```

On the Entra ID site:

1. From the Client ID app in Entra ID, click **Endpoints** and open the **Federation metadata document** in a new tab.
   <!-- -->
   * The **entity ID** on this page maps to the `external_oauth_issuer` field in the Snowflake config.
2. Back on the list of endpoints, open the **OpenID Connect metadata document** in a new tab.
   <!-- -->
   * The **jwks\_uri** field maps to the `external_oauth_jws_keys_url` field in Snowflake.
3. Navigate to the resource server in previous steps.
   <!-- -->
   * The **Application ID URI** maps to the `external_oauth_audience_list` field in Snowflake.
4. Run the configurations. You need both an Entra ID admin and a data warehouse admin to complete the setup. If these admins are not the same person, they should work together to complete the configuration.

### 6. Configuring the integration in dbt[​](#6-configuring-the-integration-in-dbt "Direct link to 6. Configuring the integration in dbt")

1. Navigate back to the dbt **Account settings** —> **Integrations** page you were on at the beginning. It’s time to start filling out all of the fields. There will be some back-and-forth between the Entra ID account and dbt.
2. `Integration name`: Give the integration a descriptive name that includes identifying information about the Entra ID environment so future users won’t have to guess where it belongs.
3. `Client secrets`: Found in the Client ID from the **Certificates and secrets** page. `Value` is the `Client secret`. Note that it only appears when created; *Microsoft hides the secret if you return later, and you must recreate it.*
4. `Client ID`: Copy the’ Application (client) ID’ on the overview page for the client ID app.
5. `Authorization URL` and `Token URL`: From the client ID app, open the `Endpoints` tab. These URLs map to the `OAuth 2.0 authorization endpoint (v2)` and `OAuth 2.0 token endpoint (v2)` fields. *You must use v2 of the `OAuth 2.0 authorization endpoint`. Do not use V1.* You can use either version of the `OAuth 2.0 token endpoint`.
6. `Application ID URI`: Copy the `Application ID URI` field from the resource server’s Overview screen.

## FAQs[​](#faqs "Direct link to FAQs")

Receiving a \`Failed to connect to DB\` error when connecting to Snowflake

1. If you see the following error:

   ```text
   Failed to connect to DB: xxxxxxx.snowflakecomputing.com:443. The role requested in the connection, or the default role if none was requested in the connection ('xxxxx'), is not listed in the Access Token or was filtered. 
   Please specify another role, or contact your OAuth Authorization server administrator.
   ```

2. Edit your OAuth Security integration and explicitly specify this scope mapping attribute:

   ```sql
   ALTER INTEGRATION <my_int_name> SET EXTERNAL_OAUTH_SCOPE_MAPPING_ATTRIBUTE = 'scp';
   ```

You can read more about this error in [Snowflake's documentation](https://community.snowflake.com/s/article/external-custom-oauth-error-the-role-requested-in-the-connection-is-not-listed-in-the-access-token).

***

1. If you see the following error:

   ```text
   Failed to connect to DB: xxxxxxx.snowflakecomputing.com:443. Incorrect username or password was specified.
   ```

   * **Unique email addresses** — Each user in Snowflake must have a unique email address. You can't have multiple users (for example, a human user and a service account) using the same email, such as `alice@acme.com`, to authenticate to Snowflake.
   * **Match email addresses with identity provider** — The email address of your Snowflake user must exactly match the email address you use to authenticate with your Identity Provider (IdP). For example, if your Snowflake user's email is `alice@acme.com` but you log in to Entra or Okta with `alice_adm@acme.com`, this mismatch can cause an error.

## Was this page helpful?

YesNo

[Privacy policy](https://www.getdbt.com/cloud/privacy-policy)[Create a GitHub issue](https://github.com/dbt-labs/docs.getdbt.com/issues)

This site is protected by reCAPTCHA and the Google [Privacy Policy](https://policies.google.com/privacy) and [Terms of Service](https://policies.google.com/terms) apply.
