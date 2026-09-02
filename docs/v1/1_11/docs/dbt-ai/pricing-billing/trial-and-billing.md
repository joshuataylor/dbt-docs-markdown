# Trial and billing

Start a dbt Wizard trial, set a spend limit, and manage paid access for both the dbt platform and local CLI.

This page covers how to get dbt Wizard access and pay for it. For the models dbt Wizard can use and how tokens and credits work, refer to [Models and pricing](./overview.md).

Billing and spend controls are shared across the dbt platform and local CLI. Both surfaces draw from the same account-level balance.

## Prerequisites

* Account admin or billing admin permissions to start a trial or change a spend limit.
* A dbt account to manage usage, billing, spend limits, and more.
  * If you don't have one, you can create one during setup. No paid dbt platform plan required. This is generally useful for users on self-hosted dbt running the CLI.
* A business email address. Personal domains such as Gmail aren't eligible for a trial.

## What you get by plan

Every new account gets free dbt Wizard usage credits to start. What you get, and how you keep going, depends on your plan.

| Plan                                                                                                                       | What you get                                        | How it renews             | When it runs out                                                                                                  |
| -------------------------------------------------------------------------------------------------------------------------- | --------------------------------------------------- | ------------------------- | ----------------------------------------------------------------------------------------------------------------- |
| Developer and Starter, or self-hosted dbt with a free dbt account                                                          | 30-day trial with $100 in usage credits per account | One-time                  | Add a credit card and set a monthly spend limit or contact your account team.                                     |
| Enterprise, including [legacy Enterprise](../../platform/billing/plans-and-billing.md#legacy-plans) | $100/month in usage credits per account             | Resets each billing month | Usage continues uninterrupted. [Contact your account team](https://www.getdbt.com/contact) to add committed spend |
| Enterprise+                                                                                                                | $200/month in usage credits per account             | Resets each billing month | Usage continues uninterrupted. [Contact your account team](https://www.getdbt.com/contact) to add committed spend |
| [Legacy Team](../../platform/billing/plans-and-billing.md#legacy-plans)                             | No access to dbt Wizard                             | —                         | Move to a [Starter, Enterprise, or Enterprise+ plan](https://www.getdbt.com/pricing)                              |

* On Developer and Starter plans, the trial is opt-in, so it won't start automatically. It ends when you use up the $100 in credits or after 30 days, whichever comes first, and unused credits don't carry over.
* Legacy Enterprise plans get the same dbt Wizard access and monthly usage credits as current Enterprise. Only legacy Team has no access.
* Enterprise and Enterprise+ usage credits are granted automatically — there's no trial to start and no credit card required. Credits don't roll over or get prorated, and if you downgrade out of Enterprise or Enterprise+, any unused credits are removed at the plan change.
* Depleting your Enterprise or Enterprise+ monthly credits doesn't pause dbt Wizard. Usage keeps going, and you'll be prompted to connect with your account rep about adding a committed spend amount. Any optional monthly spend limit you've set still pauses usage once reached.

Running dbt Wizard from the CLI against a self-hosted dbt project?

Run `dbt login` (or `wizard login`) to get the same 30-day trial. The command creates your free dbt account and provisions the trial together, and that account is where you manage usage and spend limits.

Everything on this page describes dbt managed billing — usage that dbt Labs bills through your dbt account. If you bring your own key, your AI provider bills you directly and none of this applies. Refer to [BYOK for dbt platform](../../platform/wizard-byok-platform.md) or [BYOK for the CLI](../wizard-byok.md) instead.

## Start your trial

Start your trial from anywhere in the dbt platform or from the dbt Wizard CLI. Note, Enterprise-tiered plans automatically have a [spend limit set](../wizard-billing-faqs.md#what-does-the-dbt-wizard-enterprise-plan-monthly-usage-credits-include)

### dbt platform

Start from **Billing & Usage**, or from the dbt Wizard prompt in Studio IDE or the home tab as they all take you to the same flow.

1. Click your account name, then select **Account settings**. (Or click on any button that says "Start trial" to start your Wizard trial.)
2. Under **Settings**, click **Billing & Usage**.
3. On the **Overview** tab, find the **dbt Wizard** card and click **Start trial**.

No credit card is required. Your 30-day trial with $100 in usage credits starts right away, and you can track how much you've used anytime in **Billing & Usage**.

![The Billing & Usage Overview page, showing dbt State and dbt Wizard cards with Start trial buttons, plus a usage-by-month chart](/img/docs/dbt-platform/wizard-billing-overview.png?v=2 "The Billing & Usage Overview page, showing dbt State and dbt Wizard cards with Start trial buttons, plus a usage-by-month chart")The Billing & Usage Overview page, showing dbt State and dbt Wizard cards with Start trial buttons, plus a usage-by-month chart

### Wizard CLI

There's no **Start trial** button in the CLI. Logging in is what starts your trial — one command creates your free dbt account, if you don't have one, and provisions the 30-day trial with $100 in usage credits at the same time.

1. [Install dbt Wizard](../wizard-quickstart.md).

2. Run `dbt login` and complete the browser sign-in, or create a new account to manage your dbt Wizard spend limits:

   ```shell
   dbt login
   ```

3. Run `wizard` in your project and choose **dbt-managed** when onboarding asks how AI usage is billed. You don't need an AI provider key.

You don't need a paid dbt platform plan, so this path is the same whether you're on a dbt platform plan or running against a self-hosted dbt project. Refer to [Use dbt Wizard locally](../wizard-quickstart.md) for the full onboarding walkthrough.

## Set up paid usage

What you do next depends on your plan.

### Developer, Starter, or self hosted

1. Go to **Billing & Usage > Usage-based features**.
2. Click **Set up billing**.
3. Add a credit card and fill in your payment details. Click **Save card**.
4. Complete your setup to choose a preset monthly spend limit, or set your own with the **Custom** option.
5. Optionally, turn on automatic increases for when you're close to your limit.
6. Click **Continue**.
7. Review and confirm your spend limit. Click **Activate dbt Wizard** to start your billing. You won't be charged today and will be billed monthly for actual usage, up to the limit you set. Usage then pauses if you reach that limit.

![The Add billing page, showing a credit card form and a complete billing setup flow](/img/docs/dbt-platform/wizard-add-billing.png?v=2 "The Add billing page, showing a credit card form and a complete billing setup flow")The Add billing page, showing a credit card form and a complete billing setup flow

![The Set your dbt Wizard spend limit page, showing pre-set monthly options, a Custom option, and an auto-raise toggle](/img/docs/dbt-platform/wizard-manage-spend.png?v=2 "The Set your dbt Wizard spend limit page, showing pre-set monthly options, a Custom option, and an auto-raise toggle")The Set your dbt Wizard spend limit page, showing pre-set monthly options, a Custom option, and an auto-raise toggle

![The Activate dbt Wizard page where you can review and confirm your spend limit and a button to activate dbt Wizard](/img/docs/dbt-platform/wizard-activate.png?v=2 "The Activate dbt Wizard page where you can review and confirm your spend limit and a button to activate dbt Wizard")The Activate dbt Wizard page where you can review and confirm your spend limit and a button to activate dbt Wizard

### Enterprise and Enterprise+ plans

There's no trial to start and no self-serve credit card flow. Your monthly usage credits are granted automatically — [contact your account team](https://www.getdbt.com/contact) to set up or adjust committed spend.

Using up your included monthly credits doesn't cut you off. dbt Wizard usage continues without interruption, and if you don't have a committed spend amount you'll be prompted to connect with your account rep about adding one. If you've set the optional monthly dbt Wizard spend limit, that limit still applies and pauses usage once reached.

## Manage your spend limit

Your spend limit caps how much dbt managed dbt Wizard usage your account can consume in a billing period, across both the dbt platform and local development.

* You only pay for actual usage, up to the limit you choose. The limit is a cap, not a prepaid charge.
* If you reach your limit, dbt Wizard usage pauses until you raise it or the next billing cycle starts. This applies on every plan, including Enterprise-tiered plans that set the optional limit. Enterprise-tiered accounts that don't have a committed spend amount will be prompted to connect with their account rep about adding one.
* Limits are set separately for dbt Wizard and [dbt State](../../deploy/dbt-state-about.md), but both draw from your account's overall usage-based spend.

To view or update your limit, go to **Billing & Usage > Usage-based features > Wizard**. Enterprise-tiered plans can [contact their account team](https://www.getdbt.com/contact) to adjust their limit.

![The Manage your dbt Wizard spend limit page, showing pre-set monthly options and a Custom option.](/img/docs/dbt-platform/wizard-manage-spend.png?v=2 "The Manage your dbt Wizard spend limit page, showing pre-set monthly options and a Custom option.")The Manage your dbt Wizard spend limit page, showing pre-set monthly options and a Custom option.

## View your usage and costs

To see what you've spent, go to **Account settings > Billing & Usage > Usage-based features** and open the **Wizard** tab. From there you can check:

* How much of your included monthly usage you've used, what's left, and when it resets.
* Your current dbt Wizard spend limit, with an **Edit** button to change it.
* **dbt Wizard usage by model**, which breaks down your usage (in UTC) so you can see which models are driving your costs.

## How usage is measured

dbt Wizard usage is measured in tokens, then converted into dollar-based usage based on the model and the token type, such as input, cached read, cache write, or output. Refer to [Key terms](./overview.md#key-terms) for what a token is, and the [Model Provider Rate Table](https://www.getdbt.com/legal/dbt-wizard-token-costs-by-model) for current rates.

## Related docs

* [dbt Wizard billing FAQs](../wizard-billing-faqs.md) for common billing questions
* [Models and pricing](./overview.md) for model options and token pricing
* [BYOK for dbt platform](../../platform/wizard-byok-platform.md) or [BYOK for the CLI](../wizard-byok.md) for bring-your-own-key setup
* [Billing](../../platform/billing.md) for general dbt platform billing
