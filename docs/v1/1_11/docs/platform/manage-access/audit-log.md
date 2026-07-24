# dbt audit log

dbt platform | Enterprise, Enterprise+ⓘ

To review actions performed by people in your account, dbt provides logs of audited user and system events in real time. The audit log appears as events happen and includes details such as who performed the action, what the action was, and when it was performed. You can use these details to troubleshoot access issues, perform security audits, or analyze specific events. You can also [query and export audit log events through the API](#using-the-api).

You must be an **Account admin**, **Account viewer**, or **Security admin** to access the audit log, and this feature is only available on Enterprise plans.

The dbt audit log stores all the events that occurred in your account in real time, including:

* For events within 90 days, the dbt audit log has a selectable date range that lists events triggered.
* For events beyond 90 days, **Account admins**, **Account viewers**, and **Security admins** can [export all events](#exporting-logs) by using **Export All**.

Note that the retention period for events in the audit log is at least 12 months.

## Accessing the audit log[​](#accessing-the-audit-log "Direct link to Accessing the audit log")

To access the audit log, click on your account name in the left-side menu and select **Account settings**. Click **Audit log** in the left sidebar.

## Understanding the audit log[​](#understanding-the-audit-log "Direct link to Understanding the audit log")

On the audit log page, you will see a list of various events and their associated event data. Each of these events shows the following information in dbt:

* **Event name**: Action that was triggered
* **Agent**: User who triggered that action/event
* **Timestamp**: Local timestamp of when the event occurred

### Event details[​](#event-details "Direct link to Event details")

Click the event card to see the details about the activity that triggered the event. This view provides important details, including when it happened and what type of event was triggered. For example, if someone changes the settings for a job, you can use the event details to see which job was changed (type of event: `job_definition.changed`), by whom (person who triggered the event: `actor`), and when (time it was triggered: `created_at`). For types of events and their descriptions, refer to [Events in audit log](#audit-log-events).

The event details provide the key factors of an event:

| Name           | Description                                                                                                                                            |
| -------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------ |
| account\_id    | Account ID of where the event occurred                                                                                                                 |
| actor          | Actor that carried out the event - User or Service                                                                                                     |
| actor\_id      | Unique ID of the actor                                                                                                                                 |
| actor\_ip      | IP address of the actor                                                                                                                                |
| actor\_name    | Identifying name of the actor                                                                                                                          |
| actor\_type    | Whether the action was done by a user or an API request                                                                                                |
| created\_at    | UTC timestamp of when the event occurred                                                                                                               |
| event\_type    | Unique key identifying the event                                                                                                                       |
| event\_context | This key will be different for each event and will match the event\_type. This data will include all the details about the object(s) that was changed. |
| id             | Unique ID of the event                                                                                                                                 |
| service        | Service that carried out the action                                                                                                                    |
| source         | Source of the event - dbt UI or API                                                                                                                    |

Search table...

|                  |   |   |   |   |
| ---------------- | - | - | - | - |
| Loading table... |   |   |   |   |

## Audit log events[​](#audit-log-events "Direct link to Audit log events")

The audit log supports various events for different objects in dbt. You will find events for authentication, environment, jobs, service tokens, groups, user, project, permissions, license, connection, repository, and credentials.

### Authentication[​](#authentication "Direct link to Authentication")

| Event name                 | Event type               | Description                                                                                                             |
| -------------------------- | ------------------------ | ----------------------------------------------------------------------------------------------------------------------- |
| Access Token Issued        | access\_token.issued     | dbt issued an access token after OAuth sign-in (for example, VS Code extension or Model Context Protocol (MCP) server). |
| Auth Provider Changed      | auth\_provider.changed   | Authentication provider settings changed                                                                                |
| Credential Login Succeeded | login.password.succeeded | User successfully logged in with username and password                                                                  |
| Refresh Token Issued       | refresh\_token.issued    | dbt issued a refresh token after OAuth sign-in (for example, VS Code extension or MCP server).                          |
| SSO Login Failed           | login.sso.failed         | User sign-in through SSO failed                                                                                         |
| SSO Login Succeeded        | login.sso.succeeded      | User successfully signed in through SSO                                                                                 |

Search table...

|                  |   |   |   |   |
| ---------------- | - | - | - | - |
| Loading table... |   |   |   |   |

### OAuth clients[​](#oauth-clients "Direct link to OAuth clients")

These events cover the lifecycle of OAuth clients registered in **Account settings** → **Integrations** → **App integrations** (refer to [Connect apps with OAuth](https://docs.getdbt.com/docs/platform/manage-access/connect-apps-oauth.md)). Actions a user performs through an OAuth-connected client (for example, creating a job) are logged under the relevant event (such as `job_definition.added`) with the user as the actor.

| Event name                    | Event type                            | Description                                                                                                                                   |
| ----------------------------- | ------------------------------------- | --------------------------------------------------------------------------------------------------------------------------------------------- |
| Client Authorization Approved | oauth\_client.authorization\_approved | A user approved an OAuth consent request for a client.                                                                                        |
| Client Authorization Denied   | oauth\_client.authorization\_denied   | A user denied an OAuth consent request for a client.                                                                                          |
| Client Authorized             | oauth\_client.authorized              | A user completed the OAuth consent flow and authorized a client to access dbt on their behalf.                                                |
| Client Registered             | oauth\_client.registered              | An OAuth client was registered, either manually by an admin or dynamically through [RFC 7591](https://datatracker.ietf.org/doc/html/rfc7591). |
| Client Removed                | oauth\_client.removed                 | An OAuth client was removed from **Account settings** → **Integrations** → **App integrations**.                                              |

Search table...

|                  |   |   |   |   |
| ---------------- | - | - | - | - |
| Loading table... |   |   |   |   |

### Environment[​](#environment "Direct link to Environment")

| Event name          | Event type          | Description                          |
| ------------------- | ------------------- | ------------------------------------ |
| Environment Added   | environment.added   | New environment successfully created |
| Environment Changed | environment.changed | Environment settings changed         |
| Environment Removed | environment.removed | Environment successfully removed     |

Search table...

|                  |   |   |   |   |
| ---------------- | - | - | - | - |
| Loading table... |   |   |   |   |

### Jobs[​](#jobs "Direct link to Jobs")

| Event name  | Event type              | Description                  |
| ----------- | ----------------------- | ---------------------------- |
| Job Added   | job\_definition.added   | New Job successfully created |
| Job Changed | job\_definition.changed | Job settings changed         |
| Job Removed | job\_definition.removed | Job definition removed       |

Search table...

|                  |   |   |   |   |
| ---------------- | - | - | - | - |
| Loading table... |   |   |   |   |

### Service Token[​](#service-token "Direct link to Service Token")

| Event name            | Event type             | Description                                |
| --------------------- | ---------------------- | ------------------------------------------ |
| Service Token Created | service\_token.created | New Service Token was successfully created |
| Service Token Revoked | service\_token.revoked | Service Token was revoked                  |

Search table...

|                  |   |   |   |   |
| ---------------- | - | - | - | - |
| Loading table... |   |   |   |   |

### Group[​](#group "Direct link to Group")

| Event name    | Event type    | Description                    |
| ------------- | ------------- | ------------------------------ |
| Group Added   | group.added   | New Group successfully created |
| Group Changed | group.changed | Group settings changed         |
| Group Removed | group.removed | Group successfully removed     |

Search table...

|                  |   |   |   |   |
| ---------------- | - | - | - | - |
| Loading table... |   |   |   |   |

### User[​](#user "Direct link to User")

| Event name                   | Event type               | Description                                         |
| ---------------------------- | ------------------------ | --------------------------------------------------- |
| Invite Added                 | user.invite.added        | User invitation added and sent to the user          |
| Invite Redeemed              | user.invite.redeemed     | User redeemed invitation                            |
| User Added to Account        | user.added               | New user added to the account                       |
| User Added to Group          | group.user.added         | An existing user was added to a group               |
| User Removed from Account    | user.removed             | User removed from the account                       |
| User Removed from Group      | group.user.removed       | An existing user was removed from a group           |
| User License Created         | user\_license.added      | A new user license was consumed                     |
| User License Removed         | user\_license.removed    | A user license was removed from the seat count      |
| Verification Email Confirmed | user.jit.email.confirmed | Email verification confirmed by user                |
| Verification Email Sent      | user.jit.email.sent      | Email verification sent to user created through JIT |

Search table...

|                  |   |   |   |   |
| ---------------- | - | - | - | - |
| Loading table... |   |   |   |   |

### Project[​](#project "Direct link to Project")

| Event name      | Event type      | Description              |
| --------------- | --------------- | ------------------------ |
| Project Added   | project.added   | New project added        |
| Project Changed | project.changed | Project settings changed |
| Project Removed | project.removed | Project is removed       |

Search table...

|                  |   |   |   |   |
| ---------------- | - | - | - | - |
| Loading table... |   |   |   |   |

### Permissions[​](#permissions "Direct link to Permissions")

| Event name              | Event type         | Description                    |
| ----------------------- | ------------------ | ------------------------------ |
| User Permission Added   | permission.added   | New user permissions are added |
| User Permission Removed | permission.removed | User permissions are removed   |

Search table...

|                  |   |   |   |   |
| ---------------- | - | - | - | - |
| Loading table... |   |   |   |   |

### License[​](#license "Direct link to License")

| Event name              | Event type           | Description                               |
| ----------------------- | -------------------- | ----------------------------------------- |
| License Mapping Added   | license\_map.added   | New user license mapping is added         |
| License Mapping Changed | license\_map.changed | User license mapping settings are changed |
| License Mapping Removed | license\_map.removed | User license mapping is removed           |

Search table...

|                  |   |   |   |   |
| ---------------- | - | - | - | - |
| Loading table... |   |   |   |   |

### Connection[​](#connection "Direct link to Connection")

| Event name         | Event type         | Description                                |
| ------------------ | ------------------ | ------------------------------------------ |
| Connection Added   | connection.added   | New Data Warehouse connection added        |
| Connection Changed | connection.changed | Data Warehouse Connection settings changed |
| Connection Removed | connection.removed | Data Warehouse connection removed          |

Search table...

|                  |   |   |   |   |
| ---------------- | - | - | - | - |
| Loading table... |   |   |   |   |

### Repository[​](#repository "Direct link to Repository")

| Event name         | Event type         | Description                 |
| ------------------ | ------------------ | --------------------------- |
| Repository Added   | repository.added   | New repository added        |
| Repository Changed | repository.changed | Repository settings changed |
| Repository Removed | repository.removed | Repository removed          |

Search table...

|                  |   |   |   |   |
| ---------------- | - | - | - | - |
| Loading table... |   |   |   |   |

### Credentials[​](#credentials "Direct link to Credentials")

| Event name                       | Event type          | Description                      |
| -------------------------------- | ------------------- | -------------------------------- |
| Credentials Added to Project     | credentials.added   | Project credentials added        |
| Credentials Changed in Project   | credentials.changed | Credentials changed in project   |
| Credentials Removed from Project | credentials.removed | Credentials removed from project |

Search table...

|                  |   |   |   |   |
| ---------------- | - | - | - | - |
| Loading table... |   |   |   |   |

### Git integration[​](#git-integration "Direct link to Git integration")

| Event name                 | Event type                  | Description                         |
| -------------------------- | --------------------------- | ----------------------------------- |
| GitLab Application Changed | gitlab\_application.changed | GitLab configuration in dbt changed |

Search table...

|                  |   |   |   |   |
| ---------------- | - | - | - | - |
| Loading table... |   |   |   |   |

### Webhooks[​](#webhooks "Direct link to Webhooks")

| Event name                    | Event type                    | Description                            |
| ----------------------------- | ----------------------------- | -------------------------------------- |
| Webhook Subscriptions Added   | webhook\_subscription.added   | New webhook configured in settings     |
| Webhook Subscriptions Changed | webhook\_subscription.changed | Existing webhook configuration altered |
| Webhook Subscriptions Removed | webhook\_subscription.removed | Existing webhook deleted               |

Search table...

|                  |   |   |   |   |
| ---------------- | - | - | - | - |
| Loading table... |   |   |   |   |

### Semantic Layer[​](#semantic-layer "Direct link to Semantic Layer")

| Event name                         | Event type                           | Description                                                                          |
| ---------------------------------- | ------------------------------------ | ------------------------------------------------------------------------------------ |
| Semantic Layer Config Added        | semantic\_layer\_config.added        | Semantic Layer config added                                                          |
| Semantic Layer Config Changed      | semantic\_layer\_config.changed      | Semantic Layer config (not related to credentials) changed                           |
| Semantic Layer Config Removed      | semantic\_layer\_config.removed      | Semantic Layer config removed                                                        |
| Semantic Layer Credentials Added   | semantic\_layer\_credentials.added   | Semantic Layer credentials added                                                     |
| Semantic Layer Credentials Changed | semantic\_layer\_credentials.changed | Semantic Layer credentials changed. Does not trigger `semantic_layer_config.changed` |
| Semantic Layer Credentials Removed | semantic\_layer\_credentials.removed | Semantic Layer credentials removed                                                   |

Search table...

|                  |   |   |   |   |
| ---------------- | - | - | - | - |
| Loading table... |   |   |   |   |

### Extended attributes[​](#extended-attributes "Direct link to Extended attributes")

| Event name                 | Event type                   | Description                           |
| -------------------------- | ---------------------------- | ------------------------------------- |
| Extended Attribute Added   | extended\_attributes.added   | Extended attribute added to a project |
| Extended Attribute Changed | extended\_attributes.changed | Extended attribute changed or removed |

Search table...

|                  |   |   |   |   |
| ---------------- | - | - | - | - |
| Loading table... |   |   |   |   |

### Account-scoped personal access token[​](#account-scoped-personal-access-token "Direct link to Account-scoped personal access token")

| Event name                                   | Event type                   | Description                       |
| -------------------------------------------- | ---------------------------- | --------------------------------- |
| Account Scoped Personal Access Token Created | account\_scoped\_pat.created | An account-scoped PAT was created |
| Account Scoped Personal Access Token Deleted | account\_scoped\_pat.deleted | An account-scoped PAT was deleted |

Search table...

|                  |   |   |   |   |
| ---------------- | - | - | - | - |
| Loading table... |   |   |   |   |

### IP restrictions[​](#ip-restrictions "Direct link to IP restrictions")

| Event name                   | Event type                    | Description                                 |
| ---------------------------- | ----------------------------- | ------------------------------------------- |
| IP Restrictions Toggled      | ip\_restrictions.toggled      | IP restrictions feature enabled or disabled |
| IP Restrictions Rule Added   | ip\_restrictions.rule.added   | IP restriction rule created                 |
| IP Restrictions Rule Changed | ip\_restrictions.rule.changed | IP restriction rule edited                  |
| IP Restrictions Rule Removed | ip\_restrictions.rule.removed | IP restriction rule deleted                 |

Search table...

|                  |   |   |   |   |
| ---------------- | - | - | - | - |
| Loading table... |   |   |   |   |

### SCIM[​](#scim "Direct link to SCIM")

| Event name     | Event type                          | Description                                  |
| -------------- | ----------------------------------- | -------------------------------------------- |
| User Creation  | v1.events.account.UserAdded         | New user created by SCIM service             |
| User Update    | v1.events.account.UserUpdated       | User record updated by SCIM service          |
| User Removal   | v1.events.account.UserRemoved       | User deleted by the SCIM service             |
| Group Creation | v1.events.user\_group.Added         | New group created by SCIM service            |
| Group Update   | v1.events.user\_group\_user.Changed | Group membership was updated by SCIM service |
| Group Removal  | v1.events.user\_group.Removed       | Group removed by SCIM service                |

Search table...

|                  |   |   |   |   |
| ---------------- | - | - | - | - |
| Loading table... |   |   |   |   |

## Searching the audit log[​](#searching-the-audit-log "Direct link to Searching the audit log")

You can search the audit log to find a specific event or actor, which is limited to the ones listed in [Events in audit log](#audit-log-events). The audit log lists historical events from the last 90 days. You can search for an actor or event using the search bar, and then narrow your results using the time window.

[![Use search bar to find content in the audit log](/img/docs/dbt-platform/dbt-platform-enterprise/audit-log-search.png?v=2 "Use search bar to find content in the audit log")](#)Use search bar to find content in the audit log

## Exporting logs[​](#exporting-logs "Direct link to Exporting logs")

You can use the audit log to export all historical audit results for security, compliance, and analysis purposes. Events in the audit log are retained for at least 12 months.

* **For events within 90 days:** dbt will automatically display the 90-day selectable date range. Select **Export Selection** to download a CSV file of all the events that occurred in your account within 90 days.

* **For events beyond 90 days:** Select **Export All**. An **Account admin**, **Account viewer**, or **Security admin** receives an email link to download a CSV file of all the events that occurred in your account.

[![View audit log export options](/img/docs/dbt-platform/dbt-platform-enterprise/audit-log-section.png?v=2 "View audit log export options")](#)View audit log export options

## Using the API[​](#using-the-api "Direct link to Using the API")

You can also query and export audit log events with the [Administrative API v3](https://docs.getdbt.com/dbt-cloud/api-v3). Use the API when you want to pull audit data into a script, a scheduled job, or security monitoring or log aggregation tooling. The same access requirements at the top of this page apply to the API. For common automation approaches, refer to [Common automation patterns](#common-automation-patterns).

| If you want to…                          | Refer to                          |
| ---------------------------------------- | --------------------------------- |
| Pull recent events into a script or SIEM | [Query events](#query-events)     |
| Export a date range as CSV               | [Download a CSV](#download-a-csv) |
| Export full history                      | [Bulk export](#bulk-export)       |

Search table...

|                  |   |   |   |   |
| ---------------- | - | - | - | - |
| Loading table... |   |   |   |   |

Authenticate with a [service token](https://docs.getdbt.com/docs/dbt-apis/service-tokens.md) or [personal access token](https://docs.getdbt.com/docs/dbt-apis/user-tokens.md), passed as `Authorization: Bearer YOUR_TOKEN` or `Token YOUR_TOKEN`.

Bulk exports require a [personal access token](https://docs.getdbt.com/docs/dbt-apis/user-tokens.md) as service tokens aren't supported for that step.

Without date parameters, list and CSV download requests default to the last 90 days.

In the following examples, replace `YOUR_ACCESS_URL` with your dbt access URL, `ACCOUNT_ID` with your account ID, and `YOUR_TOKEN` with your token. You can find your account ID in **Account settings**, or refer to [Finding your user and account IDs](https://docs.getdbt.com/faqs/Accounts/find-user-id.md) for more information.

### Query events[​](#query-events "Direct link to Query events")

To fetch audit log events as JSON, send a `GET` request to `/api/v3/accounts/ACCOUNT_ID/audit-logs/`:

```shell
curl --request GET \
  --url 'https://YOUR_ACCESS_URL/api/v3/accounts/ACCOUNT_ID/audit-logs/?logged_at_start=2024-01-01&logged_at_end=2024-01-31&limit=10&offset=0' \
  --header 'Accept: application/json' \
  --header 'Authorization: Bearer YOUR_TOKEN'
```

The response includes a `data` array of events with details such as `event_type`, `event_label`, `actor`, `created_at`, and `event_context`. Use `logged_at_start` and `logged_at_end` to filter by date range, and `limit` and `offset` to paginate through results. For more options, refer to [List Recent Audit Log Events](https://docs.getdbt.com/dbt-cloud/api-v3#/operations/List%20Recent%20Audit%20Log%20Events) in the API reference.

### Download a CSV[​](#download-a-csv "Direct link to Download a CSV")

To download audit log events as a CSV file, send a `GET` request to `/api/v3/accounts/ACCOUNT_ID/audit-logs/download/`. This is the API equivalent of **Export Selection** in the UI:

```shell
curl --request GET \
  --url 'https://YOUR_ACCESS_URL/api/v3/accounts/ACCOUNT_ID/audit-logs/download/?logged_at_start=2024-01-01&logged_at_end=2024-01-31' \
  --header 'Authorization: Bearer YOUR_TOKEN' \
  --output audit-log.csv
```

You can use the same `logged_at_start` and `logged_at_end` query parameters as the list endpoint.

### Bulk export[​](#bulk-export "Direct link to Bulk export")

To export your full audit log history through the API, use the same workflow as **Export All** in the UI:

1. Submit an export request with `POST` to `/api/v3/accounts/ACCOUNT_ID/audit-logs/export/`. The response returns a `job_id` in the `data` object (`data.job_id`). Use a [personal access token](https://docs.getdbt.com/docs/dbt-apis/user-tokens.md) for this step.

```shell
curl --request POST \
  --url 'https://YOUR_ACCESS_URL/api/v3/accounts/ACCOUNT_ID/audit-logs/export/' \
  --header 'Authorization: Bearer YOUR_TOKEN'
```

2. Check the export status with `GET` on `/api/v3/accounts/ACCOUNT_ID/audit-logs/export/`. Repeat this request until `is_running` in the `data` object is `false`.

```shell
curl --request GET \
  --url 'https://YOUR_ACCESS_URL/api/v3/accounts/ACCOUNT_ID/audit-logs/export/' \
  --header 'Accept: application/json' \
  --header 'Authorization: Bearer YOUR_TOKEN'
```

3. Get a download URL with `GET` on `/api/v3/accounts/ACCOUNT_ID/audit-logs/export/JOB_ID/download/`. The response includes a `download_url` in the `data` object (`data.download_url`). Use the `job_id` from step 1 as `JOB_ID` in the URL.

```shell
curl --request GET \
  --url 'https://YOUR_ACCESS_URL/api/v3/accounts/ACCOUNT_ID/audit-logs/export/JOB_ID/download/' \
  --header 'Accept: application/json' \
  --header 'Authorization: Bearer YOUR_TOKEN'
```

4. Download the CSV file from the `download_url` returned in step 3 (`data.download_url`):

```shell
curl --request GET \
  --url 'DOWNLOAD_URL' \
  --output audit-log-full.csv
```

### Common automation patterns[​](#common-automation-patterns "Direct link to Common automation patterns")

Teams often automate audit log access in a few ways:

* **Scheduled incremental pulls:** Call the list endpoint on a schedule with `logged_at_start` and `logged_at_end` to fetch new events since the last run, then load the JSON into your security monitoring or log aggregation tooling.
* **Periodic CSV exports:** Use the download endpoint to export a date range as CSV for compliance archival or analysis.
* **Full history exports:** Use the bulk export workflow when you need your complete audit log history programmatically, similar to **Export All** in the UI.

For endpoint details, refer to the [Administrative API v3 reference](https://docs.getdbt.com/dbt-cloud/api-v3#/operations/List%20Recent%20Audit%20Log%20Events).
