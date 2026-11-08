# About continuous integration (CI) in dbt

dbt platform

Use [CI jobs](./ci-jobs.md) in dbt to set up automation for testing code changes before merging to production. Additionally, [enable Advanced CI features](../platform/account-settings.md#account-access-to-advanced-ci-features) for these jobs to evaluate whether the code changes are producing the appropriate data changes you want by reviewing the comparison differences dbt provides.

Refer to the guide [Get started with continuous integration tests](../../guides/set-up-ci.md?step=1) for more information.

[![](/img/icons/dbt-bit.svg)](./continuous-integration.md)

#### [Continuous integration](./continuous-integration.md)

[Set up CI checks to test every single change prior to deploying the code to production.](./continuous-integration.md)

[![](/img/icons/dbt-bit.svg)](./advanced-ci.md)

#### [Advanced CI](./advanced-ci.md)

[Compare the differences between what's in the production environment and the pull request before merging those changes, ensuring that you're always shipping trusted data products.](./advanced-ci.md)
