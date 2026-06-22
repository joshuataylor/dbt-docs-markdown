# IBM Netezza configurations

## Instance requirements[​](#instance-requirements "Direct link to Instance requirements")

To use IBM Netezza with `dbt-ibm-netezza` adapter, ensure the instance has an attached catalog that supports creating, renaming, altering, and dropping objects such as tables and views. The user connecting to the instance via the `dbt-ibm-netezza` adapter must have the necessary permissions for the target database.

For more details, please visit the official [IBM documentation](https://cloud.ibm.com/docs/netezza?topic=netezza-getstarted)

### IBM Netezza SQL Extension Toolkit[​](#ibm-netezza-sql-extension-toolkit "Direct link to IBM Netezza SQL Extension Toolkit")

Ensure that you have the SQL Extension Toolkit installed on your IBM Netezza system. This is a pre-reqsuisite to run all the function which require string data manipulation and view options. Check [docs](https://www.ibm.com/docs/en/netezza?topic=toolkit-sql-extensions-installation-setup) for more details.

## Seeds and prepared statements[​](#seeds-and-prepared-statements "Direct link to Seeds and prepared statements")

The `dbt-ibm-netezza` adapter offers comprehensive support for all [datatypes](https://www.ibm.com/docs/en/netezza?topic=nrl-data-types) in seed files. To leverage this functionality, you must explicitly define the data types for each column.

You can configure column data types either in the dbt\_project.yml file or in property files, as supported by dbt. For more details on seed configuration and best practices, refer to the [dbt seed configuration documentation](https://docs.getdbt.com/reference/seed-configs.md).

### Recommendations[​](#recommendations "Direct link to Recommendations")

* **Check SQL Documentation:** Review IBM Netezza [SQL command reference ](https://www.ibm.com/docs/en/netezza?topic=dud-netezza-performance-server-sql-command-reference)to create your dbt project.

## Was this page helpful?

YesNo

[Privacy policy](https://www.getdbt.com/cloud/privacy-policy)[Create a GitHub issue](https://github.com/dbt-labs/docs.getdbt.com/issues)

This site is protected by reCAPTCHA and the Google [Privacy Policy](https://policies.google.com/privacy) and [Terms of Service](https://policies.google.com/terms) apply.
