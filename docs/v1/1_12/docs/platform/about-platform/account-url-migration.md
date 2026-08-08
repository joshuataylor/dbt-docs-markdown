# Migrating to account-specific access URLs

dbt platformⓘ

Account-specific access URLs are being assigned to dbt accounts as part of our ongoing efforts to improve your experience and strengthen security. Each account will receive its own unique dbt platform and [API access URLs](../../dbt-apis/overview.md) on the **dbt.com** domain rather than sharing generic **getdbt.com** region URLs. For example:

* Old: `https://cloud.getdbt.com`
* New: `https://ab123.us1.dbt.com`

## What does this mean for me?[​](#what-does-this-mean-for-me "Direct link to What does this mean for me?")

If your account has been assigned a new access URL, please review the [migration timeline](#migration-timeline) and update your account's external integrations using the [integration checklist](#integration-checklist).

If your account has not been assigned a new access URL, you will receive an email and in-app notification with a scheduled enablement date. When the change takes effect, you'll be automatically redirected to your **dbt.com** account-specific access URL. Account sign-in remains the same. Both the new **dbt.com** and **getdbt.com** region URLs will support integrations according to the dates on the [migration timeline](#migration-timeline).

info

Before assignment, if your organization uses network allowlisting, add the **dbt.com** domain to your allowlists. For single-tenant accounts, there will be no change to IP addresses. For multi-tenant accounts, refer to [Access, Regions, & IP Addresses](./access-regions-ip-addresses.md) for updated IPs. If your account has [IP restrictions](../secure/ip-restrictions.md) enabled, review [Validating IP restrictions](#validating-ip-restrictions) before your assignment date.

## Migration timeline[​](#migration-timeline "Direct link to Migration timeline")

Accounts across all regions and service providers are being assigned new access URLs. **getdbt.com** region URLs will continue to support integrations until the scheduled deprecation date, unless otherwise specified.

| Stage                                           | Status       | Timeline                                       |
| ----------------------------------------------- | ------------ | ---------------------------------------------- |
| Multi-tenant **dbt.com** access URL assignment  | ✅ Completed | January 2026                                   |
| Single tenant **dbt.com** access URL assignment | In Progress  | April - September 2026                         |
| **getdbt.com** region URL deprecation           | Scheduled    | February 3, 2027 (previously November 1, 2026) |

Search table...

|                  |   |   |   |   |
| ---------------- | - | - | - | - |
| Loading table... |   |   |   |   |

## Integration checklist[​](#integration-checklist "Direct link to Integration checklist")

Review the following checklist before the **getdbt.com** region URL deprecation date documented in the [migration timeline](#migration-timeline). Update each integration that uses a dbt access URL. If your integration isn't in this list, please speak with your IT or applicable team to identify whether you need to take action.

All dbt Labs managed integrations will be updated automatically, which consists of the dbt GitHub Application, Slack Application, and outbound Git provider webhooks.

### Identity and access management[​](#identity-and-access-management "Direct link to Identity and access management")

| Integration                                                                                                                     | Action required                  |
| ------------------------------------------------------------------------------------------------------------------------------- | -------------------------------- |
| [Google Workspace SSO](../manage-access/set-up-sso-google-workspace.md#creating-credentials) | Update or add OAuth Client       |
| [SCIM (Okta)](../manage-access/scim.md#set-up-dbt-cloud)                                     | Update the SCIM base URL in Okta |

Search table...

|                  |   |   |   |   |
| ---------------- | - | - | - | - |
| Loading table... |   |   |   |   |

### Git providers[​](#git-providers "Direct link to Git providers")

GitLab and Azure DevOps repositories will continue to use your legacy **getdbt.com** URL for OAuth flows. You can't yet update an existing repository to use your new account access URL. Instead, you can recreate a repository to generate a Redirect URI based on your new account access URL.

| Integration                                                                                                                      | Action required                                          |
| -------------------------------------------------------------------------------------------------------------------------------- | -------------------------------------------------------- |
| [GitLab (dbt Labs app)](../git/connect-gitlab.md#setting-up-a-gitlab-oauth-application)       | Update or add GL Group Application with new Redirect URI |
| [GitLab (bring-your-own app)](../git/connect-gitlab.md#setting-up-a-gitlab-oauth-application) | Update or add GL Group Application with new Redirect URI |
| [Azure DevOps (service principal)](../git/setup-service-principal.md)                         | Update or add App Registration                           |
| GitHub On-premises                                                                                                               | Contact [dbt Labs Support](mailto:support@getdbt.com)    |

Search table...

|                  |   |   |   |   |
| ---------------- | - | - | - | - |
| Loading table... |   |   |   |   |

### Data platform connections[​](#data-platform-connections "Direct link to Data platform connections")

Data platform connections will continue to use your legacy **getdbt.com** URL for OAuth flows. You can't yet update an existing connection to use your new account access URL. Instead, you can recreate a connection to generate a Redirect URI based on your new account access URL.

| Integration                                                                                                                                 | Action required                                                     |
| ------------------------------------------------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------- |
| [Snowflake OAuth](../manage-access/set-up-snowflake-oauth.md#subdomain-migration)                        | Update or add Security Integration; update dbt connection           |
| [Snowflake External OAuth](../manage-access/snowflake-external-oauth.md#identity-provider-configuration) | Update Redirect URI in your IdP application                         |
| [Databricks OAuth](../manage-access/set-up-databricks-oauth.md)                                          | Update Redirect URLs or add a new Connection; update dbt connection |
| [BigQuery OAuth](../manage-access/set-up-bigquery-oauth.md)                                              | Update Redirect URI or add a new Connection; update dbt connection  |
| [Redshift External OAuth](../manage-access/redshift-external-oauth.md)                                   | Update Redirect URI in your IdP application                         |

Search table...

|                  |   |   |   |   |
| ---------------- | - | - | - | - |
| Loading table... |   |   |   |   |

### Network and API[​](#network-and-api "Direct link to Network and API")

| Integration                                                                                 | Action required                                    |
| ------------------------------------------------------------------------------------------- | -------------------------------------------------- |
| Network allowlists                                                                          | Add new access URLs to your allowlist policies     |
| Inbound webhooks                                                                            | Update access URLs in your webhook configurations  |
| [API integrations](../../dbt-apis/overview.md)                       | Update access URLs in your API clients             |
| [Terraform provider](https://registry.terraform.io/providers/dbt-labs/dbtcloud/latest/docs) | Update access URLs in your Terraform configuration |
| Browser Bookmarks                                                                           | Update personal and shared bookmarks               |

Search table...

|                  |   |   |   |   |
| ---------------- | - | - | - | - |
| Loading table... |   |   |   |   |

## Validating IP restrictions[​](#validating-ip-restrictions "Direct link to Validating IP restrictions")

If you've received notification of a new access URL assignment and have IP restrictions enabled, review the network rules that point specifically at your **\*.getdbt.com** domain. These include VPN split-tunneling rules, proxy (PAC) rules, and firewall egress rules. Update these rules for your new **\*.dbt.com** access URL before your account's scheduled assignment date so access isn't disrupted.

To confirm your new access URL is accessible, send a test request from the same network you'd normally use to reach your **\*.getdbt.com** URL. Replace `NEW_ACCESS_URL` with your account's new access URL and `ACCOUNT_ID` with your account ID:

* **Browser:** Go to `https://NEW_ACCESS_URL/api/v2/accounts/ACCOUNT_ID/`
* **Terminal:** Run `curl -s https://NEW_ACCESS_URL/api/v2/accounts/ACCOUNT_ID/`

| Result     | Response detail                                                         | What it means                                                                                                              |
| ---------- | ----------------------------------------------------------------------- | -------------------------------------------------------------------------------------------------------------------------- |
| ✅ Passed  | `{ ... "detail": "Authentication credentials were not provided." ... }` | The request reached the API and passed IP restrictions. There's no authenticated session, which is expected for this test. |
| ⚠️ Blocked | `{ ... "user_message": "Forbidden: Access denied" ... }`                | The request's IP address isn't on your allowlist. Update your network egress rules for the new access URL and test again.  |

Search table...

|                  |   |   |   |   |
| ---------------- | - | - | - | - |
| Loading table... |   |   |   |   |

## FAQs[​](#faqs "Direct link to FAQs")

 Why are access URLs changing?

We are transitioning from generic region URLs to account-specific URLs to enhance your experience and strengthen security. This change provides more descriptive URLs to improve dbt URL management, and enables stronger cookie and token policies to protect account data.

 How do I know if my account has new access URLs?

Sign in to your dbt platform account. If your browser location has `dbt.com`, your account has been assigned a new access URL. If your browser location has `getdbt.com`, your account has not been assigned a new access URL. You may refer to [API Access URLs](./access-regions-ip-addresses.md#api-access-urls).

 How do I find my account's new access URLs?

Refer to [API Access URLs](./access-regions-ip-addresses.md#api-access-urls).

 What happens if my integrations are not updated by the getdbt.com deprecation date?

You may not be able to access your account through your Identity Provider, and dbt platform may be degraded or inoperable. For assistance, contact [dbt Labs Support](mailto:support@getdbt.com).

 What is unaffected by the migration to new access URLs?

The following are unaffected:

* Your dbt project code, models, and configurations
* Your data platform connections and credentials
* Your account settings, environments, jobs, and schedules
* dbt Labs managed integrations
* Egress Private Connectivity
* The underlying dbt platform functionality

For questions or assistance, contact [dbt Labs Support](mailto:support@getdbt.com).
