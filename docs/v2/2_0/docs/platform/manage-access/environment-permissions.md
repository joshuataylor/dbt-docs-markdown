# About environment-level permissions

Environment-level permissions give dbt admins the ability to grant write permission to groups and service tokens for specific [environment types](https://docs.getdbt.com/docs/dbt-platform-environments.md) within a project. Granting access to an environment gives users access to all environment-level write actions and resources associated with their assigned roles. For example, users with a Developer role can create and run jobs within the environment(s) they have access to. For all other environments, those same users will have read-only access.

You can configure multiple rows for the same permission set within a group when each row has different project scope and/or environment write access.

For configuration instructions, check out the [setup page](https://docs.getdbt.com/docs/platform/manage-access/environment-permissions-setup.md).

## Current limitations[​](#current-limitations "Direct link to Current limitations")

Environment-level permissions give dbt admins more flexibility to protect their environments, but it's important to understand that there are some limitations to this feature, so those admins can make informed decisions about granting access.

* Environment-level permissions do not allow you to create custom roles and permissions for each resource type in dbt.
* You can only select environment types, and can’t specify a particular environment within a project.
* You can't select specific resources within environments. dbt jobs and runs are environment resources.
  <!-- -->
  * For example, you can't specify that a user only has access to jobs but not runs. Access to a given environment gives the user access to everything within that environment.

## Environments and roles[​](#environments-and-roles "Direct link to Environments and roles")

dbt has four different environment types per project:

* **Production** — Primary deployment environment. Only one unique Production env per project.
* **Development** — Developer testing environment. Only one unique Development env per project.
* **Staging** — Pre-prod environment that sits between development and production. Only one unique Staging env per project.
* **General** — Mixed use environments. No limit on the number per project.

### Environment write permissions[​](#environment-write-permissions "Direct link to Environment write permissions")

Environment write permissions grant access to create, edit, and delete runs and jobs within an environment. However, they don't grant users access to create or delete environments themselves. See [Enterprise permissions](https://docs.getdbt.com/docs/platform/manage-access/enterprise-permissions.md) for more information about elevated permission sets.

Environment write permissions can be specified for the following roles:

* Analyst
* Database admin
* Developer\*
* Git admin
* Team admin

\* In the [enterprise permissions](https://docs.getdbt.com/docs/platform/manage-access/enterprise-permissions.md) table, the `Developer` role has read-only access to environment settings, but when added to a group, the **Environment write access** field defaults to `All environments`, which grants broader environment permissions than the table implies. The `Analyst`, `Database admin`, `Git admin`, and `Team admin` roles get **Read-only environment access** by default unless you specify different access under **Environment write access** for that group.

Depending on your current group mappings, you may have to update roles to ensure users have the correct access level to environments.

Determine what personas need updated environment access and the roles they should be mapped to. The personas below highlight a few scenarios for environment permissions:

* **Developer** — Write access to create/run jobs in non-production environments
* **Testing/QA** — Write access to staging and development environments to test
* **Production deployment** — Write access to all environments, including production, for deploying
* **Analyst** — Doesn't need environmental write access but read-only access for discovery and troubleshooting
* **Other admins** — These admins may need write access to create/run jobs or configure integrations for any number of environments

## Projects and environments[​](#projects-and-environments "Direct link to Projects and environments")

Environment-level permissions can be enforced over one or multiple projects with mixed access to the environments themselves.

### Single project environments[​](#single-project-environments "Direct link to Single project environments")

If you’re working with a single project, we recommend restricting access to the Production environment and ensuring groups have access to Development, Staging, or General environments where they can safely create and run jobs. The following is an example of how the personas could be mapped to roles:

* **Developer:** Developer role with write access to Development and General environments
* **Testing/QA:** Developer role with write access to Development, Staging, and General environments
* **Production Deployment:** Developer role with write access to all environments or Job Admin which has access to all environments by default.
* **Analyst:** Analyst role with no write access and read-only access to environments.
* **Other Admins:** Depends on the admin needs. For example, if they are managing the production deployment grant access to all environments.

### Multiple projects[​](#multiple-projects "Direct link to Multiple projects")

Let's say Acme corp has 12 projects and 3 of them belong to Finance, 3 belong to Marketing, 4 belong to Manufacturing, and 2 belong to Technology.

With mixed access across projects:

* **Developer:** If the user has the Developer role and has access to Projects A, B, C, then they only need access to Dev and General environments.
* **Testing/QA:** If they have the Developer role and they have access to Projects A, B, C, then they only need access to Development, Staging, and General environments.
* **Production Deployment:** If the user has the Admin *or* Developer role *and* they have access to Projects A, B, C, then they need access to all Environments.
* **Analyst:** If the user has the Analyst role, then the need *no* write access to *any environment*.
* **Other Admins:** A user (non-Admin) can have access to multiple projects depending on the requirements.

If the user has the same roles across projects, you can apply environment access across all projects.

## Related docs[​](#related-docs "Direct link to Related docs")

* [Environment-level permissions setup](https://docs.getdbt.com/docs/platform/manage-access/environment-permissions-setup.md)
