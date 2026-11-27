# How to upgrade a dbt account

dbt offers [several plans](https://www.getdbt.com/pricing/) with different features that meet your needs. This document is for dbt admins and explains how to select a plan in order to continue using dbt.

## Prerequisites

Before you begin:

* You *must* be part of the [Owner](../../docs/platform/manage-access/self-service-permissions.md) user group to make billing changes. Users not included in this group will not see these options.
* All amounts shown in dbt are in U.S. Dollars (USD)
* When your trial expires, your account's default plan enrollment will be a Starter plan.

## Select a plan

When your [14-day trial](https://www.getdbt.com/signup/) ends, you need to select a plan to continue using your account:

1. When you log in, a **Welcome back to dbt** message appears explaining that you've reached the end of your trial and need to select a plan to continue.
2. Click **Choose a plan**. This opens the **Plans & Pricing** page, where you can review the available dbt [plans](https://www.getdbt.com/pricing/) and their features.

If your account is past due after a failed payment, refer to [How can I update my billing information?](./change-billing.md) instead.

To select a plan, review the following guidance per plan type:

### Developer plan

1. On the **Plans & Pricing** page, click **Downgrade** under the Developer plan.
2. A confirmation message asks if you're sure you want to downgrade. It notes that you'll lose features such as API access, and that switching to the Developer plan removes read-only seats.
3. Click **Switch to Developer** to confirm.
4. After you confirm, your account continues on the Developer plan.

![Plans and Pricing page with Downgrade under the Developer plan](/img/docs/dbt-platform/developer-downgrade-plans-pricing.png?v=2 "Plans and Pricing page with Downgrade under the Developer plan")Plans and Pricing page with Downgrade under the Developer plan

![Confirmation to switch to the Developer plan](/img/docs/dbt-platform/developer-downgrade-confirm.png?v=2 "Confirmation to switch to the Developer plan")Confirmation to switch to the Developer plan

#### Plan allocation

If you select a plan but have too many seats or projects for that plan (for example, if you select the Developer plan but have more than one developer seat), you'll be directed to the users & projects pages to make edits.

### Starter plan

1. When your trial expires, your account's default plan enrollment will be a Starter plan.
2. To unlock your account and continue using the Starter plan, click on **Select plan** under the Starter column.
3. Enter your payment information and seat purchases. Then click **Complete purchase**.
4. This automatically unlocks your dbt account, and you can now enjoy the benefits of the Starter plan. 🎉

![Plans and Pricing page with Starter marked as Current trial](/img/docs/dbt-platform/starter-plans-pricing.png?v=2 "Plans and Pricing page with Starter marked as Current trial")Plans and Pricing page with Starter marked as Current trial

![](/img/docs/dbt-platform/trial-team-payments-flow.png?v=2)

### Enterprise-tier plans

1. On the **Plans & Pricing** page, find the **Enterprise** or **Enterprise+** plan.
2. Click **Contact sales**. This opens a form where you can [talk to a dbt expert](https://www.getdbt.com/contact/) and request a conversation with the Sales team.
3. Submit the form. The Sales team will follow up with more information.

![Click Contact sales on an Enterprise-tier plan to open the Talk to a dbt expert form](/img/docs/dbt-platform/enterprise-contact-sales.gif?v=2 "Click Contact sales on an Enterprise-tier plan to open the Talk to a dbt expert form")Click Contact sales on an Enterprise-tier plan to open the Talk to a dbt expert form

## Related questions

For commonly asked billings questions, refer to the dbt [pricing page](https://www.getdbt.com/pricing/).

How does billing work?

* Developer plans are free and include one developer seat and a monthly models-built allotment.
* Starter plans are billed monthly on the credit card used to sign up, based on [developer seat count and usage](../../docs/platform/billing.md). You'll also receive a monthly receipt at the billing email on file.
* Enterprise-tier plan customers are billed annually based on the number of developer seats, as well as any additional services and features in your chosen plan.

On Starter plans, account owners can view and manage payment details in **Account settings**:

1. Click your account name in the left sidebar and select **Account settings**.
2. Under **Settings**, select **Billing & Usage** (labeled **Billing** on some accounts).
3. Open **Overview** if it appears, then open the **Billing details** tab.

On Enterprise-tier plans, billing is managed by your account team. Contact them for billing questions.

Can I upgrade or downgrade my plan?

Yes, you can upgrade or downgrade at any time. Account Owners can access their dedicated billing section via the account settings page.

If you’re not sure which plan is right for you, get in touch and we’ll be happy to help you find one that fits your needs.

What happens when I upgrade from Developer to Starter?

The free two-week trial is only available to new accounts at signup — but if you're already on the Developer plan, you can upgrade to Starter and immediately access all Starter features without needing to go through the trial.

When you upgrade from Developer to Starter, you move directly to paid billing — you'll enter your payment information and select your developer license seats to complete the upgrade. Your existing projects and settings carry over.

Can I pay by invoice?

Currently, dbt Starter plan payments must be made with a credit card, and by default they will be billed monthly based on the number of [developer seats and usage](../../docs/platform/billing.md).

We don’t have any plans to do invoicing for Starter plan accounts in the near future, but we do currently support invoices for companies on the dbt Enterprise-tier plan. Feel free to [contact](https://www.getdbt.com/contact/) us to build your Enterprise pricing plan.

Why did I receive a **Failed payment** error email?

This means we were unable to charge the credit card you have on file, or you have not provided an updated card for payment. If you're a current account owner with a card on file, contact your credit card issuer to inquire as to why your card was declined or update the credit card on your account.

On Starter plans, your Account Owner can update payment details:

1. Click your account name in the left sidebar and select **Account settings**.
2. Under **Settings**, select **Billing & Usage** (labeled **Billing** on some accounts).
3. Open **Overview** if it appears, then open the **Billing details** tab.
4. Under **Payment information**, click **Edit** next to your card details, update your information, and save. We'll try again at the next billing run.

On Enterprise-tier plans, billing is managed by your account team. Contact them for help with payment issues.
