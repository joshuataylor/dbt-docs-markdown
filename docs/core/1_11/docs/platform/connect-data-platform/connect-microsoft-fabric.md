# Connect Microsoft Fabric

## Supported authentication methods[​](#supported-authentication-methods "Direct link to Supported authentication methods")

The supported authentication methods are:

* Microsoft Entra service principal
* Microsoft Entra password

SQL password (LDAP) is not supported in Microsoft Fabric Data Warehouse so you must use Microsoft Entra ID. This means that to use [Microsoft Fabric](https://www.microsoft.com/en-us/microsoft-fabric) in dbt, you will need at least one Microsoft Entra service principal to connect dbt to Fabric, ideally one service principal for each user.

### Microsoft Entra service principal[​](#microsoft-entra-service-principal "Direct link to Microsoft Entra service principal")

The following are the required fields for setting up a connection with a Microsoft Fabric using Microsoft Entra service principal authentication.

| Field              | Description                                                                                                              |
| ------------------ | ------------------------------------------------------------------------------------------------------------------------ |
| **Server**         | The service principal's **host** value for the Fabric test endpoint.                                                     |
| **Port**           | The port to connect to Microsoft Fabric. You can use `1433` (the default), which is the standard SQL server port number. |
| **Database**       | The service principal's **database** value for the Fabric test endpoint.                                                 |
| **Authentication** | Choose **Service Principal** from the dropdown.                                                                          |
| **Tenant ID**      | The service principal's **Directory (tenant) ID**.                                                                       |
| **Client ID**      | The service principal's **application (client) ID id**.                                                                  |
| **Client secret**  | The service principal's **client secret** (not the **client secret id**).                                                |

Search table...

|                  |   |   |   |   |
| ---------------- | - | - | - | - |
| Loading table... |   |   |   |   |

### Microsoft Entra password[​](#microsoft-entra-password "Direct link to Microsoft Entra password")

The following are the required fields for setting up a connection with a Microsoft Fabric using Microsoft Entra password authentication.

| Field              | Description                                                                                      |
| ------------------ | ------------------------------------------------------------------------------------------------ |
| **Server**         | The server hostname to connect to Microsoft Fabric.                                              |
| **Port**           | The server port. You can use `1433` (the default), which is the standard SQL server port number. |
| **Database**       | The database name.                                                                               |
| **Authentication** | Choose **Active Directory Password** from the dropdown.                                          |
| **User**           | The Microsoft Entra username.                                                                    |
| **Password**       | The Microsoft Entra password.                                                                    |

Search table...

|                  |   |   |   |   |
| ---------------- | - | - | - | - |
| Loading table... |   |   |   |   |

## Configuration[​](#configuration "Direct link to Configuration")

To learn how to optimize performance with data platform-specific configurations in dbt, refer to [Microsoft Fabric Data Warehouse configurations](https://docs.getdbt.com/reference/resource-configs/fabric-configs.md).

## Was this page helpful?

YesNo

[Privacy policy](https://www.getdbt.com/cloud/privacy-policy)[Create a GitHub issue](https://github.com/dbt-labs/docs.getdbt.com/issues)

This site is protected by reCAPTCHA and the Google [Privacy Policy](https://policies.google.com/privacy) and [Terms of Service](https://policies.google.com/terms) apply.
