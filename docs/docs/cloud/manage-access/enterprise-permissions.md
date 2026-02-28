# Enterprise permissions [Enterprise](https://www.getdbt.com/pricing "Go to https://www.getdbt.com/pricing")[Enterprise +](https://www.getdbt.com/pricing "Go to https://www.getdbt.com/pricing")

<!-- -->

Available to Enterprise-tier plans

This feature is available to the dbt Enterprise and Enterprise+ plans. If you're interested in learning more, contact us at <sales@getdbt.com>.

The dbt Enterprise and Enterprise+ plans support a number of pre-built permission sets to help manage access controls within a dbt account. See the docs on [access control](https://docs.getdbt.com/docs/cloud/manage-access/about-user-access.md) for more information on Role-Based access control (RBAC).

## Permission sets[​](#permission-sets "Direct link to Permission sets")

The following permission sets are available for assignment in all dbt Enterprise-tier accounts. They can be granted to dbt groups and then to users. A dbt group can be associated with more than one permission set. Permission assignments with more access take precedence.

Access to dbt features and functionality is split into `account-level` and `project-level` permission sets. Account-level permissions are primarily for account administration (inviting users, configuring SSO, and creating groups). Project-level permissions are for the configuration and maintenance of the projects themselves (configuring environments, accessing IDE, and running jobs). Account permission sets may have access to project features, and project permission sets may have access to account features. Check out the [permissions tables](https://docs.getdbt.com/docs/cloud/manage-access/enterprise-permissions.md#account-permissions) to compare sets and their access.

 Account admin

The Account admin permission set is the highest level of access and control over your dbt account and projects. We recommend limiting the number of users and groups assigned the account admin permission set.

Notable features:

* Account admin is an account-level set.
* Unrestricted access to every feature.
* The default permissions for every user who creates a new dbt account.
* The default permissions assigned to the `Owner` group.

 Admin

The Admin permission set is intended for project administration but with limited account-level access to invite and assign users.

Notable features:

* Admin is a project-level set.
* Unrestricted access to existing projects, but can't create new projects.
* Can invite new members and assign access but can't create groups.
* Can access Catalog.
* The default permissions assigned to the `Member` group.

 Analyst

The Analyst permission set is designed for users who need to run and analyze dbt models in the IDE but can't create or edit anything outside the IDE.

Notable features:

* Analyst is a project-level set.
* Full access to the IDE and the ability to configure personal credentials for adapters and Git.
* Read-only access to environment configs.
* Can view jobs but can't edit them.
* Can access Catalog.

 Billing admin

The Billing admin permission set can review product usage information that impacts the final billing of dbt (for example, models run).

Notable features:

* Billing admin is an account-level set.
* Unrestricted access to the **Billing** section of your **Account settings**.
* Read access to public models.
* No other access.

 Cost Insights Admin

The Cost Insights Admin permission set provides the minimum permissions needed to configure and manage [Cost Insights](https://docs.getdbt.com/docs/explore/cost-insights.md) settings and view cost data.

Notable features:

* Cost Insights Admin is both an account-level and project-level set.
* Can configure platform metadata credentials and Cost Insights settings in connection settings.
* Can view cost and savings data across projects, models, and jobs.
* Read-only access to connections, projects, jobs, and metadata.
* Can access dbt Catalog.

 Cost Insights Viewer

The Cost Insights Viewer permission set provides read-only access to [Cost Insights](https://docs.getdbt.com/docs/explore/cost-insights.md) data with the minimum permissions needed to view estimated cost and reduction information.

Notable features:

* Cost Insights Viewer is both an account-level and project-level set.
* Read-only access to cost and savings data across projects, models, and jobs.
* Read-only access to connections, platform metadata credentials, projects, jobs, and metadata.
* Cannot configure or edit Cost Insights settings.
* Can access dbt Catalog.

 Database admin

Database admins manage configurations between dbt and the underlying databases.

Notable features:

* Database admin is a project-level set.
* Can set up and maintain environment variables and Semantic Layer configs.
* Write access to data platform configurations within environments (credentials, warehouse, schema per environment).
* Helpful for scenarios where your data warehouse admins only need access to dbt to configure data platform settings within environments.
* Read-only access to account-level connections, Git repo, job, and run settings.
* Can access Catalog.

 Developer

The Developer permission set is intended for users who build and maintain dbt models under development and manage production behavior. This is the primary permission set for users working in the IDE and should not be conflated with the [Developer license](https://docs.getdbt.com/docs/cloud/manage-access/seats-and-users.md#licenses).

Notable features:

* Developer is a project-level set.
* Can create, edit, and test dbt code in the IDE.
* Read-only access to the underlying configs for environments, jobs, runs, and Git.
* Users manage their credentials to data warehouses and Git.
* Can access Catalog.

 Fusion admin

This permission set is used exclusively to enable users to interact with the Fusion upgrade workflows. We recommend limiting this permission to users and projects that are Fusion-ready.

By default, all users can access the Fusion upgrade experience and perform upgrades based on their existing permissions. When the Fusion upgrade permissions setting is enabled (when you see a check mark), only users with the fusion admin or account admin permission set can perform upgrades. If the setting is disabled (no check mark), upgrades are not restricted.

When upgrade permissions are enabled:

* **Fusion admin** — Assign to user accounts. This permission cannot be assigned to service tokens.
* **Account admin** — Assign to users or service tokens. This permission allows both users and service tokens to perform upgrades.

See the [dbt platform Fusion upgrade](https://docs.getdbt.com/docs/dbt-versions/upgrade-dbt-version-in-cloud.md#dbt-fusion-engine) docs for more information.

 Git admin

Git admins manage Git repository integrations and cloning.

Notable features:

* Git admin is a project-level set.
* Can create new Git integrations and environment variables.
* Can edit project settings.
* Read-only access to account settings (including users and groups).
* No access to the IDE.
* Can access Catalog.

 Job admin

Job admin is an administrative permission set for users who create, run, and manage jobs in dbt.

Notable features:

* Job admin is a project-level set.
* Job admins can create and edit jobs, runs, environment variables, and data warehouse configs.
* Job admins can set up project integrations, including [Tableau lineage](https://docs.getdbt.com/docs/cloud-integrations/semantic-layer/tableau.md).
* Read-only access to project configs.
* Read-only access to connections and public models.
* Can access Catalog.

 Job runner

Job runner is a specialized permission set for users who need access to run jobs and view the outcomes.

Notable features:

* Job runner is a project-level set.
* Can run jobs.
* Has read-only access to jobs, including status and results.
* No other access to dbt features.

 Job viewer

Job viewer enables users to monitor and review job executions within dbt. Users with this role can see jobs’ status, logs, and outcomes but cannot initiate or modify them.

Notable features:

* Job viewer is a project-level set.
* Read-only access to job results, status, and logs.
* No other access to dbt features.
* Can access Catalog.

 Manage marketplace apps

Manage marketplace apps is a specialized permission set associated with dbt marketplace apps. Usually implemented for the Snowflake Native App.

Notable features:

* Manage marketplace apps is an account-level set.
* Used exclusively for marketplace app integrations.
* Not intended for general user/group assignment.

 Metadata (Discovery API only)

Metadata is intended to be a read-only [Discovery API](https://docs.getdbt.com/docs/dbt-cloud-apis/discovery-api.md) integration permission set.

Notable features:

* Metadata is a project-level set.
* Grants read-only access to metadata related to dbt models, runs, sources, and tests.
* No access to modify, execute, or manage dbt jobs, repositories, or users.
* No other access to dbt features.

 Project creator

The Project creator permission set can create, configure, and set up new projects. It is recommended for the admin of teams that will own a project.

Notable features:

* Project creator is an account-level set.
* Only permission set other than Account admin that can create projects.
* Limited account settings access. The project creator can create and edit connections, invite users, create groups, and assign licenses.
* Unrestricted access to project configurations.
* Can access Catalog

 Security admin

Security admins have limited access to the security settings and policies for the dbt account. This is intended for members of a security team who need to ensure compliance with security standards and oversee the implementation of best security practices across the account. The [IT license-type](https://docs.getdbt.com/docs/cloud/manage-access/seats-and-users.md#licenses) includes this permission set by default.

Notable features:

* Security admin is an account-level set.
* Can create and edit users and groups and assign licenses.
* Can create and edit authentication and SSO settings.
* Can create and edit IP restrictions and service tokens, as well as manage user access controls.
* No access to jobs, runs, environments, or the IDE.

 Semantic Layer

A specialized permission set with strict access to only query the Semantic Layer using a service token.

Notable features:

* Semantic Layer is a project-level set.
* Can only query Semantic Layer.
* No other access to dbt features.

 Stakeholder and Read-Only

The Stakeholder and Read-Only are identical permission sets that are similar to Viewer, but without access to sensitive content such as account settings, billing information, or audit logs. Useful for personas who need to monitor projects and their configurations.

Notable features:

* Stakeholder is a project-level set.
* Read-only access to projects, environments, jobs, and runs.
* Read-only access to user and group information.
* Can access Catalog.
* No access to the IDE.
* Limited access to audit log content that excludes sensitive information, such as user settings and account-level changes.

 Team admin

Team admin is an administrative permission set intended for team leaders or similar personas. The permissions grants the ability to manage projects for the team.

Notable features:

* Team admin is a project-level set.
* Access to manage the project(s) for a team of users. Limited scope and access can be extended via environment permissions.
* Read-only access to many account settings (excluding sensitive content like billing and auth providers).
* Can access Catalog.

 Viewer

The Account Viewer permissions set provides read-only access to the dbt account. Useful for any persona who needs insights into your dbt account without access to create or change configurations.

The Viewer permission set is frequently paired with the [Read-only license-type](https://docs.getdbt.com/docs/cloud/manage-access/seats-and-users.md).

Notable features:

* Viewer is an account-level set.
* Read-only access to all settings, projects, environments, and runs.
* Read-only access to audit logs, including sensitive account-level information.
* No access to the IDE.
* Can access Catalog

<!-- -->

License types override group permissions

**User license types always override their assigned group permission sets.** For example, a user with a Read-Only license cannot perform administrative actions, even if they belong to an Account Admin group.

This ensures that license restrictions are always enforced, regardless of group membership.

Permissions:

* **Account-level permissions** — Permissions related to the management of the dbt account. For example, billing and account settings.
* **Project-level permissions** — Permissions related to the projects in dbt. For example, repos and access to the Studio IDE or dbt CLI.

note

Some permissions sets have read-only access to environment settings that can be overriden with more privileged access if the user is assigned to a group with [Environment write access](https://docs.getdbt.com/docs/cloud/manage-access/about-user-access.md#environment-write-access) configured.

### Account permissions[​](#account-permissions "Direct link to Account permissions")

Account permission sets enable you to manage the dbt account and manage the account settings (for example, generating service tokens, inviting users, and configuring SSO). They also provide project-level permissions. The **Account Admin** permission set is the highest level of access you can assign.

Key:

* **(W)rite** — Create new or modify existing. Includes `send`, `create`, `delete`, `allocate`, `modify`, and `develop`.
* **(R)ead** — Can view but cannot create or change any fields.

#### Account access for account permissions[​](#account-access-for-account-permissions "Direct link to Account access for account permissions")

|   |
| - |

| Account-level permission | Account Admin | Billing admin | Manage marketplace apps | Project creator | Security admin | Viewer |
| ------------------------ | ------------- | ------------- | ----------------------- | --------------- | -------------- | ------ |
| Account settings\*       | W             | -             | -                       | R               | R              | R      |
| Audit logs               | R             | -             | -                       | -               | R              | R      |
| Auth provider            | W             | -             | -                       | -               | W              | R      |
| Billing                  | W             | W             | -                       | -               | -              | R      |
| Connections              | W             | -             | -                       | W               | -              | -      |
| Groups                   | W             | -             | -                       | R               | W              | R      |
| Invitations              | W             | -             | -                       | W               | W              | R      |
| IP restrictions          | W             | -             | -                       | -               | W              | R      |
| Licenses                 | W             | -             | -                       | W               | W              | R      |
| Marketplace app          | -             | -             | W                       | -               | -              | -      |
| Members                  | W             | -             | -                       | W               | W              | R      |
| Project (create)         | W             | -             | -                       | W               | -              | -      |
| Public models            | R             | R             | -                       | R               | R              | R      |
| Service tokens           | W             | -             | -                       | -               | R              | R      |
| Webhooks                 | W             | -             | -                       | -               | -              | -      |

Search table...

|                  |   |   |   |   |
| ---------------- | - | - | - | - |
| Loading table... |   |   |   |   |

Search table...

|                  |   |   |   |   |
| ---------------- | - | - | - | - |
| Loading table... |   |   |   |   |
