# About private connectivity

Available to certain Enterprise tiers

The private connection feature is available on the following dbt Enterprise tiers:

* Business Critical
* Virtual Private

To learn more about these tiers, contact us at <sales@getdbt.com>.

Environment variables

Using [Environment variables](https://docs.getdbt.com/docs/build/environment-variables.md) when configuring private connection endpoints isn't supported in dbt. Instead, use [Extended Attributes](https://docs.getdbt.com/docs/deploy/deploy-environments.md#extended-attributes) to dynamically change these values in your dbt environment.

Private connections enables secure communication from any dbt environment to your data platform hosted on a cloud provider, such as [AWS](https://aws.amazon.com/privatelink/), [Azure](https://azure.microsoft.com/en-us/products/private-link), or [GCP](https://cloud.google.com/vpc/docs/private-service-connect), using that provider's private connection technology. Private connections allow dbt customers to meet security and compliance controls as it allows connectivity between dbt and your data platform without traversing the public internet. This feature is supported in most regions across North America, Europe, and Asia, but [contact us](https://www.getdbt.com/contact/) if you have questions about availability.

Private connection endpoints can't connect across cloud providers (AWS, Azure, and GCP). For a private connection to work, both dbt and the server (like <!-- -->) must be hosted on the same cloud provider. For example, dbt hosted on AWS cannot connect to services hosted on Azure, and dbt hosted on Azure can’t connect to services hosted on GCP.

## Available platforms[​](#available-platforms "Direct link to Available platforms")

Select your cloud platform to view private connectivity options, support matrix, and configuration guides.

[![](/img/icons/dbt-bit.svg)](https://docs.getdbt.com/docs/cloud/secure/private-connectivity/aws/aws-overview.md)

#### [AWS](https://docs.getdbt.com/docs/cloud/secure/private-connectivity/aws/aws-overview.md)

[Amazon Web Services PrivateLink](https://docs.getdbt.com/docs/cloud/secure/private-connectivity/aws/aws-overview.md)

[![](/img/icons/dbt-bit.svg)](https://docs.getdbt.com/docs/cloud/secure/private-connectivity/azure/azure-overview.md)

#### [Azure](https://docs.getdbt.com/docs/cloud/secure/private-connectivity/azure/azure-overview.md)

[Microsoft Azure Private Link](https://docs.getdbt.com/docs/cloud/secure/private-connectivity/azure/azure-overview.md)

[![](/img/icons/dbt-bit.svg)](https://docs.getdbt.com/docs/cloud/secure/private-connectivity/gcp/gcp-overview.md)

#### [GCP](https://docs.getdbt.com/docs/cloud/secure/private-connectivity/gcp/gcp-overview.md)

[Google Cloud Platform Private Service Connect](https://docs.getdbt.com/docs/cloud/secure/private-connectivity/gcp/gcp-overview.md)

## Was this page helpful?

YesNo

[Privacy policy](https://www.getdbt.com/cloud/privacy-policy)[Create a GitHub issue](https://github.com/dbt-labs/docs.getdbt.com/issues)

This site is protected by reCAPTCHA and the Google [Privacy Policy](https://policies.google.com/privacy) and [Terms of Service](https://policies.google.com/terms) apply.
