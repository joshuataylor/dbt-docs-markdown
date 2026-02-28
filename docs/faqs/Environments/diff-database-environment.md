# Can I set a different connection at the environment level?

dbt supports [Connections](https://docs.getdbt.com/docs/cloud/connect-data-platform/about-connections.md#connection-management), available to all dbt users. Connections allows different data platform connections per environment, eliminating the need to duplicate projects. Projects can only use multiple connections of the same warehouse type. Connections are reusable across projects and environments.

In dbt Core, you can maintain separate production and development environments through the use of [`targets`](https://docs.getdbt.com/reference/dbt-jinja-functions/target.md) within a [profile](https://docs.getdbt.com/docs/core/connect-data-platform/profiles.yml.md). dbt Core users can define different targets in their profiles.yml, which means you can have targets for different data warehouses for the same profile.

## Was this page helpful?

YesNo

[Privacy policy](https://www.getdbt.com/cloud/privacy-policy)[Create a GitHub issue](https://github.com/dbt-labs/docs.getdbt.com/issues)

This site is protected by reCAPTCHA and the Google [Privacy Policy](https://policies.google.com/privacy) and [Terms of Service](https://policies.google.com/terms) apply.
