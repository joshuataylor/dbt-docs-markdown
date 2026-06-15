# Salesforce Data 360 setup


# Salesforce Data 360 setup <Lifecycle status="beta"/>

This `dbt-salesforce` adapter is available via the <Constant name="fusion_engine" /> CLI. To access the adapter, [install <Constant name="fusion" />](/docs/fusion/about-fusion-install). We recommend using the [VS Code Extension](/docs/local/install-dbt?version=2) as the development interface. <Constant name="dbt_platform" /> support coming soon. 

import SalesforcePrereqs from '/snippets/_salesforce-data-cloud-prereqs.md';

<SalesforcePrereqs />

## Configure Fusion

To connect dbt to Salesforce Data 360, set up your `profiles.yml`. Refer to the following configuration:

<File name='~/.dbt/profiles.yml'>

```yaml
company-name:
  target: dev
  outputs:
    dev:
      type: salesforce
      method: jwt_bearer
      client_id: [Consumer Key of your Data 360 app]
      private_key_path: [local file path of your server key]
      login_url: "https://login.salesforce.com"
      username: [username on the Data 360 Instance]
```
</File>


| Profile field | Required | Description | Example |
| --- | --- | --- | --- |
| `method` | Yes | Authentication Method. Currently, only `jwt_bearer` supported. | `jwt_bearer` |
| `client_id` | Yes | This is the `Consumer Key` from your connected app secrets. |  |
| `private_key_path` | Yes | File path of the `server.key` file in your computer. | `/Users/dbt_user/Documents/server.key` |
| `login_url` | Yes | Login URL of the Salesforce instance.  | [https://login.salesforce.com](https://login.salesforce.com/) |
| `username` | Yes | Username on the Data 360 Instance. | dbt_user@dbtlabs.com |


## More information

Find Salesforce-specific configuration information in the [Salesforce adapter reference guide](/reference/resource-configs/data-cloud-configs).