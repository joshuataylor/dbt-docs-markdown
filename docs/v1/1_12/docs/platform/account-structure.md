# How a dbt account is structured

dbt platform

This page shows how a dbt platform account is organized. Some things, like git and single sign-on (SSO), you set up once for everyone. Others, like environments, you set up inside each project.

Think of your account as an office building. The front door and security desk are shared by everyone. Each team gets its own floor with its own rooms.

![The account: what you set up once, and the projects that share it.](/img/docs/dbt-platform/account-structure-account.svg?v=2 "The account: what you set up once, and the projects that share it.")The account: what you set up once, and the projects that share it.

![Inside a project: the repository, connection profiles, and environments.](/img/docs/dbt-platform/account-structure-project.svg?v=2 "Inside a project: the repository, connection profiles, and environments.")Inside a project: the repository, connection profiles, and environments.

## What lives where

The following table shows where you configure each part of your account.

| Level   | What you set up here                                                                                                            | How many                                                  |
| ------- | ------------------------------------------------------------------------------------------------------------------------------- | --------------------------------------------------------- |
| Account | [Git integration](./account-integrations.md)                                                | One per git provider                                      |
| Account | [Single sign-on (SSO)](./manage-access/sso-overview.md)                                     | One per account                                           |
| Account | [Users, groups, and permissions](./manage-access/about-user-access.md)                      | Defined once, then scoped to projects                     |
| Account | [Security and networking](./secure/about-network-security.md)                               | Defined once                                              |
| Account | [Warehouse connections](./connect-data-platform/about-connections.md#connection-management) | As many as you need                                       |
| Project | Repository                                                                                                                      | One per project                                           |
| Project | [Connection profiles](./about-profiles.md)                                                  | As many as you need                                       |
| Project | [Environments](../dbt-platform-environments.md) and jobs                                              | One development environment, plus deployment environments |

## Common mix-ups

These are the things that trip people up most during setup.

* **You don't need a project per environment.** A project isn't "dev" or "prod". Each project can have a development, staging, or production environments. Create a new project when you have a separate codebase or team, not a separate stage. Refer to [types of environments](../dbt-platform-environments.md?version=2#types-of-environments) for more information.
* **Integrations are set up once per account.** You connect your SSO identity provider once at the account level, and you can’t set a different one for each project. With Git, you can connect each git provider application once for the account (for example, an account supports one [GitHub application](./git/connect-github.md)), and each project then chooses its own repo. Projects in the same account can use different providers, such as [GitHub](./git/connect-github.md#installing-dbt-in-your-github-account) for one and [GitLab](./git/connect-gitlab.md?version=1.12) for another, and more than one project can use the same repository.
* **Warehouse connections belong to the account, not the project.** Any project can reuse a connection. A connection only reaches a project when you attach it to one of that project's environments.
* **Connection profiles belong to the project.** A profile bundles one connection with deployment credentials and extended attributes. Profiles you create in one project aren't visible in others, and they don't apply to development environments.

## Environments in a project

Each project has the following environment types.

| Environment type | How many per project | What it's for                                                                 | Shows in Catalog |
| ---------------- | -------------------- | ----------------------------------------------------------------------------- | ---------------- |
| Development      | One at most          | Building in the Studio IDE or dbt platform CLI with your personal credentials | No               |
| General          | As many as you need  | Deployment work like QA, sandboxes, or ad hoc runs                            | No               |
| Staging          | One at most          | Testing with production tools before changes reach production                 | Yes              |
| Production       | One at most          | Scheduled and triggered jobs that build your source of truth                  | Yes              |

For more information, refer to [dbt environments](../dbt-platform-environments.md) and [Deployment environments](../deploy/deploy-environments.md).

## More than one data platform

One account can hold connections to more than one data platform. Each project picks the connection it needs for each environment. Your git, SSO, users, and security settings stay shared across the whole account.
