# Tenancy

dbt platform

dbt is available in single-tenant (virtual private) and multi-tenant SaaS configurations. Many multi-tenant accounts use cell-based hosting with account-specific access URLs. For more information, refer to [Multi-cell hosting](./tenancy.md#multi-cell-hosting).

## Multi-tenant

The Multi Tenant (SaaS) deployment environment refers to the SaaS dbt application hosted by dbt Labs. This is the most commonly used deployment and is completely managed and maintained by dbt Labs, the makers of dbt. As a SaaS product, a user can quickly [create an account](https://www.getdbt.com/signup/) on our North American servers and get started using the dbt and related services immediately. *If your organization requires cloud services hosted on EMEA or APAC regions*, please [contact us](https://www.getdbt.com/contact/). The deployments are hosted on AWS, Azure, or GCP and are always kept up to date with the currently supported dbt versions, software updates, and bug fixes.

#### Multi-cell hosting

Multi-cell (also called cell-based hosting) means your dbt platform account runs in a cell: a defined slice of our shared SaaS stack with its own capacity, scaling, and status boundaries. Cells segment how we run multi-tenant infrastructure at scale; you still remain on the same multi-tenant product managed by dbt Labs. Cell-based hosting is different from [single tenant](#single-tenant) in that it doesn't provide a dedicated virtual private cloud (VPC) or isolated cloud account on its own.

Generally, your plan and the features available to you stay the same as for other multi-tenant accounts in your managed cloud provider and region (described in [Available features](#available-features)). The main differences are in some setup details, such as the URL you use to sign in, which IP addresses to allow, and which status page to monitor if something goes wrong in your cell. Refer to the [API access URLs](./access-regions-ip-addresses.md#api-access-urls) section for more information.

## Single tenant

The single tenant deployment environment provides a hosted alternative to the multi-tenant (SaaS) dbt environment. While still managed and maintained by dbt Labs, single tenant dbt instances provide dedicated infrastructure in a VPC environment. This is accomplished by spinning up all the necessary infrastructure with a re-usable Infrastructure as Code (IaC) deployment built with [Terraform](https://www.terraform.io/). The single tenant infrastructure lives in a dedicated AWS, Azure, or GCP account and can be customized with certain configurations, such as firewall rules, to limit inbound traffic or hosting in a specific region.

A few common reasons for choosing a single tenant deployment over the Production SaaS product include:

* A requirement that the dbt application be hosted in a dedicated VPC that is logically separated from other customer infrastructure
* A desire for multiple isolated dbt instances for testing, development, etc

*To learn more about setting up a dbt single tenant deployment, [please contact our sales team](mailto:sales@getdbt.com).*

## Available features

The following table outlines which dbt features are supported on the different SaaS options available today. For more information about feature availability, please [contact us](https://www.getdbt.com/contact/).

Cell-based (multi-cell) accounts are still multi-tenant SaaS. Use the multi-tenant column for your cloud provider (for example, the AWS Multi-tenant column). This table does not list features by cell. For differences in hosting and access URLs compared with single tenant, refer to [Multi-cell hosting](./tenancy.md#multi-cell-hosting).

| Feature                     | AWS Multi-tenant | AWS single tenant | Azure multi-tenant | Azure single tenant | GCP multi-tenant | GCP single tenant |
| --------------------------- | ---------------- | ----------------- | ------------------ | ------------------- | ---------------- | ----------------- |
| Audit logs                  | ✅               | ✅                | ✅                 | ✅                  | ✅               | ✅                |
| Continuous integration jobs | ✅               | ✅                | ✅                 | ✅                  | ✅               | ✅                |
| dbt platform CLI            | ✅               | ✅                | ✅                 | ✅                  | ✅               | ✅                |
| Studio IDE                  | ✅               | ✅                | ✅                 | ✅                  | ✅               | ✅                |
| dbt Wizard                  | ✅               | ✅                | ✅                 | ✅                  | ✅               | ✅                |
| Catalog                     | ✅               | ✅                | ✅                 | ✅                  | ✅               | ✅                |
| Mesh                        | ✅               | ✅                | ✅                 | ✅                  | ✅               | ✅                |
| Semantic Layer              | ✅               | ✅                | ✅                 | ✅                  | ✅               | ✅                |
| Discovery API               | ✅               | ✅                | ✅                 | ✅                  | ✅               | ✅                |
| IP restrictions             | ✅               | ✅                | ✅                 | ✅                  | ✅               | ✅                |
| Orchestrator                | ✅               | ✅                | ✅                 | ✅                  | ✅               | ✅                |
| PrivateLink egress          | ✅               | ✅                | ✅                 | ✅                  | ✅               | ✅                |
| PrivateLink ingress         | ❌               | ✅                | ❌                 | ✅                  | ❌               | ❌                |
| Webhooks (Outbound)         | ✅               | ✅                | ✅                 | ❌                  | ❌               | ❌                |
