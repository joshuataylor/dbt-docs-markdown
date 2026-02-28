# Database permissions

Database permissions are access rights and privileges granted to users or roles within a database or data platform. They help you specify what actions users or roles can perform on various database objects, like tables, views, schemas, or even the entire database.

### Why are they useful[​](#why-are-they-useful "Direct link to Why are they useful")

* Database permissions are essential for security and data access control.
* They ensure that only authorized users can perform specific actions.
* They help maintain data integrity, prevent unauthorized changes, and limit exposure to sensitive data.
* Permissions also support compliance with data privacy regulations and auditing.

### How to use them[​](#how-to-use-them "Direct link to How to use them")

* Users and administrators can grant and manage permissions at various levels (such as table, schema, and so on) using SQL statements or through the database system's interface.
* Assign permissions to individual users or roles (groups of users) based on their responsibilities.
  <!-- -->
  * Typical permissions include "SELECT" (read), "INSERT" (add data), "UPDATE" (modify data), "DELETE" (remove data), and administrative rights like "CREATE" and "DROP."
* Users should be assigned permissions that ensure they have the necessary access to perform their tasks without overextending privileges.

Something to note is that each data platform provider might have different approaches and names for privileges. Refer to their documentation for more details.

### Examples[​](#examples "Direct link to Examples")

Refer to the following database permission pages for more info on examples and how to set up database permissions:

* [Databricks](https://docs.getdbt.com/reference/database-permissions/databricks-permissions.md)
* [Postgres](https://docs.getdbt.com/reference/database-permissions/postgres-permissions.md)
* [Redshift](https://docs.getdbt.com/reference/database-permissions/redshift-permissions.md)
* [Snowflake](https://docs.getdbt.com/reference/database-permissions/snowflake-permissions.md)

## Was this page helpful?

YesNo

[Privacy policy](https://www.getdbt.com/cloud/privacy-policy)[Create a GitHub issue](https://github.com/dbt-labs/docs.getdbt.com/issues)

This site is protected by reCAPTCHA and the Google [Privacy Policy](https://policies.google.com/privacy) and [Terms of Service](https://policies.google.com/terms) apply.
