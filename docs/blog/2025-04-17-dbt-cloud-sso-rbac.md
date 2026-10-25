# Establishing dbt Cloud: Securing your account through SSO & RBAC

April 17, 2025 · 9 min read

[![Brian Jan](/img/blog/authors/brian-jan.jpeg)](./authors/brian-jan.md)

[Brian Jan](./authors/brian-jan.md)

Lead Cloud Onboarding Architect

[](https://www.linkedin.com/in/brian-jan-3990a723/ "LinkedIn")

As a dbt Cloud admin, you’ve just upgraded to dbt Cloud on the [Enterprise plan](https://www.getdbt.com/pricing) - **congrats**! dbt Cloud has a lot to offer such as [CI/CD](../docs/deploy/about-ci.md), [Orchestration](../docs/deploy/deployments.md), [dbt Explorer](../docs/explore/explore-projects.md), [dbt Semantic Layer](../docs/use-dbt-semantic-layer/dbt-sl.md), [dbt Mesh](../best-practices/how-we-mesh/mesh-1-intro.md), [Visual Editor](../docs/platform/canvas.md), [dbt Copilot](../docs/platform/wizard-platform.md), and so much more. ***But where should you begin?***

We strongly recommend as you start adopting dbt Cloud functionality to make it a priority to set up Single-Sign On (SSO) and Role-Based Access Control (RBAC). This foundational step enables your organization to keep your data pipelines secure, onboard users into dbt Cloud with ease, and optimize cost savings for the long term.

## Authentication vs. Authorization

Before we dig into SSO, RBAC, and more — let’s go over how they map into two foundational security concepts.

* **Authentication:** [SSO](./2025-04-17-dbt-cloud-sso-rbac.md#single-sign-on-sso) is configured to gate authentication - it verifies (via an IdP) that users are who they say they are and can log into the specified dbt Cloud account.
* **Authorization:** [RBAC](./2025-04-17-dbt-cloud-sso-rbac.md#role-based-access-control-via-idp) is an authorization model - it controls what users can see and do within dbt Cloud based on their assigned licenses, groups, and permission sets.

## Single-Sign On (SSO)

Your SSO configuration steps will depend on your IdP, so we encourage you to start at our [SSO Overview](../docs/platform/manage-access/sso-overview.md) page and find the IdP-specific doc under that section that’s specific to your setup.

Regardless of what IdP you use, one of the first things you should do as a dbt Cloud admin is set the **login slug** value. This should be a *unique company identifier*.

Keep in mind that whatever you set, the slug will be appended to the end of the SSO login URL that your users will use to sign into dbt Cloud. For example:

* If I set my login slug to `mynewco`
* My SSO login URL will look something like `https://cloud.getdbt.com/enterprise-login/mynewco`.

At first glance, this screen has a lot of info and fields — but with the [SSO docs](../docs/platform/manage-access/sso-overview.md) in hand, dbt Cloud admins are ready to start setting up smooth, scalable workflows.

[![dbt Cloud's SSO configuration page](/img/blog/2025-04-10-sso-and-rbac/1_sso_config.png?v=2 "dbt Cloud's SSO configuration page")](#)dbt Cloud's SSO configuration page

Let’s break this down at a high level to make it more digestible:

1. After setting the desired login slug, a *dbt Cloud admin* will go to the dbt Cloud SSO configuration page and copy/paste everything under the **Identity provider values** section and will share the values with the *IdP admin*.

2. The *IdP admin* will create a [dbt Cloud app](../docs/platform/manage-access/set-up-sso-saml-2.0.md#creating-the-application) and then provide the values under the **dbt configuration** section to the *dbt Cloud admin*.

   tip

   Refer to the appropriate setup docs for [Google Workspace](../docs/platform/manage-access/set-up-sso-google-workspace.md), [Okta](../docs/platform/manage-access/set-up-sso-okta.md), [Microsoft Entra ID](../docs/platform/manage-access/set-up-sso-microsoft-entra-id.md), or [SAML 2.0](../docs/platform/manage-access/set-up-sso-saml-2.0.md).

3. The *dbt Cloud admin* will fill in those values into the SSO configuration page under the **dbt configuration** section and click **Save** to complete the process.

After completing this process:

* We *strongly* advise you validate the SSO flow is working by pasting the SSO login URL (it should look like `https://cloud.getdbt.com/enterprise-login/dbtlabs`) into your web browser’s private window
* And try to log into your account via the IdP.
* If the SSO flow isn’t working as expected, an account admin will still be able to log in with a password to correct the configuration.

tip

Be aware of our [SSO enforcement policy](https://docs.getdbt.com/docs/platform/manage-access/sso-overview#sso-enforcement) — once SSO is configured, all non-admin users will have to log in via SSO as a security best practice, while account admins, by default, can still authenticate with a password in lieu of [multi-factor authentication (MFA)](../docs/platform/manage-access/mfa.md).

Once you've set up SSO successfully, you have additional ways to onboard your users into dbt Cloud on top of sending out an email invite:

* Provide users the SSO login URL to access dbt Cloud. This is also known as the *SP-initiated flow* (SP stands for Service Provider; in this case, it would be dbt Cloud).
* Provision the dbt Cloud for users to access on their IdP’s dashboard. This is also known as the *IdP-initiated flow*.

[![SSO flows into dbt Cloud](/img/blog/2025-04-10-sso-and-rbac/2_sso_flows.png?v=2 "SSO flows into dbt Cloud")](#)SSO flows into dbt Cloud

Get stuck setting up SSO? [Open a support ticket](mailto:support@getdbt.com), and one of our Customer Solutions Engineers will be happy to help you!

## Licenses and Groups

In dbt Cloud, there are two main levers to control user access:

* [Licenses](./2025-04-17-dbt-cloud-sso-rbac.md#licenses)
* [Groups](./2025-04-17-dbt-cloud-sso-rbac.md#groups)

As a prerequisite, these all should be set *before* configuring RBAC. Let’s get into these!

### Licenses

There are three [license types](../docs/platform/manage-access/seats-and-users.md) in dbt Cloud:

* **Developer:** User can be granted *any* permissions.
* **Read-Only:** User has read-only permissions applied to all dbt Cloud resources regardless of the role-based permissions that the user is assigned.
* **IT:** User has Security Admin and Billing Admin permissions applied, regardless of the group permissions assigned.

Odds are that the majority of your users will be developers or analysts who’ll need Developer licenses. You can assign default licenses to users based on the groups that they’re in on the IdP side under **Account Settings** --> **Groups & Licenses** --> **License mappings**.

[![An example license mapping](/img/blog/2025-04-10-sso-and-rbac/3_license_mapping_example.png?v=2 "An example license mapping")](#)An example license mapping

If a user is in multiple groups with different license types assigned, they will be granted the highest license type — Developer.

### Groups

Groups are used to manage permissions. They define what a user can see and do across projects and environments. We recommend reviewing our [available permissions sets](https://docs.getdbt.com/docs/platform/manage-access/enterprise-permissions) and determining which are applicable to your dbt Cloud user base.

Keep in mind group permissions are additive in nature for users that belong to more than one group — meaning if a user belongs to multiple groups, they'll inherit all assigned permissions.

Navigating to Groups & Licenses page in dbt Cloud, you’ll see three default groups — Everyone, Member, and Owner. There’s also an option to create your own groups on the top right.

[![The out-of-the-box dbt Cloud groups you may use](/img/blog/2025-04-10-sso-and-rbac/4_default_dbt_cloud_groups.png?v=2 "The out-of-the-box dbt Cloud groups you may use")](#)The out-of-the-box dbt Cloud groups you may use

Here’s a brief primer on the default groups:

* **Owner:** This group is for individuals responsible for the entire account and will give them elevated account admin privileges. You cannot change the permissions.
* **Member:** This group is for the general members of your organization, who will also have full developer access to the account. You cannot change the permissions. By default, dbt Cloud adds new users to this group.
* **Everyone:** A general group for all members of your organization. Customize the permissions to fit your organizational needs. By default, dbt Cloud adds new users to this group and only grants user access to their personal profile.

While we recommend creating your own groups and deleting the defaults to better tailor it to your business’ needs, you should only delete the defaults *after* your own groups have been created and permission sets have been associated with them. These default groups are available to you as a means of getting users started in dbt Cloud. To sum up what they do, the Owner group will give users full account admin access while Everyone and Member groups will give users full developer access.

To help get you started, these are the main permission sets that should be assigned to most users:

| **User persona** | **Permission set** |
| ---------------- | ------------------ |
| dbt Cloud Admin  | Account Admin      |
| dbt Developer    | Developer          |
| dbt Analyst      | Analyst            |

You can also use groups to control which projects and environments users can access.

[![Creating a new dbt Cloud group](/img/blog/2025-04-10-sso-and-rbac/5_new_dbt_cloud_group.png?v=2 "Creating a new dbt Cloud group")](#)Creating a new dbt Cloud group

## Role-Based Access Control via IdP

If you made it this far, thanks for staying with me here! We’re now ready to configure RBAC, which assign users to the right groups and effectively the right permission sets after they authenticate into dbt Cloud. This hinges on the *SSO group mapping(s)* you’ll find within a group.

As an example, let’s say that I want specific users in this group where the SSO group mapping is `dbt-developer`. Note that you can also specify more than one.

[![Configuring a SSO group mapping within a group](/img/blog/2025-04-10-sso-and-rbac/6_sso_group_mapping.png?v=2 "Configuring a SSO group mapping within a group")](#)Configuring a SSO group mapping within a group

Here’s what we do to make it happen:

1. Have your IdP admin create a `dbt-developer` group in the IdP.
2. Assign users who should be in the dbt Cloud group to that IdP group.
3. Have users sign into dbt Cloud to confirm they get assigned to that group.

Easy enough, right? Just make sure these two conditions are checked for RBAC to work properly between your IdP and dbt Cloud:

* Group names must be an exact match
* Group names have the same casing

[![Making a SSO group mapping work with your identity provider](/img/blog/2025-04-10-sso-and-rbac/7_okta_sso_group_mapping_example.png?v=2 "Making a SSO group mapping work with your identity provider")](#)Making a SSO group mapping work with your identity provider

## Automate SSO & RBAC: Introducing SCIM

We have exciting news — [System for Cross-Domain Identity Management) (SCIM)](../docs/platform/manage-access/scim.md) support will be generally available in May 2025 (for SCIM-compliant IdPs & Okta)! If you’re unfamiliar with SCIM, you can think of it as automated user provisioning in dbt Cloud. It makes user data more secure and simplifies the admin and user experience by automating the user identity and group lifecycle.

Here’s why you should care about SCIM as a dbt Cloud admin:

1. **Improved Admin and end user experience** — Through automating user onboarding and offboarding, SCIM saves time for dbt Cloud admins that are managing multiple users on a weekly basis. If a user is added or removed in the IdP, their license and user account is automatically added/removed from dbt Cloud.
2. **Simplified RBAC with group management** — Admins can simplify access control management by using SCIM to update group membership. Currently, SSO group mapping enables admins to add new users to groups when they are JIT provisioned. SCIM would build on that functionality to allow group management not only for new users but also for existing users.

## Closing thoughts

Securing your account through SSO and RBAC should be one of your first priorities after getting on the Enterprise plan.

Not only does it keep your data safe, it allows you to onboard users into your account at scale. While it may be just the beginning of your dbt Cloud journey, putting in the work to check off this crucial step will establish that users are leveraging dbt responsibly at an enterprise grade level!

**Tags:**

* [dbt tutorials](./tags/dbt-tutorials.md)
