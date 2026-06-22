# Migrating to account-specific access URLs

Account-specific access URLs are being assigned to dbt accounts as part of our ongoing efforts to improve your experience and strengthen security. Each account will receive its own unique dbt platform and [API access URLs](https://docs.getdbt.com/docs/dbt-apis/overview.md) on the **dbt.com** domain rather than sharing generic **getdbt.com** region URLs. For example:

* Old: `https://cloud.getdbt.com`
* New: `https://ab123.us1.dbt.com`

## What does this mean for me?[​](#what-does-this-mean-for-me "Direct link to What does this mean for me?")

If your account has been assigned a new access URL, please review the [migration timeline](#migration-timeline) and update your account's external integrations using the [integration checklist](#integration-checklist).

If your account has not been assigned a new access URL, you will receive an email and in-app notification with a scheduled assignment date. When the change takes effect, you'll be automatically redirected to your **dbt.com** account-specific access URL. Account sign-in remains the same. Both the new **dbt.com** and **getdbt.com** region URLs will support integrations according to the dates on the [migration timeline](#migration-timeline).

info

Before assignment, if your organization uses network allowlisting, add the **dbt.com** domain to your allowlists. For single-tenant accounts, there will be no change to IP addresses. For multi-tenant accounts, refer to [Access, Regions, & IP Addresses](https://docs.getdbt.com/docs/platform/about-platform/access-regions-ip-addresses.md) for updated IPs.

## Migration timeline[​](#migration-timeline "Direct link to Migration timeline")

Accounts across all regions and service providers are being assigned new access URLs. **getdbt.com** region URLs will continue to support integrations until the scheduled deprecation date, unless otherwise specified.

| Stage                                           | Status       | Timeline               |
| ----------------------------------------------- | ------------ | ---------------------- |
| Multi-tenant **dbt.com** access URL assignment  | ✅ Completed | January 2026           |
| Single tenant **dbt.com** access URL assignment | In Progress  | April - September 2026 |
| **getdbt.com** region URL deprecation           | Scheduled    | November 1, 2026       |

Search table...

|                  |   |   |   |   |
| ---------------- | - | - | - | - |
| Loading table... |   |   |   |   |

## Integration checklist[​](#integration-checklist "Direct link to Integration checklist")

Review the following checklist before the **getdbt.com** region URL deprecation date documented in the [migration timeline](#migration-timeline). Update each integration that uses a dbt access URL. If your integration isn't in this list, please speak with your IT or applicable team to identify whether you need to take action.

All dbt Labs managed integrations will be updated automatically, which consists of the dbt GitHub Application, Slack Application, and outbound Git provider webhooks.

| Integration                                                                                                                                 | Action required                                                     |
| ------------------------------------------------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------- |
| [Google Workspace SSO](https://docs.getdbt.com/docs/platform/manage-access/set-up-sso-google-workspace.md#creating-credentials)             | Update or add OAuth Client                                          |
| [Azure ADO OAuth SSO](https://docs.getdbt.com/docs/platform/git/setup-service-principal.md)                                                 | Update or add App Registration                                      |
| [GitLab (dbt Labs app)](https://docs.getdbt.com/docs/platform/git/connect-gitlab.md#setting-up-a-gitlab-oauth-application)                  | Update or add GL Group Application with new Redirect URI            |
| [GitLab (bring-your-own app)](https://docs.getdbt.com/docs/platform/git/connect-gitlab.md#setting-up-a-gitlab-oauth-application)            | Update or add GL Group Application with new Redirect URI            |
| GitHub On-premises                                                                                                                          | Contact [dbt Labs Support](mailto:support@getdbt.com)               |
| [Snowflake OAuth](https://docs.getdbt.com/docs/platform/manage-access/set-up-snowflake-oauth.md#subdomain-migration)                        | Update or add Security Integration; update dbt connection           |
| [Snowflake External OAuth](https://docs.getdbt.com/docs/platform/manage-access/snowflake-external-oauth.md#identity-provider-configuration) | Update Redirect URI in your IdP application                         |
| [Databricks OAuth](https://docs.getdbt.com/docs/platform/manage-access/set-up-databricks-oauth.md)                                          | Update Redirect URLs or add a new Connection; update dbt connection |
| [BigQuery OAuth](https://docs.getdbt.com/docs/platform/manage-access/set-up-bigquery-oauth.md)                                              | Update Redirect URI or add a new Connection; update dbt connection  |
| [Redshift External OAuth](https://docs.getdbt.com/docs/platform/manage-access/redshift-external-oauth.md)                                   | Update Redirect URI in your IdP application                         |
| Network allowlists                                                                                                                          | Add new access URLs to your allowlist policies                      |
| Inbound webhooks                                                                                                                            | Update access URLs in your webhook configurations                   |
| [SCIM (Okta)](https://docs.getdbt.com/docs/platform/manage-access/scim.md#set-up-dbt-cloud)                                                 | Update the SCIM base URL in Okta                                    |
| [API integrations](https://docs.getdbt.com/docs/dbt-apis/overview.md)                                                                       | Update access URLs in your API clients                              |
| [Terraform provider](https://registry.terraform.io/providers/dbt-labs/dbtcloud/latest/docs)                                                 | Update access URLs in your Terraform configuration                  |
| Browser Bookmarks                                                                                                                           | Update personal and shared bookmarks                                |

Search table...

|                  |   |   |   |   |
| ---------------- | - | - | - | - |
| Loading table... |   |   |   |   |

## FAQs[​](#faqs "Direct link to FAQs")

 Why are access URLs changing?

We are transitioning from generic region URLs to account-specific URLs to enhance your experience and strengthen security. This change provides more descriptive URLs to improve dbt URL management, and enables stronger cookie and token policies to protect account data.

 How do I know if my account has new access URLs?

Sign in to your dbt platform account. If your browser location has `dbt.com`, your account has been assigned a new access URL. If your browser location has `getdbt.com`, your account has not been assigned a new access URL. You may refer to [API Access URLs](https://docs.getdbt.com/docs/platform/about-platform/access-regions-ip-addresses.md#api-access-urls).

 How do I find my account's new access URLs?

Refer to [API Access URLs](https://docs.getdbt.com/docs/platform/about-platform/access-regions-ip-addresses.md#api-access-urls).

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

## Was this page helpful?

YesNo

[Privacy policy](https://www.getdbt.com/cloud/privacy-policy)[Create a GitHub issue](https://github.com/dbt-labs/docs.getdbt.com/issues)

This site is protected by reCAPTCHA and the Google [Privacy Policy](https://policies.google.com/privacy) and [Terms of Service](https://policies.google.com/terms) apply.
