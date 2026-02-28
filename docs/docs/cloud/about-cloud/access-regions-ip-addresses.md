# Access, Regions, & IP addresses

dbt is [hosted](https://docs.getdbt.com/docs/cloud/about-cloud/architecture.md) in multiple regions across the following service providers:

* [Amazon Web Services](#AWS)
* [Google Cloud Platform](#GCP)
* [Microsoft Azure](#Azure)

Your dbt account will always connect to your data platform or git provider from the below IP addresses. Be sure to allow traffic from these IPs in your firewall, and include them in any database grants.

* [dbt Enterprise-tier](https://www.getdbt.com/pricing/) plans can choose to have their account hosted in any of the regions listed in the following table.
* Organizations **must** choose a single region per dbt account. To run dbt in multiple regions, we recommend using multiple dbt accounts.

## Amazon Web Services (AWS)[​](#AWS "Direct link to Amazon Web Services (AWS)")

|   |
| - |

| Region                               | Location                    | Access URL                  | IP addresses                                                                                              | Available plans                                           | Status page link                                                                                                                                                                                                                                                                       |
| ------------------------------------ | --------------------------- | --------------------------- | --------------------------------------------------------------------------------------------------------- | --------------------------------------------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| North America                        | AWS us-east-1 (N. Virginia) | ACCOUNT\_PREFIX.us1.dbt.com | 52.45.144.63<br />54.81.134.249<br />52.22.161.231<br />52.3.77.232<br />3.214.191.130<br />34.233.79.135 | [All dbt platform plans](https://www.getdbt.com/pricing/) | **Multi-tenant:**<br />[US AWS](https://status.getdbt.com/us-aws)<br /><br />**Cell based:**<br />[US Cell 1 AWS](https://status.getdbt.com/us-cell-1-aws)<br />[US Cell 2 AWS](https://status.getdbt.com/us-cell-2-aws)<br />[US Cell 3 AWS](https://status.getdbt.com/us-cell-3-aws) |
| EMEA                                 | eu-central-1 (Frankfurt)    | ACCOUNT\_PREFIX.eu1.dbt.com | 3.123.45.39<br />3.126.140.248<br />3.72.153.148                                                          | All Enterprise plans                                      | [EMEA AWS](https://status.getdbt.com/emea-aws)                                                                                                                                                                                                                                         |
| APAC                                 | ap-southeast-2 (Sydney)     | ACCOUNT\_PREFIX.au1.dbt.com | 52.65.89.235<br />3.106.40.33<br />13.239.155.206<br />                                                   | All Enterprise plans                                      | [APAC AWS](https://status.getdbt.com/apac-aws)                                                                                                                                                                                                                                         |
| Japan                                | ap-northeast-1 (Tokyo)      | ACCOUNT\_PREFIX.jp1.dbt.com | 35.76.76.152<br />54.238.211.79<br />13.115.236.233<br />                                                 | All Enterprise plans                                      | [JP Cell 1 AWS](https://status.getdbt.com/jp-cell-1-aws)                                                                                                                                                                                                                               |
| Virtual Private dbt or Single tenant | Customized                  | Customized                  | Ask [Support](https://docs.getdbt.com/community/resources/getting-help.md#dbt-cloud-support) for your IPs | All Enterprise plans                                      | Customized                                                                                                                                                                                                                                                                             |

Search table...

|                  |   |   |   |   |
| ---------------- | - | - | - | - |
| Loading table... |   |   |   |   |

Search table...

|                  |   |   |   |   |
| ---------------- | - | - | - | - |
| Loading table... |   |   |   |   |
