# Set up SCIM with Entra ID [Enterprise](https://www.getdbt.com/pricing "Go to https://www.getdbt.com/pricing")[Enterprise +](https://www.getdbt.com/pricing "Go to https://www.getdbt.com/pricing")

dbt platform supports System for Cross-Domain Identity Management (SCIM) with Microsoft Entra ID for user and group provisioning and profile updates.

License mapping with SCIM

dbt platform supports automatic license assignment with SCIM, with these different identity provider options:

 Toggle options per identity provider

* **Okta:** Enable the **Ignore dbt license mapping** toggle in **Account settings > SSO & SCIM** and follow the [Okta license management doc](https://docs.getdbt.com/docs/platform/manage-access/scim-manage-user-licenses.md).
* **Entra ID:** Use [SSO-based Active Directory group → license mapping](https://docs.getdbt.com/docs/platform/manage-access/seats-and-users.md#mapped-configuration). It works alongside an active Entra ID SCIM setup. Keep the **Ignore dbt license mapping** toggle *disabled* as enabling it removes license mapping for Entra ID users.

For more details, refer to the [Does SCIM support automatic license assignment?](https://docs.getdbt.com/docs/platform/manage-access/scim-faq.md#does-scim-support-automatic-license-assignment) FAQ.

## Prerequisites[​](#prerequisites "Direct link to Prerequisites")

* Available on [Enterprise or Enterprise+ plans](https://www.getdbt.com/pricing).
* You must use Entra ID as your single sign-on (SSO) provider and have it connected in the dbt platform.
* You must have [permissions](https://docs.getdbt.com/docs/platform/manage-access/enterprise-permissions.md) to configure the account settings in dbt platform.
* Complete [setup SSO with Entra ID](https://docs.getdbt.com/docs/platform/manage-access/set-up-sso-microsoft-entra-id.md) before configuring SCIM settings.
* Complete the [Set up SCIM](https://docs.getdbt.com/docs/platform/manage-access/scim.md#set-up-dbt) to get your SCIM base URL and token.

## Set up Entra ID[​](#set-up-entra-id "Direct link to Set up Entra ID")

1. Log in to your Azure account and open the **Entra ID** configurations.

2. From the sidebar, under **Manage**, click **Enterprise Applications**.

3. Click **New Application** and select the option to **Create your own application**.
   <!-- -->
   [![Create your own application.](/img/docs/dbt-platform/access-control/create-your-own.png?v=2 "Create your own application.")](#)Create your own application.

4. Give your app a unique name and ensure the **Integrate any other application you don't find in the gallery (Non-gallery)** field is selected. Ignore any prompts for existing apps. Click **Create**.
   <!-- -->
   [![Give your app a unique name.](/img/docs/dbt-platform/access-control/create-application.png?v=2 "Give your app a unique name.")](#)Give your app a unique name.

5. From the application **Overview** screen, click **Provision User Accounts**.
   <!-- -->
   [![The 'Provision user accounts' option.](/img/docs/dbt-platform/access-control/provision-user-accounts.png?v=2 "The 'Provision user accounts' option.")](#)The 'Provision user accounts' option.

6. From the **Create configuration** section, click **Connect your application**.

7. Fill out the form with the information from your dbt account:

   <!-- -->

   * The **Tenant URL** in Entra ID is your **SCIM base URL** from dbt.
   * The **Secret token** in Entra ID is your **SCIM token** from dbt.

8. Click **Test connection** and click **Create** once complete.
   <!-- -->
   [![Configure the app and test the connection.](/img/docs/dbt-platform/access-control/provisioning-config.png?v=2 "Configure the app and test the connection.")](#)Configure the app and test the connection.

## Attribute mapping[​](#attribute-mapping "Direct link to Attribute mapping")

To map the attributes that will sync with dbt:

1. From the enterprise app **Overview** screen sidebar menu, click **Provisioning**.

   [![The Provisioning option on the sidebar.](/img/docs/dbt-platform/access-control/provisioning.png?v=2 "The Provisioning option on the sidebar.")](#)The Provisioning option on the sidebar.

2. Under **Manage**, click **Provisioning** again.

3. Expand the **Mappings** section and click **Provision Microsoft Entra ID users**.

   [![Provision the Entra ID users.](/img/docs/dbt-platform/access-control/provision-entra-users.png?v=2 "Provision the Entra ID users.")](#)Provision the Entra ID users.

4. Select the box for **Show advanced options** and then click **Edit attribute list for customappsso**.

   [![Click to edit the customappsso attributes.](/img/docs/dbt-platform/access-control/customappsso-attributes.png?v=2 "Click to edit the customappsso attributes.")](#)Click to edit the customappsso attributes.

5. Scroll to the bottom of the **Edit Attribute List** window and find an empty field where you can add a new entry with the following fields:

   * **Name:** `emails[type eq "work"].primary`
   * **Type:** `Boolean`
   * **Required:** True

   [![Add the new field to the entry list.](/img/docs/dbt-platform/access-control/customappsso-entry.png?v=2 "Add the new field to the entry list.")](#)Add the new field to the entry list.

6. Mark all of the fields listed in Step 10 below as `Required`.

   [![Mark the fields as required.](/img/docs/dbt-platform/access-control/mark-as-required.png?v=2 "Mark the fields as required.")](#)Mark the fields as required.

7. Click **Save**.

8. Back on the **Attribute mapping** window, click **Add new mapping** and complete fields with the following:

   * **Mapping type:** `none`
   * **Default value if null (optional):** `True`
   * **Target attribute:** `emails[type eq "work"].primary`
   * **Match objects using this attribute:** `No`
   * **Matching precedence:** *Leave blank*
   * **Apply this mapping:** `Always`

   [![Complete the fields as shown.](/img/docs/dbt-platform/access-control/edit-attribute.png?v=2 "Complete the fields as shown.")](#)Complete the fields as shown.

9. Click **Ok**.

10. Make sure the following mappings are in place and delete any others:

    * **UserName:** `userPrincipalName` or the value you want users to leverage to log in to dbt.
    * **active:** `Switch([IsSoftDeleted], , "False", "True", "True", "False")`
    * **emails\[type eq "work"].value:** `userPrincipalName` is the most common, but this value needs to be the same set for **UserName**.
    * **name.givenName:** `givenName`
    * **name.familyName:** `surname`
    * **externalid:** `mailNickname`
    * **emails\[type eq "work"].primary**

    [![Edit the attributes so they match the list as shown.](/img/docs/dbt-platform/access-control/attribute-list.png?v=2 "Edit the attributes so they match the list as shown.")](#)Edit the attributes so they match the list as shown.

    SSO and SCIM username

    When you use both SSO and SCIM with Entra ID, map SCIM **UserName** to **userPrincipalName** (or the same Entra attribute users use to sign in to dbt platform). Set **emails\[type eq "work"].value** to that same source.

    SCIM needs the values to match so provisioning aligns with SSO; if **UserName** and **emails\[type eq "work"].value** don't match, users may not sync or match correctly.

11. Click **Save**.

You can now begin assigning users to your SCIM app in Entra ID!

## Assign users to SCIM app[​](#assign-users-to-scim-app "Direct link to Assign users to SCIM app")

The following steps go over how to assign users/groups to the SCIM app. Refer to Microsoft's [official instructions for assigning users or groups to an Enterprise App in Entra ID](https://learn.microsoft.com/en-us/azure/databricks/admin/users-groups/scim/aad#step-3-assign-users-and-groups-to-the-application) to learn more. Although the article is written for Databricks, the steps are identical.

1. Navigate to Enterprise applications and select the SCIM app.

2. Go to **Manage** > **Provisioning**.

3. To synchronize Microsoft Entra ID users and groups to dbt, click **Start provisioning**.
   <!-- -->
   [![Start provisioning to synchronize users and groups.](/img/docs/dbt-platform/dbt-platform-enterprise/access-control/scim-entraid-start-provision.png?v=2 "Start provisioning to synchronize users and groups.")](#)Start provisioning to synchronize users and groups.

4. Navigate back to the SCIM app's overview page and go to **Manage** > **Users and groups**.

5. Click **Add user/group** and select the users and groups.
   <!-- -->
   [![Add user/group.](/img/docs/dbt-platform/dbt-platform-enterprise/access-control/scim-entraid-add-users.png?v=2 "Add user/group.")](#)Add user/group.

6. Click the **Assign** button.

7. Wait a few minutes. In the dbt platform, confirm the users and groups exist in your dbt account.

   <!-- -->

   * Users and groups that you add and assign will automatically be provisioned to your dbt account when Microsoft Entra ID schedules the next sync.
   * By enabling provisioning, you immediately trigger the initial Microsoft Entra ID sync. Subsequent syncs are triggered every 20-40 minutes, depending on the number of users and groups in the application. Refer to Microsoft Entra ID's [Provisioning tips](https://learn.microsoft.com/en-us/azure/databricks/admin/users-groups/scim/aad#provisioning-tips) documentation for more information.
   * You can also prompt a manual provisioning outside of the cycle by clicking **Restart provisioning**.

   [![Prompt manual provisioning.](/img/docs/dbt-platform/dbt-platform-enterprise/access-control/scim-entraid-manual.png?v=2 "Prompt manual provisioning.")](#)Prompt manual provisioning.

## Assign permission sets to SCIM groups[​](#assign-permission-sets-to-scim-groups "Direct link to Assign permission sets to SCIM groups")

SCIM syncs groups and memberships into dbt platform, but it does not assign [permission sets](https://docs.getdbt.com/docs/platform/manage-access/enterprise-permissions.md). Without a permission set, group members will not have access to features beyond their user profile.

1. After the sync, go to **Account settings** → **Groups & Licenses**, where the SCIM group appears automatically.
2. Open the SCIM-synced group. Don't create another group for the same IdP group, as this creates a duplicate.
3. Under **Access & permissions**, click **Add permission**.
4. Select a [permission set](https://docs.getdbt.com/docs/platform/manage-access/enterprise-permissions.md), the projects it should apply to, and [environment-level access](https://docs.getdbt.com/docs/platform/manage-access/environment-permissions.md) if applicable.
5. Click **Save**.

Repeat for each SCIM-synced group that needs access.

## FAQ and troubleshooting[​](#faq-and-troubleshooting "Direct link to FAQ and troubleshooting")

For common questions about SCIM provisioning with Entra ID — including onboarding workflows, attribute matching, IP allowlisting issues, and troubleshooting — refer to [SCIM FAQs and troubleshooting](https://docs.getdbt.com/docs/platform/manage-access/scim-faq.md).
