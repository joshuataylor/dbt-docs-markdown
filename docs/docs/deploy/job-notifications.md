# Job notifications

Set up notifications in dbt platform to receive alerts about the status of a job run. You can choose to be notified by one or more of the following job run statuses:

* **Succeeds** option — A job run completed successfully with no warnings or errors.
* **Warns** option — A job run encountered warnings from [data tests](https://docs.getdbt.com/docs/build/data-tests.md) or [source freshness](https://docs.getdbt.com/docs/deploy/source-freshness.md) checks (if applicable).
* **Fails** option — A job run failed to complete.
* **Is canceled** option — A job run is canceled.

### Notification options[​](#notification-options "Direct link to Notification options")

dbt platform currently supports the following notification channels:

* [Email](#email-notifications) — Available for all users
* [Slack (user-linked)](#slack-notifications) — Available for all users
* [Slack (account-level)](#slack-notifications-account) — Available in private beta. To request access, contact your account manager.
* [Microsoft Teams](#microsoft-teams-notifications) — Available in private beta. To request access, contact your account manager.

Microsoft Teams without the private beta

If you don’t have access to the native Microsoft Teams integration (private beta), you can still send job notifications to a Teams channel by using the channel’s email address as an external email, as explained in the next section, Email notifications.

## Email notifications[​](#email-notifications "Direct link to Email notifications")

You can receive email alerts about jobs by configuring the dbt email notification settings.

### Prerequisites[​](#prerequisites "Direct link to Prerequisites")

* You must be either a *developer user* or an *account admin* to configure email notifications in dbt. For more details, refer to [Users and licenses](https://docs.getdbt.com/docs/cloud/manage-access/seats-and-users.md).

  <!-- -->

  * As a developer user, you can set up email notifications for yourself.
  * As an account admin, you can set up notifications for yourself and other team members.

### Configure email notifications[​](#configure-email-notifications "Direct link to Configure email notifications")

1. Select your profile icon and then click **Notification settings**.

2. By default, dbt sends notifications to the email address that's in your **User profile** page.

   If you're an account admin, you can choose a different email address to receive notifications:

   1. Under Job notifications, click the **Notification email** dropdown.

   2. Select another address from the list. The list includes **Internal Users** with access to the account and **External Emails** that have been added.

   3. To add an external email address, click the **Notification email** dropdown

   4. Click **Add external email**.

   5. Enter the email address, and click Add user. After adding an external email, it becomes available for selection in the **Notification email** dropdown list. External emails can be addresses that are outside of your dbt account and also for third-party integrations like [channels in Microsoft Teams](https://support.microsoft.com/en-us/office/tip-send-email-to-a-channel-2c17dbae-acdf-4209-a761-b463bdaaa4ca) and [PagerDuty email integration](https://support.pagerduty.com/docs/email-integration-guide).

      <!-- -->

      note

      External emails and their notification settings persist until edited or removed even if you remove the admin who added them from the account.

   [![Example of the Notification email dropdown](/img/docs/deploy/example-notification-external-email.png?v=2 "Example of the Notification email dropdown")](#)Example of the Notification email dropdown

3. Select the **Environment** for the jobs you want to receive notifications about from the dropdown.

4. Click **Edit** to configure the email notification settings. Choose one or more of the run statuses for each job you want to receive notifications about.

5. When you're done with the settings, click **Save**.

   As an account admin, you can add more email recipients by choosing another **Notification email** from the dropdown, **Edit** the job notification settings, and **Save** the changes.

   To set up alerts on jobs from a different environment, select another **Environment** from the dropdown, **Edit** those job notification settings, and **Save** the changes.

   [![Example of the Email notifications page](/img/docs/deploy/example-email-notification-settings-page.png?v=2 "Example of the Email notifications page")](#)Example of the Email notifications page

### Unsubscribe from email notifications[​](#unsubscribe-from-email-notifications "Direct link to Unsubscribe from email notifications")

1. Select your profile icon and click on **Notification settings**.
2. On the **Email notifications** page, click **Unsubscribe from all email notifications**.

### Send job notifications to a Microsoft Teams channel (email)[​](#email-job-notification-teams "Direct link to Send job notifications to a Microsoft Teams channel (email)")

You can send dbt job [notification emails](#configure-email-notifications) directly to a Microsoft Teams channel by using the channel’s email address.

1. In Microsoft Teams, get the email address for the channel you want to send notifications to. See [Send an email to a channel](https://support.microsoft.com/en-us/office/tip-send-email-to-a-channel-2c17dbae-acdf-4209-a761-b463bdaaa4ca).
2. In dbt platform, click on your profile in the left sidebar and then click **Notification settings**.
3. Under **Job notifications**, click the **Notification email** dropdown.
4. To add an external email address, click **Add external email** at the bottom of the dropdown.
5. Enter the Teams channel email address, and click **Add user**.
6. Make sure you select the Teams channel email from the **Notification email** dropdown (it might be selected already).
7. Then choose the environment for the jobs you want to receive notifications from.
8. Click **Edit**, select the job statuses you want. Then click **Save** to save.

## Slack notifications (user)[​](#slack-notifications "Direct link to Slack notifications (user)")

You can receive Slack alerts about jobs by setting up the Slack integration and then configuring the dbt Slack notification settings. dbt integrates with Slack via OAuth to ensure secure authentication.

This is the current Slack integration available for all users and set at the user level, not to be confused with the [Slack notifications at the account level](#slack-notifications-account) feature, which is available only in private beta. To request access, contact your account manager. Only refer to these instructions if you *don't* have access to the private beta features.

note

Virtual Private Cloud (VPC) admins must [contact support](mailto:support@getdbt.com) to complete the Slack integration.

If there has been a change in user roles or Slack permissions where you no longer have access to edit a configured Slack channel, please [contact support](mailto:support@getdbt.com) for assistance.

### Prerequisites[​](#prerequisites-1 "Direct link to Prerequisites")

* You have a Slack workspace that you want to receive job notifications from.
* You must be a Slack Workspace Owner.
* You must be an account admin to configure Slack notifications in dbt. For more details, refer to [Users and licenses](https://docs.getdbt.com/docs/cloud/manage-access/seats-and-users.md).
* The integration only supports *public* channels in the Slack workspace.

### Set up the Slack integration[​](#set-up-the-slack-integration "Direct link to Set up the Slack integration")

1. Select **Account settings** and then select **Integrations** from the left sidebar.
2. Locate the **OAuth** section with the Slack application and click **Link**.
   <!-- -->
   [![Link for the Slack app](/img/docs/dbt-cloud/Link-your-Slack-Profile.png?v=2 "Link for the Slack app")](#)Link for the Slack app

#### Logged in to Slack[​](#logged-in-to-slack "Direct link to Logged in to Slack")

If you're already logged in to Slack, the handshake only requires allowing the app access. If you're a member of multiple workspaces, you can select the appropriate workspace from the dropdown menu in the upper right corner.

[![Allow dbt access to Slack](/img/docs/dbt-cloud/Allow-dbt-to-access-slack.png?v=2 "Allow dbt access to Slack")](#)Allow dbt access to Slack

#### Logged out[​](#logged-out "Direct link to Logged out")

If you're logged out or the Slack app/website is closed, you must authenticate before completing the integration.

1. Complete the field defining the Slack workspace you want to integrate with dbt.
   <!-- -->
   [![Define the workspace](/img/docs/dbt-cloud/define-workspace.png?v=2 "Define the workspace")](#)Define the workspace
2. Sign in with an existing identity or use the email address and password.
3. Once you have authenticated successfully, accept the permissions.
   <!-- -->
   [![Allow dbt access to Slack](/img/docs/dbt-cloud/accept-permissions.png?v=2 "Allow dbt access to Slack")](#)Allow dbt access to Slack

### Configure Slack notifications[​](#configure-slack-notifications "Direct link to Configure Slack notifications")

1. Select your profile icon and then click on **Notification settings**.

2. Select **Slack notifications** in the left sidebar.

3. Select the **Notification channel** you want to receive the job run notifications from the dropdown.

   [![Example of the Notification channel dropdown](/img/docs/deploy/example-notification-slack-channels.png?v=2 "Example of the Notification channel dropdown")](#)Example of the Notification channel dropdown

4. Select the **Environment** for the jobs you want to receive notifications about from the dropdown.

5. Click **Edit** to configure the Slack notification settings. Choose one or more of the run statuses for each job you want to receive notifications about.

6. When you're done with the settings, click **Save**.

   To send alerts to another Slack channel, select another **Notification channel** from the dropdown, **Edit** those job notification settings, and **Save** the changes.

   To set up alerts on jobs from a different environment, select another **Environment** from the dropdown, **Edit** those job notification settings, and **Save** the changes.

   [![Example of the Slack notifications page](/img/docs/deploy/example-slack-notification-settings-page.png?v=2 "Example of the Slack notifications page")](#)Example of the Slack notifications page

### Disable the Slack integration[​](#disable-the-slack-integration "Direct link to Disable the Slack integration")

1. Select **Account settings** and on the **Integrations** page, scroll to the **OAuth** section.
2. Click the **X** icon (on the far right of the Slack integration) and click **Unlink**. Channels that you configured will no longer receive Slack notifications. *This is not an account-wide action.* Channels configured by other account admins will continue to receive Slack notifications if they still have active Slack integrations. To migrate ownership of a Slack channel notification configuration, have another account admin edit their configuration.

## Slack notifications (account) [Private beta](https://docs.getdbt.com/docs/dbt-versions/product-lifecycles "Go to https://docs.getdbt.com/docs/dbt-versions/product-lifecycles")[​](#slack-notifications-account "Direct link to slack-notifications-account")

info

Configuring Slack notifications at the account level is currently available in private beta. To request access, contact your account manager. Only refer to these instructions if you have access to the private beta feature.

Integrate Slack with dbt platform at the account level to receive job notifications in Slack. dbt integrates with Slack via OAuth to ensure secure authentication.

A single dbt platform account can integrate with one Slack workspace.

### Prerequisites[​](#prerequisites-2 "Direct link to Prerequisites")

* You have a Slack workspace that you want to receive job notifications from.
* A dbt platform account admin must link the Slack app at the account level.
* Install the official dbt platform Slack app using the [steps outlined in the next section](#set-up-the-slack-integration-1).
* To install the Slack app to a workspace, your Slack org must permit app installations. In some orgs this requires a Slack admin approval.
* The integration only supports *public* channels in the Slack workspace.

After an account admin links the Slack app for the account, [any licensed user](https://docs.getdbt.com/docs/cloud/manage-access/seats-and-users.md) in the account can configure Slack job notifications so long as they are assigned to the **Account Admin**, **Owner**, or **Member** default [groups](https://docs.getdbt.com/docs/cloud/manage-access/about-user-access.md#groups). IT licenses don't have access to configure Slack job notifications.

### Set up the Slack integration[​](#set-up-the-slack-integration-1 "Direct link to Set up the Slack integration")

The account-level Slack integration uses the official dbt platform Slack app, which is separate from the [user-linked Slack integration](#slack-notifications).

To use the private beta Slack notifications, you must unlink the old Slack app and then connect the new official app:

1. Go to **Account settings** > **Integrations** > **OAuth**.
2. Click the **X** icon next to Slack and select **Unlink**.
3. In the same OAuth section, click **Link** to connect the official Slack app.

Until you do this, the account-level Slack option will not appear.

[![Link for the Slack app](/img/docs/dbt-cloud/Link-your-Slack-Profile.png?v=2 "Link for the Slack app")](#)Link for the Slack app

### Logged in to Slack[​](#logged-in-to-slack-1 "Direct link to Logged in to Slack")

If you're already logged in to Slack, the integration only requires allowing the app access. If you're a member of multiple workspaces, you can select the appropriate workspace from the dropdown menu in the upper right corner.

[![Allow dbt access to Slack](/img/docs/dbt-cloud/Allow-dbt-to-access-slack.png?v=2 "Allow dbt access to Slack")](#)Allow dbt access to Slack

### Logged out[​](#logged-out-1 "Direct link to Logged out")

If you're logged out or the Slack app/website is closed, you must authenticate before completing the integration.

1. Complete the field defining the Slack workspace you want to integrate with dbt.

   [![Define the workspace](/img/docs/dbt-cloud/define-workspace.png?v=2 "Define the workspace")](#)Define the workspace

2. Sign in with an existing identity or use the email address and password.

3. Once you have authenticated successfully, accept the permissions.

   [![Allow dbt access to Slack](/img/docs/dbt-cloud/accept-permissions.png?v=2 "Allow dbt access to Slack")](#)Allow dbt access to Slack

### Configure Slack notifications[​](#configure-slack-notifications-1 "Direct link to Configure Slack notifications")

Configure the Slack channel you want to receive job notifications from.

1. Select your profile icon and then click on **Notification settings**.

2. Select **Slack notifications** in the left sidebar.

3. From the first dropdown, select the **Notification channel** you want to receive the job run notifications.
   <!-- -->
   [![Example of the Notification channel dropdown](/img/docs/deploy/example-notification-slack-channels.png?v=2 "Example of the Notification channel dropdown")](#)Example of the Notification channel dropdown

4. From the second dropdown, select the **Environment** for the jobs you want to receive notifications about.

5. Click **Edit** to configure the Slack notification settings. Choose one or more of the run statuses for each job you want to receive notifications about.

6. When you're done with the settings, click **Save**.

   <!-- -->

   * To send alerts to another Slack channel, select another **Notification channel** from the dropdown, **Edit** those job notification settings, and **Save** the changes.
   * To set up alerts on jobs from a different environment, select another **Environment** from the dropdown, **Edit** those job notification settings, and **Save** the changes.

   [![Example of the Slack notifications page](/img/docs/deploy/example-slack-notification-settings-page.png?v=2 "Example of the Slack notifications page")](#)Example of the Slack notifications page

That's it! Your Slack channel is now set up to receive dbt job notifications at the account level. This integration is now available throughout the account for all licensed users.

### Disable the Slack integration[​](#disable-the-slack-integration-1 "Direct link to Disable the Slack integration")

In this step, you'll disable the Slack integration and remove the account-level Slack credentials. You can always re-enable the integration by following the [Set up the Slack integration](#set-up-the-slack-integration-1) steps.

1. Select **Account settings** and on the **Integrations** page, scroll to the **OAuth** section.

2. Click the **X** icon (on the far right of the Slack integration) and click **Unlink**.

   <!-- -->

   * This removes the account-level Slack credentials. All Slack notifications that rely on the account-level integration will stop sending.
   * If any legacy, user-linked Slack integrations still exist, those notifications may continue until the legacy link is removed. We recommend migrating to the new account-level app and removing legacy links.

## Microsoft Teams notifications [Private beta](https://docs.getdbt.com/docs/dbt-versions/product-lifecycles "Go to https://docs.getdbt.com/docs/dbt-versions/product-lifecycles")[​](#microsoft-teams-notifications- "Direct link to microsoft-teams-notifications-")

info

Configuring Microsoft Teams notifications is currently in private beta. To request access, contact your account manager.

You can receive Microsoft Teams alerts for your dbt jobs by connecting your Teams account to the dbt platform, and configuring your notification preferences.

dbt integrates with Teams through Microsoft Entra to provide secure authentication. Only refer to these instructions if you have access to the private beta feature.

### Prerequisites[​](#prerequisites-3 "Direct link to Prerequisites")

Before you begin:

* You must have a dbt platform account
* You have a Microsoft Teams account that you want to receive job notifications from.
* Make sure you have permission to view the **Account integrations** and **Job notifications** pages in dbt platform.

### Set up Microsoft Teams[​](#set-up-microsoft-teams "Direct link to Set up Microsoft Teams")

To enable Microsoft Teams job notifications, complete the following sections:

1. [Link dbt platform account to Teams](#link-dbt-platform-account-to-teams) — A user-level connection that links an individual dbt platform account (or a dedicated service account) to a Microsoft Teams user profile within your tenant.
2. [Configure Teams notifications](#configure-teams-notifications) — Configures which Teams channels receive job notifications.
3. [Disable the Teams integration](#disable-the-teams-integration) (optional) — Remove or reset the connection between dbt platform and Microsoft Teams.

### Link dbt platform account to Teams[​](#link-dbt-platform-account-to-teams "Direct link to Link dbt platform account to Teams")

info

You can link any Teams user account from your tenant, but we recommend creating a dedicated account just for posting dbt notifications. During the OAuth process, you’ll need to sign in to a Microsoft account to complete the integration.

* If you’re logged into a single Microsoft account, the integration will complete automatically.
* If you’re logged into multiple accounts (or none), you’ll be prompted to select or log in to one.

 Image of the Microsoft account selection popup

[![Example of the Microsoft account popup](/img/docs/deploy/pick-account.png?v=2 "Example of the Microsoft account popup")](#)Example of the Microsoft account popup

To link your dbt platform account to Microsoft Teams:

1. In dbt platform, go to the **Account settings** page by clicking on your account name and selecting **Account settings**.
2. In the left sidebar, select **Integrations**.
3. Scroll to the **OAuth** section.
4. Next to **Teams** and click on the **Link** button.
5. After doing this, you’ll either be prompted to choose your Microsoft account before completing the setup, or return directly to the dbt platform with your Teams profile linked.
6. Your dbt platform account is now linked to Microsoft Teams!

dbt will now add the **dbt-cloud-integration app** to your Microsoft Entra tenant. This app manages authentication requests and permissions securely.

[![Example of the dbt-cloud-integration app overview](/img/docs/deploy/dbt-cloud-integrations.png?v=2 "Example of the dbt-cloud-integration app overview")](#)Example of the dbt-cloud-integration app overview

* The current Entra app permissions are:

  <!-- -->

  * `profile`
  * `openid`
  * `offline_access`
  * `Team.ReadBasic.All`
  * `TeamsActivity.Send`
  * `ChannelMessage.Send`
  * `ChannelMessage.Read.All`
  * `Channel.ReadBasic.All`

### Configure Teams notifications[​](#configure-teams-notifications "Direct link to Configure Teams notifications")

Once you’ve connected dbt platform and Teams, you can configure which Teams channels receive job notifications. The **Teams notifications** menu requires that you have an active integration with Teams on the account.

info

Currently, dbt only sends notifications to Teams channels (standard, shared, or private) that you belong to.

1. In the dbt platform, click your profile icon and select **Notification settings**.
2. Select **Teams notifications** in the left sidebar.
3. From the first dropdown, select the **Notification team** that you want to send notifications to.
4. From the second dropdown, select the **Notification channel** you want to send notifications to.
   <!-- -->
   * dbt platform only sends notifications to Teams channels (standard, shared, or private) that *you* belong to.
5. In the dropdown, choose the environment for the jobs you want to receive notifications about.
6. Click **Edit** on the top right to configure the Teams job notification settings and customize which job statuses trigger job notifications.
7. When finished, click **Save**.

Your Teams channel is now set up to receive dbt job notifications!

[![Example of the configure Teams notification page](/img/docs/deploy/configure-teams-notification.png?v=2 "Example of the configure Teams notification page")](#)Example of the configure Teams notification page

### Disable the Teams integration[​](#disable-the-teams-integration "Direct link to Disable the Teams integration")

Disabling and unlinking the Teams integration in the dbt platform removes it for the entire account. To disable it:

1. In the dbt platform, go to **Account settings**.
2. Click on **Integrations** and scroll down to **OAuth**.
3. On the far right of the **Teams** integration, click the **X** icon.
4. Confirm the unlinking by selecting **Unlink**.

The Teams integration has been disabled. You can always re-enable the integration by following the [Set up Microsoft Teams](#set-up-microsoft-teams) steps.

## Was this page helpful?

YesNo

[Privacy policy](https://www.getdbt.com/cloud/privacy-policy)[Create a GitHub issue](https://github.com/dbt-labs/docs.getdbt.com/issues)

This site is protected by reCAPTCHA and the Google [Privacy Policy](https://policies.google.com/privacy) and [Terms of Service](https://policies.google.com/terms) apply.
