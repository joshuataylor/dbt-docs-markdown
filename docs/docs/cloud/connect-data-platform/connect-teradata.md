# Connect Teradata [Preview](https://docs.getdbt.com/docs/dbt-versions/product-lifecycles "Go to https://docs.getdbt.com/docs/dbt-versions/product-lifecycles")

Your environment(s) must be on a supported [release track](https://docs.getdbt.com/docs/dbt-versions/cloud-release-tracks.md) to use the Teradata connection.

| Field           | Description                                                                                      | Type           | Required? | Example                               |
| --------------- | ------------------------------------------------------------------------------------------------ | -------------- | --------- | ------------------------------------- |
| Host            | Host name of your Teradata environment.                                                          | String         | Required  | host-name.env.clearscape.teradata.com |
| Port            | The database port number. Equivalent to the Teradata JDBC Driver DBS\_PORT connection parameter. | Quoted integer | Optional  | 1025                                  |
| Retries         | Number of times to retry to connect to database upon error.                                      | Integer        | optional  | 10                                    |
| Request timeout | The waiting period between connections attempts in seconds. Default is "1" second.               | Quoted integer | Optional  | 3                                     |

Search table...

|                  |   |   |   |   |
| ---------------- | - | - | - | - |
| Loading table... |   |   |   |   |

[![Example of the Teradata connection fields.](/img/docs/dbt-cloud/teradata-connection.png?v=2 "Example of the Teradata connection fields.")](#)Example of the Teradata connection fields.

### Development and deployment credentials[​](#development-and-deployment-credentials "Direct link to Development and deployment credentials")

| Field    | Description                                                                                  | Type   | Required? | Example             |
| -------- | -------------------------------------------------------------------------------------------- | ------ | --------- | ------------------- |
| Username | The database username. Equivalent to the Teradata JDBC Driver USER connection parameter.     | String | Required  | database\_username  |
| Password | The database password. Equivalent to the Teradata JDBC Driver PASSWORD connection parameter. | String | Required  | DatabasePassword123 |
| Schema   | Specifies the initial database to use after login, rather than the user's default database.  | String | Required  | dbtlabsdocstest     |

Search table...

|                  |   |   |   |   |
| ---------------- | - | - | - | - |
| Loading table... |   |   |   |   |

[![Example of the developer credential fields.](/img/docs/dbt-cloud/teradata-deployment.png?v=2 "Example of the developer credential fields.")](#)Example of the developer credential fields.

## Was this page helpful?

YesNo

[Privacy policy](https://www.getdbt.com/cloud/privacy-policy)[Create a GitHub issue](https://github.com/dbt-labs/docs.getdbt.com/issues)

This site is protected by reCAPTCHA and the Google [Privacy Policy](https://policies.google.com/privacy) and [Terms of Service](https://policies.google.com/terms) apply.
