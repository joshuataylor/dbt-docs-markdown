# Set up SCIM with Okta [Enterprise](https://www.getdbt.com/pricing "Go to https://www.getdbt.com/pricing")[Enterprise +](https://www.getdbt.com/pricing "Go to https://www.getdbt.com/pricing")

SCIM available for Okta

System for Cross-Domain Identity Management (SCIM) [license mapping](https://docs.getdbt.com/docs/platform/manage-access/scim-manage-user-licenses.md) is currently only supported for Okta. For other providers, license types must be [managed](https://docs.getdbt.com/docs/platform/manage-access/seats-and-users.md#mapped-configuration) within the dbt platform user interface.

## Prerequisites[​](#prerequisites "Direct link to Prerequisites")

* Available on [Enterprise or Enterprise+ plans](https://www.getdbt.com/pricing).
* You must use Okta as your single sign-on (SSO) provider and have it connected in the dbt platform.
* You must have [permissions](https://docs.getdbt.com/docs/platform/manage-access/enterprise-permissions.md) to configure the account settings in dbt platform.
* Complete [setup SSO with Okta](https://docs.getdbt.com/docs/platform/manage-access/set-up-sso-okta.md) before configuring SCIM settings.
* Complete the [Set up SCIM](https://docs.getdbt.com/docs/platform/manage-access/scim.md#set-up-dbt) to get your SCIM base URL and token.

## Set up Okta[​](#set-up-okta "Direct link to Set up Okta")

1. Log in to your Okta account and select **Applications** from the sidebar. Open the application you configured for SSO.

2. Click on the app and navigate to the **General** tab.

3. In **App Settings**, click **Edit** and make sure you check the **SCIM** checkbox on the **Provisioning** row to enable SCIM provisioning.

4. Click **Save** and the **Provisioning** tab will now be visible.

   [![Enable SCIM provisioning in Okta.](/img/docs/dbt-platform/access-control/scim-provisioned.png?v=2 "Enable SCIM provisioning in Okta.")](#)Enable SCIM provisioning in Okta.

5. Open the **Provisioning** tab and select **Integration**.

6. Click **Edit** and enter the **SCIM base URL** from [Set up SCIM](https://docs.getdbt.com/docs/platform/manage-access/scim.md#set-up-dbt) in the first field. For **Unique identifier field for users**, we recommend `userName`.

7. Under **Supported provisioning actions**, select:

   * **Push New Users**
   * **Push Profile Updates**
   * **Push Groups**
   * **Import New Users and Profile Updates** (Optional for users created before SSO/SCIM setup)
   * **Import Groups** (Optional)

8. For **Authentication mode** dropdown, select **HTTP Header**.

9. In the **Authorization** section, enter the token from dbt into the **Bearer** field.

   [![The completed SCIM configuration in the Okta app.](/img/docs/dbt-platform/access-control/scim-okta-config.png?v=2 "The completed SCIM configuration in the Okta app.")](#)The completed SCIM configuration in the Okta app.

10. Click **Save** to test the connection and you will be taken to the **Provisioning** tab.

11. Click **Edit** and ensure the following provisioning actions are selected:

    * **Create Users**
    * **Update User Attributes**
    * **Deactivate Users**

    [![Ensure the users are properly provisioned with these settings.](/img/docs/dbt-platform/access-control/provisioning-actions.png?v=2 "Ensure the users are properly provisioned with these settings.")](#)Ensure the users are properly provisioned with these settings.

12. Click **Save** to complete the provisioning configuration.

13. To complete your group setup, go to **Push Groups** and push your Okta groups to dbt platform. This makes the groups available in dbt platform.

## Assign permission sets to SCIM groups[​](#assign-permission-sets-to-scim-groups "Direct link to Assign permission sets to SCIM groups")

SCIM syncs groups and memberships into dbt platform, but it does not assign [permission sets](https://docs.getdbt.com/docs/platform/manage-access/enterprise-permissions.md). Without a permission set, group members will not have access to features beyond their user profile.

1. After the sync, go to **Account settings** → **Groups & Licenses**, where the SCIM group appears automatically.
2. Open the SCIM-synced group. Don't create another group for the same IdP group, as this creates a duplicate.
3. Under **Access & permissions**, click **Add permission**.
4. Select a [permission set](https://docs.getdbt.com/docs/platform/manage-access/enterprise-permissions.md), the projects it should apply to, and [environment-level access](https://docs.getdbt.com/docs/platform/manage-access/environment-permissions.md) if applicable.
5. Click **Save**.

Repeat for each SCIM-synced group that needs access.

You've now configured SCIM for the Okta SSO integration in dbt platform. You can [manage user licenses with SCIM](https://docs.getdbt.com/docs/platform/manage-access/scim-manage-user-licenses.md) to set license type for users as they are provisioned.

## SCIM username format[​](#scim-username-format "Direct link to SCIM username format")

For dbt platform SCIM with Okta, `userName` **must be the email address format**. dbt platform uses `userName` to look up existing users during SCIM sync. If Okta sends another format (such as an Okta internal ID like `00u...` or an employee ID), dbt platform cannot match the existing user, and provisioning will fail.

If your Okta configurations map the `Username` field to a different attribute, set your Okta app config to `Email`:

1. Open the SAML app created for the dbt integration.
2. In the **Sign on** tab, click **Edit** in the **Settings** pane.
3. Set the **Application username format** field to **Email**.
4. Click **Save**.

SSO and SCIM username

When you use both SSO and SCIM with Okta, the SAML **Application username** format must be **Email**. SCIM requires `userName` in email address format, and it must be the same value as the email attribute so users match between SSO and provisioning.

## SCIM license mapping[​](#scim-license-mapping "Direct link to SCIM license mapping")

To automate seat assignments in Okta for users as they are provisioned, see [Manage user licenses with SCIM](https://docs.getdbt.com/docs/platform/manage-access/scim-manage-user-licenses.md).

## Existing Okta integrations[​](#existing-okta-integrations "Direct link to Existing Okta integrations")

If you are adding SCIM to an existing Okta integration in dbt platform (as opposed to setting up SCIM and SSO concurrently for the first time), be aware of the following behavior. Refer to [SCIM FAQ](https://docs.getdbt.com/docs/platform/manage-access/scim-faq.md) for more details on what happens to pre-existing users:

* Users and groups already synced to dbt will become SCIM-managed once you complete the SCIM configuration.

* (Recommended) Import and manage existing dbt groups and users with Okta's **Import Groups** and **Import Users** features. Update the groups in your IdP with the same naming convention used for dbt groups. New users, groups, and changes to existing profiles will be automatically imported into dbt.

  <!-- -->

  * Ensure the **Import users and profile updates** and **Import Groups** boxes are selected under the **Provisioning settings** tab in the Okta SCIM configuration.
  * Use **Import Users** to sync all users from dbt, including previously deleted users, if you need to re-provision those users.
  * Read more about this feature in the [Okta documentation](https://help.okta.com/en-us/content/topics/users-groups-profiles/usgp-import-groups-app-provisioning.htm).

To set license type for users as they are provisioned, see [Manage user licenses with SCIM](https://docs.getdbt.com/docs/platform/manage-access/scim-manage-user-licenses.md).

## FAQ and troubleshooting[​](#faq-and-troubleshooting "Direct link to FAQ and troubleshooting")

For common questions about SCIM provisioning with Okta — including onboarding workflows, SSO group mapping behavior, and troubleshooting, refer to [SCIM FAQs and troubleshooting](https://docs.getdbt.com/docs/platform/manage-access/scim-faq.md).
