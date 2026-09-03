# Invite read-only users to dbt Wizard [Preview](https://docs.getdbt.com/docs/dbt-versions/product-lifecycles "Go to https://docs.getdbt.com/docs/dbt-versions/product-lifecycles")

dbt platform | Usage-based

Give the people who need answers a way to get them. With a few setup steps, read-only users can ask questions of governed production data in plain language — no developer license required.

[Read-only users](./manage-access/seats-and-users.md) work in the dbt Wizard home tab in [Explore mode](./wizard-home.md#ask-questions-in-explore-mode): the agent queries and explains your data but can't edit files, run builds, or change your project.

This page is for admins. It covers which license read-only users need, how to invite them, and how to set your project up for good answers. Users with development access don't need a read-only license — they get [Explore mode](../dbt-ai/wizard-ide.md#agent-modes) alongside the authoring modes automatically.

info

Read-only users see dbt Wizard in the home tab as soon as they have access, but they can't query anything until two things are true: credentials exist (either their own or the project's [analytics credential](#set-up-analytics-credentials)), and the project has metadata or Semantic Layer definitions to ask questions against.

## What read-only users can do

A read-only user in dbt Wizard can:

* Ask questions of production data in plain language in the [dbt Wizard home tab](./wizard-home.md), and ask follow-ups in the same conversation.
* See the Semantic Layer metric definition or SQL behind every answer.
* See which environment an answer was queried against.

A read-only user can't:

* Use dbt Wizard in the [Studio IDE](../dbt-ai/wizard-ide.md). Read-only users work in the [dbt Wizard home tab](./wizard-home.md) only.
* Edit files, commit, or open pull requests.
* Run dbt commands or trigger builds.
* Switch to **Ask for approval** or **Edit files automatically** mode. Those options are disabled.
* Access data their [analytics credentials permissions](#set-up-analytics-credentials) don't already allow.

Read-only users only have access to Explore mode and see a simpler UI. The only tools available are analytics tools: project metadata and Semantic Layer definitions. Tools that need filesystem or development access, like the context panel and the branch picker, don't appear. Users with development access still see all three [agent modes](../dbt-ai/wizard-ide.md#agent-modes) and choose between them.

## FAQs

 Which license does a read-only user need?

Read-only. For current license types and how they're assigned, refer to [User licenses](./manage-access/about-user-access.md).

 How many read-only users can we invite?

It depends on your dbt platform plan. For license availability by plan, refer to [dbt pricing](https://www.getdbt.com/pricing).

 Does read-only usage count against our AI usage credits?

Yes. Questions read-only users ask in Explore mode use dbt-managed inference and draw from your account's dbt Wizard consumption pool, the same as any other dbt Wizard usage. Refer to [dbt Wizard billing and access FAQs](../dbt-ai/wizard-billing-faqs.md) for more information.

 Do read-only users need the Analyst read permission set?

Not to use Explore mode. Anyone without development access who can reach dbt Wizard sees Explore mode, whichever read permission set they have.

They do need [Analyst read](./manage-access/enterprise-permissions.md#analyst-read) to set up their own warehouse credentials. Without it, they query through the project's [analytics credential](#set-up-analytics-credentials).

 Can read-only users use their own warehouse credentials?

Yes, if they have the [Analyst read](./manage-access/enterprise-permissions.md#analyst-read) permission set and dbt Wizard uses those instead of the analytics credential.

 Which credentials does Explore mode query with?

The user's personal credentials first, if they have any. The project's [analytics credential](#set-up-analytics-credentials) is the fallback. If neither exists, dbt Wizard asks the user to set up credentials or contact an admin.

 A read-only user can see dbt Wizard but can't get answers. Why?

Seeing dbt Wizard in the Home tab doesn't mean the account is ready to answer questions. Check that:

* An admin has [set up an analytics credential](#set-up-analytics-credentials) on the connection, and assigned that connection to the project.
* The project has metadata or Semantic Layer definitions to ask questions against. An empty project has nothing to answer from.
* The credential can reach the data the user is asking about.

## Invite read-only users

Read-only users need AI features turned on to use Wizard. AI features are [enabled](./manage-dbt-ai.md) by default, so there's usually nothing to change.

1. In dbt platform, go to **Account settings** --> **Users**.
2. Click **Invite users**.
3. Enter the email addresses of the people you want to invite.
4. Assign the [read-only license](./manage-access/seats-and-users.md#licenses) and a group whose [permission set](./manage-access/enterprise-permissions.md) matches the data they should see. Explore mode isn't limited to one permission set — anyone without development access who can reach dbt Wizard gets it.
   * Use the [Analyst read](./manage-access/enterprise-permissions.md#analyst-read) permission set if you want these users to configure their own warehouse credentials rather than querying through the analytics credential.
5. Click **Send invitations**.

Invited users log in and should land on the account home, then open dbt Wizard home tab from the left sidebar to start asking questions in Explore mode.

These are the read-only specifics. For the full invite flow, seat management, and resending invites, refer to [Invite users](./manage-access/invite-users.md).

## Set up analytics credentials

To run a query, Explore mode needs warehouse credentials. Most read-only users don't have their own, so an admin sets up an **analytics credential**: a shared, project-level warehouse credential that read-only users fall back to when they haven't configured personal credentials. Read-only users that have their own credentials can use the [Analyst read](./manage-access/enterprise-permissions.md#analyst-read) permission set.

At query time, dbt Wizard resolves credentials in this order:

1. The user's personal warehouse credentials if they have them.
2. The project's analytics credential, if an admin has [set one up](./wizard-read-only-users.md#add-an-analytics-credential-to-a-connection).
3. If neither exists, dbt Wizard returns an error asking the user to set up personal credentials or contact an admin.

### Prerequisites

* An [account admin](./manage-access/about-user-access.md) role.
* A [connection](./connect-data-platform/about-connections.md) to one of the supported warehouses: Snowflake, BigQuery, Redshift, or Databricks.

### Add an analytics credential to a connection

1. In dbt platform, go to **Account settings** --> **Connections** and select the connection.
2. Scroll to the **Analytics credential** card and click **Edit**. A connection can have one analytics credential.
3. Pick an **Auth method** and enter the warehouse credentials dbt Wizard should query with, then click **Save**.

The fields you see depend on the connection's warehouse and the auth method you pick. The following example uses Snowflake with key pair authentication:

1. Enter the **Username** and **Private key** for the warehouse user dbt Wizard queries as.
2. Add the **Private key passphrase** if your key is encrypted.
3. Optionally add a **Role** and **Warehouse** to pin which ones queries run with.

![Analytics credentials with Key pair selected as the auth method](/img/docs/dbt-platform/analytics-credential-keypair.png?v=2 "Analytics credentials with Key pair selected as the auth method")Analytics credentials with Key pair selected as the auth method

Give the credential the *least privilege* it needs to answer questions — read-only access to the data read-only users should see, and nothing more. It's a shared credential, so anyone querying through it sees whatever it can see, without a per-user trail. If you need per-user access control or auditing at the warehouse, have users configure personal credentials instead — that requires the [Analyst read](./manage-access/enterprise-permissions.md#analyst-read) permission set.

### Assign the connection to a project

1. Go to **Account settings** --> **Projects** and select the project.
2. In the **Analytics configuration** section, select a connection that has an analytics credential. Only connections with one appear in the picker.
3. Click **Save**.

Read-only users on that project now query through this credential in Explore mode.

You can reuse the same connection across projects, but each project uses one analytics connection at a time.

## Control what read-only users can see

Explore mode respects your existing access controls. It doesn't create a new permission surface.

* Scope the [analytics credential](#set-up-analytics-credentials) to only the data read-only users should reach, since they all query through it.
* Use [permission sets and groups](./manage-access/enterprise-permissions.md) to scope which projects a read-only user can reach.
* Use [environment-level permissions](./manage-access/environment-permissions.md) to control which environments they can query.

## Set your team up for good answers

Explore mode answers are grounded in what your project defines. Here are some things to make the experience easier for users:

* Define the metrics people ask about in the [Semantic Layer](../build/about-metricflow.md). Questions that map to a governed metric return your approved numbers instead of ad-hoc SQL.
* Document your models and columns by writing up descriptions, which help dbt Wizard pick the right table and explain the answer in business terms.
* Tell users which environment to trust by pointing them at production and explain what development data is, so a work-in-progress number never lands in a business review.

## Related docs

* [Agent modes in dbt Wizard](../dbt-ai/wizard-ide.md#agent-modes)
* [dbt Wizard home tab](./wizard-home.md)
* [User licenses](./manage-access/about-user-access.md)
* [Invite users](./manage-access/invite-users.md)
* [Enable AI features in the dbt platform](./manage-dbt-ai.md)
