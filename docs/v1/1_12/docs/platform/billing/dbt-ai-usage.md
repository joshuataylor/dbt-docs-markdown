# dbt AI: Usage metering and limiting

dbt platform

dbt AI usage is metered differently depending on the feature you use:

What's changing on September 1, 2026

From September 1, 2026, a couple of things are changing for dbt AI features:

* **AI features are being enabled by default.** They're already on for new accounts and are rolling out soon to existing accounts. If your organization opted out, they'll remain off. Admins can turn AI features on or off anytime in **Account settings**.
* **dbt Wizard is moving to usage-based billing** for [dbt-managed AI](#dbt-managed-inference). Usage is metered per token against your consumption pool, and an admin can set a monthly spend limit in dbt platform.

Refer to [dbt Wizard billing and access FAQs](../../dbt-ai/wizard-billing-faqs.md) for more info.

| Feature                     | How it's metered                                   | What limits usage                                    |
| --------------------------- | -------------------------------------------------- | ---------------------------------------------------- |
| [dbt Wizard](#dbt-wizard)   | Dollar-based usage, converted from tokens          | Your consumption pool, then your monthly spend limit |
| [dbt Copilot](#dbt-copilot) | A count of completed AI requests, known as actions | A monthly action allotment set by your plan          |

Bring your own key (BYOK) usage isn't metered by dbt. Your AI provider bills you directly and the usage doesn't draw from either limit.

## dbt Wizard

dbt Wizard is metered by dollar-based usage rather than a count of actions. Usage is measured in tokens — prompts, project context, cached content, and generated responses all consume them — and converted into a dollar amount based on the model and token type. That amount is deducted from your consumption pool.

Usage from the dbt platform and the local CLI both draw from the same account-level pool.

### Usage credits by plan

All credits are account-level

Usage credits are granted per account, not per user or per seat. Everyone on your account draws from the same balance, whether they use dbt Wizard in the dbt platform or the local CLI.

| Plan                                      | What you get                                                       |
| ----------------------------------------- | ------------------------------------------------------------------ |
| Enterprise                                | $100/month in usage credits per account, resets each billing month |
| Enterprise+                               | $200/month in usage credits per account, resets each billing month |
| All other plans and self hosted dbt users | One-time 30-day trial with $100 in usage credits per account       |

Pools don't roll over. For eligibility, how to start a trial, and how to set up paid access afterward, refer to [Trial and billing](../../dbt-ai/pricing-billing/trial-and-billing.md).

### When you reach your limit

Your spend limit caps how much dbt managed dbt Wizard usage your account can consume in a billing period. You only pay for actual usage, up to the limit you choose.

* If you use up your consumption pool on a Developer, Starter, or self-hosted plan, usage pauses until you add paid usage or the next billing cycle starts.
* Enterprise and Enterprise+ accounts should add a committed spend amount to their contract to keep using dbt Wizard. You may lose access to dbt Wizard without this commit in place. If you've set an optional monthly dbt Wizard spend limit, that still applies and pauses usage once reached.
* Limits are set separately for dbt Wizard and [dbt State](../../deploy/dbt-state-about.md), but both draw from your account's overall usage-based spend.

### View Wizard usage

1. Navigate to [**Account settings**](../account-settings.md).
2. Select **Billing & Usage** under the Settings header.
3. On the **Overview** tab, check the **Consumption pool** card, or open **Usage-based features > Wizard** for usage and spend controls.

You need to be an account admin or billing admin to view or change spend limits.

For trials, consumption pools, spend limits, and BYOK billing, refer to [dbt Wizard billing and access FAQs](../../dbt-ai/wizard-billing-faqs.md).

## dbt Copilot

dbt Copilot usage is measured by the number of completed AI requests, known as dbt Copilot actions. A defined number of actions is allocated monthly based on your [subscription plan](https://www.getdbt.com/pricing). Once you reach the limit, dbt Copilot is temporarily disabled until the start of the next billing cycle.

### View Copilot actions

1. Navigate to [**Account settings**](../account-settings.md).
2. Select **Billing** under the Settings header.
3. On the billing page, click the **Copilot Actions** tab to view your usage.

![View usage in dbt Copilot](/img/docs/dbt-platform/view-usage-in-copilot.gif?v=2 "View usage in dbt Copilot")View usage in dbt Copilot

## FAQs

 Temporary dbt Copilot Actions bridge (ended September 1, 2026)

As a temporary compatibility bridge, dbt Wizard drew from your existing dbt Copilot included action allotment. That bridge ended on September 1, 2026, and dbt Wizard usage is now metered separately as dollar-based usage against your usage credits and consumption pool.

Users who bring their own key (BYOK) were never affected by this bridge.

 What counts as a dbt Copilot action

dbt Copilot actions refer to requests made to the dbt Copilot assistant through the dbt interface. These actions are recorded and displayed on the billing page alongside other usage metrics by accessing the **Copilot Actions** tab in the **Billing** page.

The following interactions count as dbt Copilot actions:

* **Each inline generation** — Every time dbt AI writes or suggests code in your file, it counts toward your usage limit.

* **Each generation of documentation, tests, semantic models, or metrics** — Any time you ask dbt AI to automatically create things like documentation, tests, data models, or metrics, it counts as one interaction.

* **Each generation within dbt AI chats on Canvas or Insights** — Any time you use dbt Copilot chat in Canvas or Insights to generate something, it counts as an interaction.

 Allowed limits on number of dbt Copilot actions per month per license

The following table outlines the limits of dbt Copilot actions by plan per month:

| Plan                  | Limit  |
| --------------------- | ------ |
| Developer             | ❌     |
| Starter\*             | 100    |
| Legacy Enterprise\*\* | 1,000  |
| Enterprise            | 5,000  |
| Enterprise+           | 10,000 |

\*Team plan customers who enrolled in dbt Copilot Beta prior to March 19, 2025 have access to dbt Copilot. All other legacy Team plan customers must move to the [Starter plan or above](https://www.getdbt.com/pricing) to get access.

\*\*Enterprise-tier customers on plans enrolled prior to May 1, 2025 (including legacy Business Critical) have a limit of 1,000 dbt Copilot actions per month. To get a higher allotment, move to the current [Enterprise or Enterprise+ plan](https://www.getdbt.com/pricing).

 Notifications when limits are reached

When usage limits are reached, a notification appears in the UI. Additionally, an email notification is sent to the designated recipient.

For users on the Starter plan, the account owner receives an email notification when the usage limit is reached.

For users enrolled on the Enterprise and Enterprise+ plans, both the billing administrator and the account administrator are notified by email when the usage limit is reached.

Once usage limits are reached, attempts to perform an action in dbt Copilot triggers a banner notification indicating that the limit has been exceeded.

Under Bring Your Own Key (BYOK), usage is not tracked by dbt AI and is subject to your OpenAI limits.

## Related docs

* [Trial and billing](../../dbt-ai/pricing-billing/trial-and-billing.md) to start a dbt Wizard trial and set a spend limit
* [dbt Wizard billing and access FAQs](../../dbt-ai/wizard-billing-faqs.md) for common billing questions
* [Models and pricing](../../dbt-ai/pricing-billing/overview.md) for model options and token pricing
* [Billing](../billing.md) for general dbt platform billing
