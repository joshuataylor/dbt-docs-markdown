# Single sign-on and OAuth

dbt platform | Enterprise, Enterprise+ⓘ

This section covers how to authenticate users and connect data platforms in dbt platform using:

* [Single sign-on (SSO)](#sso)
* [System for Cross-Domain Identity Management (SCIM)](#scim)
* [Connection OAuth](#connection-oauth)

These features are available on Enterprise and Enterprise+ plans and are typically configured by account admins or security teams.

## SSO[​](#sso "Direct link to SSO")

Lets users log in to dbt with your identity provider (IdP) instead of a password. Supports Just-in-Time provisioning and IdP-initiated login. *For admins setting up Okta, Microsoft Entra ID, Google Workspace, or SAML 2.0.*

* [Single sign-on (SSO) overview](./sso-overview.md) — How SSO works and prerequisites
* [Migrating to Auth0 for SSO](./auth0-migration.md)
* [Set up SSO with SAML 2.0](./set-up-sso-saml-2.0.md)
* [Set up SSO with Okta](./set-up-sso-okta.md)
* [Set up SSO with Google Workspace](./set-up-sso-google-workspace.md)
* [Set up SSO with Microsoft Entra ID](./set-up-sso-microsoft-entra-id.md)

## SCIM[​](#scim "Direct link to SCIM")

Automates user and group provisioning from your IdP into dbt (and, with Okta, license assignment). *For admins using Okta or Microsoft Entra ID who want to sync users and groups.*

* [Set up SCIM](./scim.md) — Prerequisites and enabling SCIM in dbt
* [Set up SCIM with Okta](./scim-okta.md) (includes [license management](./scim-manage-user-licenses.md))
* [Set up SCIM with Entra ID](./scim-entra-id.md)

## Connection OAuth[​](#connection-oauth "Direct link to Connection OAuth")

Connection OAuth is for authenticating to your data platform (like Snowflake, BigQuery), which is different from SSO, which handles user login to dbt platform. It lets developers authorize their user credentials with a data platform using that platform's login instead of storing passwords in dbt. *For admins and developers connecting to supported data platforms.*

* [OAuth overview](./oauth-intro.md) — What's available by platform
* [Set up Snowflake OAuth](./set-up-snowflake-oauth.md)
* [Set up Databricks OAuth](./set-up-databricks-oauth.md)
* [Set up BigQuery OAuth](./set-up-bigquery-oauth.md)
* [Set up external OAuth with Snowflake](./snowflake-external-oauth.md)
* [Set up external OAuth with Redshift](./redshift-external-oauth.md)
