# Azure private connectivity

dbt platform | Enterprise+

Available to certain Enterprise tiers

This feature is available on the following dbt Enterprise tiers:

* Enterprise+
* Virtual Private

To learn more about these tiers, contact us at <sales@getdbt.com>.

Azure Private Link enables secure, private connectivity between dbt and your Azure-hosted services. With Private Link, traffic between dbt and your data platforms or self-hosted services stays within the Azure network and does not traverse the public internet.

For more details, refer to the [Azure Private Link documentation](https://learn.microsoft.com/en-us/azure/private-link/).

## Azure private connectivity matrix

The following charts outline private connectivity options for Azure deployments of dbt ([multi-tenant and single-tenant](../../../about-platform/tenancy.md)).

**Legend:**

* ✅ = Available
* ❌ = Not currently available

*Tenancy:* MT (multi-tenant) and ST (single-tenant) — [learn more about tenancy](../../../about-platform/tenancy.md).

About the following matrix tables

These tables indicate whether private connectivity can be established to specific services, considering major factors such as the network and basic auth layers. dbt has validated these configurations using common deployment patterns and typical use cases. However, individual configurations may vary. If you encounter issues or have questions about your environment, [contact dbt Support](../../../../../community/resources/getting-help.md#dbt-cloud-support) for guidance.

***

### Connecting to the dbt platform (Ingress)

Your services can connect to dbt over private connectivity using the dbt-provisioned model. In this case, dbt is the service producer and you are the consumer.

| Connectivity type              | MT | ST |
| ------------------------------ | -- | -- |
| Private dbt access             | ❌ | ✅ |
| Dual access (public + private) | ❌ | ✅ |

***

### Connecting the dbt platform to managed services (Egress)

dbt can establish private connections to managed data platforms and cloud-native services.

| Service                                                                                                                                                | MT | ST | Setup guide                                                                                         |
| ------------------------------------------------------------------------------------------------------------------------------------------------------ | -- | -- | --------------------------------------------------------------------------------------------------- |
| Snowflake                                                                                                                                              | ✅ | ✅ | [View](./azure-snowflake.md)  |
|   Snowflake Internal Stage                                                                                                                             | ✅ | ✅ | [View](./azure-snowflake.md)  |
| Databricks                                                                                                                                             | ✅ | ✅ | [View](./azure-databricks.md) |
| Azure Database for PostgreSQL Flexible Server                                                                                                          | ✅ | ✅ | [View](./azure-postgres.md)   |
| Azure Synapse                                                                                                                                          | ✅ | ✅ | [View](./azure-synapse.md)    |
| Azure Fabric [Beta](https://docs.getdbt.com/docs/dbt-versions/product-lifecycles "Go to https://docs.getdbt.com/docs/dbt-versions/product-lifecycles") | ✅ | ✅ | [View](./azure-fabric.md)     |
| Teradata VantageCloud                                                                                                                                  | ✅ | ✅ | [View](./azure-teradata.md)   |

***

### Connecting the dbt platform to self-hosted services (Egress)

All of the services below share a common Private Link setup guide — backend configuration varies by service. Self-hosted connections use the customer-provisioned model — you are the service producer and dbt is the consumer.

**Setup guide:** [Configuring Azure Private Link for self-hosted services](./azure-self-hosted.md)

| Service                  | MT | ST |
| ------------------------ | -- | -- |
| GitHub Enterprise Server | ✅ | ✅ |
| GitLab Self-Managed      | ✅ | ✅ |
| Bitbucket Data Center    | ✅ | ✅ |
| Azure DevOps Server      | ✅ | ✅ |
| Postgres                 | ✅ | ✅ |
| Starburst / Trino        | ✅ | ✅ |
| Teradata (self-hosted)   | ✅ | ✅ |

If you have questions about whether your specific architecture is supported, [contact dbt Support](../../../../../community/resources/getting-help.md#dbt-cloud-support).

## Cross-region private connections

dbt Labs has globally connected private networks specifically used to host private endpoints, which are connected to dbt instance environments. This connectivity allows dbt environments to connect to any supported region from any dbt instance within the same cloud provider network. To ensure security, access to these endpoints is protected by security groups, network policies, and application connection safeguards, in addition to the authentication and authorization mechanisms provided by each of the connected platforms.
