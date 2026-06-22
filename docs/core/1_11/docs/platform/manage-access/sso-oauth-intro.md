# Single sign-on and OAuth [Enterprise](https://www.getdbt.com/pricing "Go to https://www.getdbt.com/pricing")[Enterprise +](https://www.getdbt.com/pricing "Go to https://www.getdbt.com/pricing")

This section covers how to authenticate users and connect data platforms in dbt platform using:

* [Single sign-on (SSO)](#sso)
* [System for Cross-Domain Identity Management (SCIM)](#scim)
* [Connection OAuth](#connection-oauth)

These features are available on Enterprise and Enterprise+ plans and are typically configured by account admins or security teams.

## SSO[​](#sso "Direct link to SSO")

Lets users log in to dbt with your identity provider (IdP) instead of a password. Supports Just-in-Time provisioning and IdP-initiated login. *For admins setting up Okta, Microsoft Entra ID, Google Workspace, or SAML 2.0.*

* [Single sign-on (SSO) overview](https://docs.getdbt.com/docs/platform/manage-access/sso-overview.md) — How SSO works and prerequisites
* [Migrating to Auth0 for SSO](https://docs.getdbt.com/docs/platform/manage-access/auth0-migration.md)
* [Set up SSO with SAML 2.0](https://docs.getdbt.com/docs/platform/manage-access/set-up-sso-saml-2.0.md)
* [Set up SSO with Okta](https://docs.getdbt.com/docs/platform/manage-access/set-up-sso-okta.md)
* [Set up SSO with Google Workspace](https://docs.getdbt.com/docs/platform/manage-access/set-up-sso-google-workspace.md)
* [Set up SSO with Microsoft Entra ID](https://docs.getdbt.com/docs/platform/manage-access/set-up-sso-microsoft-entra-id.md)

## SCIM[​](#scim "Direct link to SCIM")

Automates user and group provisioning from your IdP into dbt (and, with Okta, license assignment). *For admins using Okta or Microsoft Entra ID who want to sync users and groups.*

* [Set up SCIM](https://docs.getdbt.com/docs/platform/manage-access/scim.md) — Prerequisites and enabling SCIM in dbt
* [Set up SCIM with Okta](https://docs.getdbt.com/docs/platform/manage-access/scim-okta.md) (includes [license management](https://docs.getdbt.com/docs/platform/manage-access/scim-manage-user-licenses.md))
* [Set up SCIM with Entra ID](https://docs.getdbt.com/docs/platform/manage-access/scim-entra-id.md)

## Connection OAuth[​](#connection-oauth "Direct link to Connection OAuth")

Connection OAuth is for authenticating to your data platform (like Snowflake, BigQuery), which is different from SSO, which handles user login to dbt platform. It lets developers authorize their development credentials with a data platform using that platform's login instead of storing passwords in dbt. *For admins and developers connecting to supported data platforms.*

* [OAuth overview](https://docs.getdbt.com/docs/platform/manage-access/oauth-intro.md) — What's available by platform
* [Set up Snowflake OAuth](https://docs.getdbt.com/docs/platform/manage-access/set-up-snowflake-oauth.md)
* [Set up Databricks OAuth](https://docs.getdbt.com/docs/platform/manage-access/set-up-databricks-oauth.md)
* [Set up BigQuery OAuth](https://docs.getdbt.com/docs/platform/manage-access/set-up-bigquery-oauth.md)
* [Set up external OAuth with Snowflake](https://docs.getdbt.com/docs/platform/manage-access/snowflake-external-oauth.md)
* [Set up external OAuth with Redshift](https://docs.getdbt.com/docs/platform/manage-access/redshift-external-oauth.md)
