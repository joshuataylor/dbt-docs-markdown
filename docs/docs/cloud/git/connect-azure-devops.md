# Connect to Azure DevOps [Enterprise](https://www.getdbt.com/pricing "Go to https://www.getdbt.com/pricing")[Enterprise +](https://www.getdbt.com/pricing "Go to https://www.getdbt.com/pricing")

Available for dbt Enterprise and Enterprise+

Connecting an Azure DevOps cloud account is available for organizations using the dbt Enterprise or Enterprise+ plans.

dbt's native Azure DevOps integration does not support Azure DevOps Server (on-premise). Instead, you can [import a project by git URL](https://docs.getdbt.com/docs/cloud/git/import-a-project-by-git-url.md) to connect to an Azure DevOps Server.

## About Azure DevOps and dbt[​](#about-azure-devops-and-dbt "Direct link to About Azure DevOps and dbt")

Connect your Azure DevOps cloud account in dbt to unlock new product experiences:

* Import new Azure DevOps repos with a couple clicks during dbt project setup.
* Clone repos using HTTPS rather than SSH
* Enforce user authorization with OAuth 2.0.
* Carry Azure DevOps user repository permissions (read / write access) through to Studio IDE or dbt CLI's git actions.
* Trigger Continuous integration (CI) builds when pull requests are opened in Azure DevOps.

Currently, there are multiple methods for integrating Azure DevOps with dbt. The following methods are available to all accounts:

* [**Service principal (recommended)**](https://docs.getdbt.com/docs/cloud/git/setup-service-principal.md)
* [**Service user (legacy)**](https://docs.getdbt.com/docs/cloud/git/setup-service-user.md)
* [**Service user to service principal migration**](https://docs.getdbt.com/docs/cloud/git/setup-service-principal.md#migrate-to-service-principal)

No matter which approach you take, you will need admins for dbt, Azure Entra ID, and Azure DevOps to complete the integration. For more information, follow the setup guide that's right for you.

## Was this page helpful?

YesNo

[Privacy policy](https://www.getdbt.com/cloud/privacy-policy)[Create a GitHub issue](https://github.com/dbt-labs/docs.getdbt.com/issues)

This site is protected by reCAPTCHA and the Google [Privacy Policy](https://policies.google.com/privacy) and [Terms of Service](https://policies.google.com/terms) apply.
