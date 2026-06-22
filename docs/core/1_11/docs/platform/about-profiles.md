# About dbt platform profiles

dbt platform profiles define the connections, credentials, and attributes you use to connect to a data warehouse.

Assign profiles to [deployment environments](https://docs.getdbt.com/docs/dbt-platform-environments.md#deployment-environment) and reuse those profiles in other deployment environments within the same project. You can manage profiles programmatically using our [API documentation](https://docs.getdbt.com/dbt-cloud/api-v3#/operations/List%20Profiles).

## Environment profiles table[​](#environment-profiles-table "Direct link to Environment profiles table")

On an environment's **Settings** page, the **Connection profiles** section lists the profiles assigned to that environment:

* **Profile name**: Click a profile name to open the view/edit drawer. In view mode, there’s no separate action column, so use the profile name to open and view a profile.
* **Connection**: Click the connection to open the [connection details](https://docs.getdbt.com/docs/platform/connect-data-platform/about-connections.md#connection-management) page in a new tab.
* **Edit mode**: Click **Edit** to switch to edit mode. Use the **swap icon** ![Swap icon](/img/docs/deploy/swap-icon.png) next to a profile row to assign a different profile.

### Considerations[​](#considerations "Direct link to Considerations")

* Profiles don't apply to development environments because of the unique configurations and individual credentials applied.
* The Semantic Layer configuration isn't supported with profiles yet.

## Create a profile[​](#create-a-profile "Direct link to Create a profile")

new feature rollout

dbt automatically creates a new project-level profile for each deployment environment and populates it with your existing connection, credentials, and extended attributes. You don't need to take any action to create profiles for your existing projects.

You can create profiles from either the project or the environment settings. No matter which approach you take, dbt creates the profile at the project level. Profiles you create in one project won't be visible in others.

To create a new profile:

* From project settings
* From environment settings

1. From the main menu, navigate to your project's **Dashboard**.
2. Click **Settings**.
3. Scroll down to the **Profiles** section and click **Create new profile**.

[![Creating a profile from project settings.](/img/docs/dbt-platform/profile-from-project.png?v=2 "Creating a profile from project settings.")](#)Creating a profile from project settings.

1. From the main menu, click **Orchestration** and select **Environments**.
2. Click an available deployment environment.
3. Click **Settings**.
4. Click **Edit** to switch to edit mode, then scroll to the **Connection profiles** section.
5. Click the **swap icon** ![Swap icon](/img/docs/deploy/swap-icon.png) next to the profile row you want to change.
6. Select **Add new profile** from the **Profile** dropdown.
7. Click **Create profile**.
8. Click **Save**.

[![Creating a profile from the environment settings.](/img/docs/dbt-platform/profile-from-environment.png?v=2 "Creating a profile from the environment settings.")](#)Creating a profile from the environment settings.

The following steps are the same regardless of which approach you take:

1. Give the profile a name that's unique across all projects in the account, easy to identify, and adheres to the naming policy:

   * Starts with a letter
   * Ends with a letter or number
   * Contains only letters, numbers, dashes, or underscores
   * Has no consecutive dashes or underscores

2. From **Connection details**, select a connection from the list of available [global connections](https://docs.getdbt.com/docs/platform/connect-data-platform/about-connections.md#connection-management) or add a new connection.

3. Configure the **Deployment credentials** for your warehouse connection.

4. Add any [**Extended attributes**](https://docs.getdbt.com/docs/dbt-platform-environments.md#extended-attributes) you need. If you use [`env_var()`](https://docs.getdbt.com/reference/dbt-jinja-functions/env_var.md) in Extended Attributes, the referenced environment variables must be *project-scoped* in order to work with connection tests. Since profiles are environment-agnostic, environment-scoped variables are not available during connection tests.

   To set a project-scoped variable, go to **Orchestration** > **Environments** > **Environment variables**, and enter a value in the **Project default** column. Learn more in [environment variables](https://docs.getdbt.com/docs/build/environment-variables.md?version=2.0#setting-environment-variables).

5. Click **Save** at the top of the screen.

[![Sample of a configured profile.](/img/docs/dbt-platform/profile-sample.png?v=2 "Sample of a configured profile.")](#)Sample of a configured profile.

Repeat these steps until you've created all the profiles you need for your project's deployment environments.

## Assign a profile[​](#assign-a-profile "Direct link to Assign a profile")

You configure profiles when you create a deployment environment. For accounts that already have environments configured when you enable profiles, dbt automatically creates and assigns a default profile to all projects.

To assign a different profile, update the deployment environment settings:

1. From the left navigation, click **Orchestration** and select **Environments**.
2. Click an available deployment environment.
3. Click **Settings**.
4. Click **Edit** to switch to edit mode, then scroll to the **Connection profiles** section.
5. Click the **swap icon** ![Swap icon](/img/docs/deploy/swap-icon.png) next to the profile row you want to change.
6. Select the new profile from the **Profile** dropdown.
7. Click **Assign profile**.
8. Click **Save**.

## Permissions and access to profiles[​](#permissions-and-access-to-profiles "Direct link to Permissions and access to profiles")

Profiles are created at the project level. Only users with permission to edit the project can create profiles and anyone with permission to create or edit deployment environments in that project can assign that profile and its credentials to those environments.

To avoid unintended access, only grant permission sets like **Job Admin** or **Project Admin** to users who should have access to all credentials in a project. Be mindful that profiles created at the project level can be used to configure credentials for any deployment environment in that project.

For more information on permission sets, see [Enterprise permissions](https://docs.getdbt.com/docs/platform/manage-access/enterprise-permissions.md).

## FAQs[​](#faqs "Direct link to FAQs")

 Do I need to create profiles for all of my existing projects?

You don't need to take any action. dbt automatically creates profiles for all existing projects and deployment environments based on the existing connection, credentials, and extended attributes.

 Are there any changes to development environments?

Not at this time. Profiles only apply to deployment environments.

 What happens if I change my connection details, credentials, or attributes?

Any profiles using those settings automatically update with the new information.

 What if I use APIs to configure project settings?

Existing APIs continue to work and automatically map to a profile behind the scenes. You won't need to take any manual action unless you use APIs to create a deployment environment with no credentials configured. This is a rare occurrence unique to APIs, but it's the only scenario where dbt wouldn't create a profile.

Profile-specific APIs are available. Check out our [API documentation](https://docs.getdbt.com/docs/dbt-apis/overview.md) for more information.

 Does the Semantic Layer support profiles?

Semantic Layer configuration isn't supported with profiles yet.
