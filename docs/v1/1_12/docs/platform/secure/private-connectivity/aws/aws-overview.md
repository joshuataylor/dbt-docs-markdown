# AWS private connectivity

dbt platform | Enterprise+

Available to certain Enterprise tiers

This feature is available on the following dbt Enterprise tiers:

* Enterprise+
* Virtual Private

To learn more about these tiers, contact us at <sales@getdbt.com>.

AWS PrivateLink enables secure, private connectivity between dbt and your AWS-hosted services. With PrivateLink, traffic between dbt and your data platforms or self-hosted services stays within the AWS network and does not traverse the public internet.

For more details, refer to the [AWS PrivateLink documentation](https://docs.aws.amazon.com/vpc/latest/privatelink/).

## AWS private connectivity matrix

The following charts outline private connectivity options for AWS deployments of dbt ([multi-tenant and single-tenant](../../../about-platform/tenancy.md)).

**Legend:**

* ✅ = Available
* ❌ = Not currently available

*Tenancy:* MT (multi-tenant) and ST (single-tenant) — [learn more about tenancy](../../../about-platform/tenancy.md).

About the following matrix tables

These tables indicate whether private connectivity can be established to specific services, considering major factors such as the network and basic auth layers. dbt has validated these configurations using common deployment patterns and typical use cases. However, individual configurations may vary. If you encounter issues or have questions about your environment, [contact dbt Support](../../../../../community/resources/getting-help.md#dbt-cloud-support) for guidance.

***

### Connecting to the dbt platform (Ingress) [Beta](https://docs.getdbt.com/docs/dbt-versions/product-lifecycles "Go to https://docs.getdbt.com/docs/dbt-versions/product-lifecycles")

Your services can connect to dbt over private connectivity using the dbt-provisioned model. In this case, dbt is the service producer and you are the consumer.

| Connectivity type              | MT | ST | Setup guide                                                                                  |
| ------------------------------ | -- | -- | -------------------------------------------------------------------------------------------- |
| Private dbt access             | ❌ | ✅ | [View](./aws-ingress.md) |
| Dual access (public + private) | ❌ | ✅ | [View](./aws-ingress.md) |

***

### Connecting the dbt platform to managed services (Egress)

dbt can establish private connections to managed data platforms and cloud-native services.

| Service                    | MT | ST | Setup guide                                                                                     |
| -------------------------- | -- | -- | ----------------------------------------------------------------------------------------------- |
| Snowflake                  | ✅ | ✅ | [View](./aws-snowflake.md)  |
|   Snowflake Internal Stage | ✅ | ✅ | [View](./aws-snowflake.md)  |
| Databricks                 | ✅ | ✅ | [View](./aws-databricks.md) |
| Redshift                   | ✅ | ✅ | [View](./aws-redshift.md)   |
| Redshift Serverless        | ✅ | ✅ | [View](./aws-redshift.md)   |
| Amazon Athena w/ AWS Glue  | ❌ | ✅ |                                                                                                 |
| AWS CodeCommit             | ❌ | ✅ |                                                                                                 |
| Teradata VantageCloud      | ✅ | ✅ | [View](./aws-teradata.md)   |

***

### Connecting the dbt platform to self-hosted services (Egress)

All of the services below share a common PrivateLink setup guide — backend configuration varies by service. Self-hosted connections use the customer-provisioned model — you are the service producer and dbt is the consumer.

**Setup guide:** [Configuring AWS PrivateLink for self-hosted services](./aws-self-hosted.md)

| Service                  | MT | ST |
| ------------------------ | -- | -- |
| GitHub Enterprise Server | ✅ | ✅ |
| GitLab Self-Managed      | ✅ | ✅ |
| Bitbucket Data Center    | ✅ | ✅ |
| Azure DevOps Server      | ✅ | ✅ |
| Postgres                 | ✅ | ✅ |
| Spark                    | ✅ | ✅ |
| Starburst / Trino        | ✅ | ✅ |
| Teradata (self-hosted)   | ✅ | ✅ |

If you have questions about whether your specific architecture is supported, [contact dbt Support](../../../../../community/resources/getting-help.md#dbt-cloud-support).

## Cross-region private connections

dbt Labs has globally connected private networks specifically used to host private endpoints, which are connected to dbt instance environments. This connectivity allows dbt environments to connect to any supported region from any dbt instance within the same cloud provider network. To ensure security, access to these endpoints is protected by security groups, network policies, and application connection safeguards, in addition to the authentication and authorization mechanisms provided by each of the connected platforms.
