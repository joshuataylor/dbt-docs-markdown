# GCP private connectivity

Available to certain Enterprise tiers

The private connection feature is available on the following dbt Enterprise tiers:

* Business Critical
* Virtual Private

To learn more about these tiers, contact us at <sales@getdbt.com>.

GCP Private Service Connect enables secure, private connectivity between dbt and your GCP-hosted services. With Private Service Connect, traffic between dbt and your data platforms or self-hosted services stays within the Google Cloud network and does not traverse the public internet.

For more details, refer to the [GCP Private Service Connect documentation](https://cloud.google.com/vpc/docs/private-service-connect).

## GCP private connectivity matrix[​](#gcp-private-connectivity-matrix "Direct link to GCP private connectivity matrix")

The following charts outline private connectivity options for GCP deployments of dbt ([multi-tenant](https://docs.getdbt.com/docs/cloud/about-cloud/tenancy.md)).

**Legend:**

* ✅ = Available
* ❌ = Not currently available
* \* = Shared endpoint (all others are dedicated)

*Tenancy:* MT (multi-tenant) — [learn more about tenancy](https://docs.getdbt.com/docs/cloud/about-cloud/tenancy.md).

About the following matrix tables

These tables indicate whether private connectivity can be established to specific services, considering major factors such as the network and basic auth layers. dbt has validated these configurations using common deployment patterns and typical use cases. However, individual configurations may vary. If you encounter issues or have questions about your environment, [contact dbt Support](https://docs.getdbt.com/community/resources/getting-help.md#dbt-cloud-support) for guidance.

**GCP regional considerations:** Some GCP services, such as BigQuery, may have regional restrictions for Private Service Connect endpoints. Refer to [Google's Private Service Connect documentation](https://cloud.google.com/vpc/docs/private-service-connect) for service-specific regional availability.

***

### Connecting the dbt platform to managed services (Egress)[​](#connecting-the-dbt-platform-to-managed-services-egress "Direct link to Connecting the dbt platform to managed services (Egress)")

dbt can establish private connections to managed data platforms and cloud-native services.

| Service               | MT   | Setup guide                                                                                 |
| --------------------- | ---- | ------------------------------------------------------------------------------------------- |
| Snowflake             | ✅   | [View](https://docs.getdbt.com/docs/cloud/secure/private-connectivity/gcp/gcp-snowflake.md) |
| Google BigQuery       | ✅\* | [View](https://docs.getdbt.com/docs/cloud/secure/private-connectivity/gcp/gcp-bigquery.md)  |
| Teradata VantageCloud | ✅   |                                                                                             |

Search table...

|                  |   |   |   |   |
| ---------------- | - | - | - | - |
| Loading table... |   |   |   |   |

***

### Connecting the dbt platform to self-hosted services (Egress)[​](#connecting-the-dbt-platform-to-self-hosted-services-egress "Direct link to Connecting the dbt platform to self-hosted services (Egress)")

All of the services below share a common Private Service Connect setup guide — backend configuration varies by service. Self-hosted connections use the customer-provisioned model — you are the service producer and dbt is the consumer.

**Setup guide:** [Configuring GCP Private Service Connect for self-hosted services](https://docs.getdbt.com/docs/cloud/secure/private-connectivity/gcp/gcp-self-hosted.md)

| Service                  | MT |
| ------------------------ | -- |
| GitHub Enterprise Server | ✅ |
| GitLab Self-Managed      | ✅ |
| Bitbucket Data Center    | ✅ |
| Azure DevOps Server      | ✅ |
| Postgres                 | ✅ |
| Starburst / Trino        | ✅ |
| Teradata (self-hosted)   | ✅ |

Search table...

|                  |   |   |   |   |
| ---------------- | - | - | - | - |
| Loading table... |   |   |   |   |

If you have questions about whether your specific architecture is supported, [contact dbt Support](https://docs.getdbt.com/community/resources/getting-help.md#dbt-cloud-support).

## Cross-region private connections[​](#cross-region-private-connections "Direct link to Cross-region private connections")

dbt Labs has globally connected private networks specifically used to host private endpoints, which are connected to dbt instance environments. This connectivity allows dbt environments to connect to any supported region from any dbt instance within the same cloud provider network. To ensure security, access to these endpoints is protected by security groups, network policies, and application connection safeguards, in addition to the authentication and authorization mechanisms provided by each of the connected platforms.

## Was this page helpful?

YesNo

[Privacy policy](https://www.getdbt.com/cloud/privacy-policy)[Create a GitHub issue](https://github.com/dbt-labs/docs.getdbt.com/issues)

This site is protected by reCAPTCHA and the Google [Privacy Policy](https://policies.google.com/privacy) and [Terms of Service](https://policies.google.com/terms) apply.
