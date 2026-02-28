# About dbt platform profiles

dbt platform profiles define the connections, credentials, and attributes you use to connect to a data warehouse. You assign profiles to [deployment environments](https://docs.getdbt.com/docs/dbt-cloud-environments.md#deployment-environment) and can reuse those profiles in other deployment environments within the same project.

Because of the unique configurations and individual credentials applied, profiles don't apply to development environments.

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

[![Creating a profile from project settings.](/img/docs/dbt-cloud/profile-from-project.png?v=2 "Creating a profile from project settings.")](#)Creating a profile from project settings.

1. From the main menu, click **Orchestration** and select **Environments**.
2. Click an available deployment environment.
3. Click **Settings**, then click **Edit**.
4. Navigate to the **Connection profiles** section, click the three-dot menu next to an existing profile, and select **Change profile**.
5. Click the **Profile** dropdown and select **Create new profile**.

[![Creating a profile from the environment settings.](/img/docs/dbt-cloud/profile-from-environment.png?v=2 "Creating a profile from the environment settings.")](#)Creating a profile from the environment settings.

The following steps are the same regardless of which approach you take:

1. Give the profile a name that's unique across all projects in the account, easy to identify, and adheres to the naming policy:

   <!-- -->

   * Starts with a letter
   * Ends with a letter or number
   * Contains only letters, numbers, dashes, or underscores
   * Has no consecutive dashes or underscores

2. From **Connection details**, select a connection from the list of available [global connections](https://docs.getdbt.com/docs/cloud/connect-data-platform/about-connections.md#connection-management) or add a new connection.

3. Configure the **Deployment credentials** for your warehouse connection.

4. Add any [**Extended attributes**](https://docs.getdbt.com/docs/dbt-cloud-environments.md#extended-attributes) you need.

5. Click **Save** at the top of the screen.

[![Sample of a configured profile.](/img/docs/dbt-cloud/profile-sample.png?v=2 "Sample of a configured profile.")](#)Sample of a configured profile.

Repeat these steps until you've created all the profiles you need for your project's deployment environments.

## Assign a profile[​](#assign-a-profile "Direct link to Assign a profile")

You configure profiles when you create a deployment environment. For accounts that already have environments configured when you enable profiles, dbt automatically creates and assigns a default profile to all projects.

To assign a different profile, update the deployment environment settings:

1. From the main menu, click **Orchestration** and select **Environments**.
2. Click an available deployment environment.
3. Click **Settings**, then click **Edit**.
4. Navigate to the **Connection profiles** section, click the three-dot menu next to an existing profile, and select **Change profile**.
5. Click the **Profile** dropdown and select the new profile to assign.

## FAQs[​](#faqs "Direct link to FAQs")

 Do I need to create profiles for all of my existing projects?

You don't need to take any action. dbt automatically creates profiles for all existing projects and deployment environments based on the existing connection, credentials, and extended attributes.

 Are there any changes to development environments?

Not at this time. Profiles only apply to deployment environments.

 What happens if I change my connection details, credentials, or attributes?

Any profiles using those settings automatically update with the new information.

 What if I use APIs to configure project settings?

Existing APIs continue to work and automatically map to a profile behind the scenes. You won't need to take any manual action unless you use APIs to create a deployment environment with no credentials configured. This is a rare occurrence unique to APIs, but it's the only scenario where dbt wouldn't create a profile.

Profile-specific APIs are available. Check out our [API documentation](https://docs.getdbt.com/docs/dbt-cloud-apis/overview.md) for more information.

## Was this page helpful?

YesNo

[Privacy policy](https://www.getdbt.com/cloud/privacy-policy)[Create a GitHub issue](https://github.com/dbt-labs/docs.getdbt.com/issues)

This site is protected by reCAPTCHA and the Google [Privacy Policy](https://policies.google.com/privacy) and [Terms of Service](https://policies.google.com/terms) apply.
