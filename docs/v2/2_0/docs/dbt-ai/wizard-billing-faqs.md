# dbt Wizard billing and AI access FAQs

Common questions about AI being turned on by default, how dbt Wizard usage is measured, what your usage credits covers, and how spend limits work.

What's changing from September 1, 2026

From September 1, 2026, a couple of things are changing for dbt AI features:

* **AI features are being enabled by default.** They're already on for new accounts and are rolling out soon to existing accounts. If your organization opted out, they'll remain off. Admins can turn AI features on or off anytime in **Account settings**.
* **dbt Wizard is moving to usage-based billing** for [dbt-managed AI](#dbt-managed-inference). Usage is metered per token against your consumption pool, and an admin can set a monthly spend limit in dbt platform.

## AI enabled by default

AI features are being enabled by default for dbt platform accounts. They're already on for new accounts and are rolling out soon to existing accounts. If your organization opted out, they'll remain off. Admins can turn AI features on or off anytime in **Account settings**. Turning AI on doesn't create a charge on its own — refer to [Billing FAQs](#billing-faqs) in the next section to understand how usage is metered.

 Which AI features are enabled by default?

The following surfaces are on by default:

* dbt Wizard in Studio IDE
* dbt Wizard home tab
* dbt Copilot in dbt platform (includes Canvas and Insights).
* Any future dbt AI features will automatically become available as well.

 AI features aren't on for my account yet. How do I turn them on?

AI features are already on for new accounts and are rolling out soon to existing accounts, so they may not be on for your account right away. An account admin can turn them on now in **Account settings** — refer to [Manage AI features in dbt platform](../platform/manage-dbt-ai.md). If your organization opted out, they'll remain off until an admin turns them on.

 I previously asked for AI to be permanently disabled. Will it turn on anyway?

No. If your organization already opted out of AI features contractually or had them permanently disabled, they stay off. You don't need to do anything before September 1, 2026.

 Can I opt out of AI features?

Yes. An account admin can turn AI off at any time in **Account settings**. Refer to [Manage AI features in dbt platform](../platform/manage-dbt-ai.md) for the steps — the same toggle controls both dbt Wizard and dbt Copilot.

 If AI is enabled by default, will I be charged automatically?

No. Enabling AI doesn't authorize paid usage by itself. dbt Wizard usage draws from your included consumption pool or trial pool, and dbt-managed dbt Wizard pauses once that pool is depleted. Going beyond it requires explicit purchase.

If you keep AI disabled, you incur no AI charges after September 1, 2026.

 Which AI features use consumption-based billing?

Only dbt Wizard with dbt-managed inference, across dbt Wizard in dbt platform and the dbt Wizard CLI. dbt Copilot stays on its existing actions-based model and isn't moving to consumption-based billing.

 How do I check whether AI is enabled and what my account has used?

An account admin will be able to check the AI toggle in **Account settings**. To see usage and remaining credit from September 1st, 2026, go to **Account settings** > **Billing & Usage**.

The overview and the dbt Wizard usage-based feature page will show your consumption pool, amount used and remaining, and the reset date. Historical dbt Copilot Actions usage appears there too.

 Who do I contact about AI access or usage limits?

Contact your dbt Labs account team for questions about enabling or disabling AI features, purchasing additional usage credits, or contract-specific billing questions. If you're on Developer or Starter plan, [reach out to dbt Support](mailto:support@getdbt.com) for help.

## Billing FAQs

The following questions cover how dbt-managed inference is metered for dbt Wizard usage, what your plan's consumption pool will include, and how you can track and cap your spend once Wizard usage-based billing goes live on September 1st, 2026.

### Wizard usage overview

 How is Wizard usage measured and priced?

Wizard usage with the dbt-managed inference will be measured per token. Every token is processed as input or output counts. Each model has its own unique per-token pricing.

Cost will depend on the model used, prompt length and complexity, and response size.

Refer to [Model Provider Rate table](https://www.getdbt.com/legal/dbt-wizard-token-costs-by-model) and [Service Consumption Table](https://www.getdbt.com/legal/service-consumption-table) for the currently available models.

 What is the dbt Wizard usage consumption pool?

The dbt Wizard consumption pool is the usage balance available when using dbt-managed inference for dbt Wizard — this includes Developer and Starter plan trial Wizard usage credits and the monthly Enterprise/Enterprise+ included usage credits.

As you use dbt Wizard, your token usage will automatically convert into a dollar amount and deducted from your active usage credit. Once your usage credit is depleted, additional usage must draw from a newly purchased consumption pool, if one exists. If your pool is depleted, use of Wizard will be disabled until it is refreshed.

 Is the Wizard consumption pool shared between dbt Wizard in the dbt platform and Wizard CLI?

Yes. The consumption pool and usage credits will be shared across all users within an account and across Wizard in dbt platform and local Wizard CLI. Usage from either surface draws from the same account-level consumption pool.

 Which AI models are available for use through dbt managed inference?

Refer to the [Model Provider Rate table](https://www.getdbt.com/legal/dbt-wizard-token-costs-by-model) and [Service Consumption Table](https://www.getdbt.com/legal/service-consumption-table) for the models available as of September 1, 2026. dbt Labs bills usage of these models through your dbt account.

### Developer and Starter plan (self-serve free trial)

 What does the dbt Wizard Developer and Starter plan free trial include?

Developer and Starter plans get $100 in usage credits for dbt-managed Wizard inference, free for 30 days. The $100 is *per account, not per user*, which means everyone on the account shares the same credits. On Developer, that's a single user, since it's a single-user plan.

The credit covers Wizard usage across both the dbt platform and the CLI, and the trial ends when your account hits $100 in usage or 30 days, whichever comes first.

The trial credits can only be spent on dbt Wizard, not on dbt State or other consumption-based features.

 Who is eligible to start a dbt Wizard 30-day free trial?

Developer or Starter plan accounts are eligible for the 30-day, $100/account free trial. Trials require a business email address, so personal email domains such as Gmail aren't eligible. An account admin or billing admin must start the trial.

 Do unused trial consumption pools roll over or expire?

Unused trial usage credits don't roll over. Your trial ends when you deplete the entire $100 usage credit or 30 days pass, whichever happens first.

 What happens if I deplete my trial usage credit before the 30-day trial ends?

Your trial ends when you reach 30 days or use up the full $100 credits, whichever comes first. Because the credit is shared across the account, usage from any user on it counts toward the same $100. To continue using dbt managed providers, you will need to add a payment method and set a monthly spend limit.

You can also continue with your own AI provider ("Bring Your Own Key (BYOK)") if you configure credentials for a supported AI provider.

 What happens when my dbt Wizard trial ends?

dbt-managed Wizard usage pauses unless paid access is configured by purchasing additional consumption pools.

Self-serve accounts will be able to add a payment method and choose a monthly spend limit.

Enterprise and Enterprise+ accounts should contact their dbt Labs account team. BYOK usage remains separate and is billed by your provider.

 Will dbt automatically charge me when my trial ends?

No. Starting a trial doesn't automatically create paid usage. For self-serve access, you must add a payment method and choose a spend limit. If you set up payment while the trial is active, paid usage begins only after the trial ends or its credit is exhausted.

### Enterprise & Enterprise+ plans

 What does the dbt Wizard Enterprise plan monthly usage credits include?

Enterprise plans automatically include $100/month in usage credits at no cost, and Enterprise+ includes $200/month.

These amounts are *per account, not per user*, which means a 5-person account and a 500-person account both get the same monthly credit, and everyone on the account draws from the same shared balance. No billing setup or opt-in is required to receive the monthly included consumption usage credits as it renews each billing period and doesn't roll over.

These included monthly credits can only be spent on dbt Wizard. They can't be used for dbt State or any other consumption-based feature.

 What happens when your account depletes its monthly usage credits limit?

dbt Managed Wizard usage pauses until an authorized admin purchases additional usage credits through your account team or the next billing cycle begins. BYOK usage is unaffected because your AI provider bills it separately.

### Consumption pool add-on

 How does the consumption pool work?

It's the balance that covers dbt-managed inference usage (which must be purchased once you have depleted any freely available monthly or trial usage credits that may be available), metered per token at cost. Pool dollars don't roll over at the end of a committed term.

Unlike free Wizard usage credits, purchased committed spend isn't limited to dbt Wizard — it covers both dbt Wizard and dbt State.

 Do I pay the full consumption pool or only for what I use?

It depends on how you purchase dbt Wizard:

* **Pay-as-you-go (self-service):** You pay only for actual dbt-managed Wizard usage, up to your selected spend limit. The spend limit is a cap, not a prepaid charge. Typically for Developer, Starter, and self-hosted plans.
* **Pre-committed spend:** You commit to a specific amount upfront through your account team and are billed for that amount. Your usage is deducted from the committed amount as you use dbt Wizard. Typically for Enterprise-tiered plans.

Talk to your account team to set up a pre-committed spend.

 Who can set or change the dbt Wizard consumption pool limit?

An account admin or billing admin can manage Wizard billing and spend controls.

In the dbt platform, go to **Billing & Usage** > **Usage-based features** > **Wizard** to view or update the limit.

 Is the consumption pool for dbt Wizard also shared with dbt State or dbt Copilot?

It depends on which credits you're using:

* Free dbt Wizard usage credits (The Developer and Starter trial pool, and the Enterprise and Enterprise+ monthly included usage credits) are scoped to dbt Wizard only.
* Consumption pool add-on that you purchase covers both dbt Wizard and dbt State, so usage from either feature draws down the same account-level pool.

Either way, Copilot Actions are metered separately on an actions-based model and never touch your dbt Wizard consumption pool. dbt Wizard also has its own feature-level spend limit, configured separately from dbt State.

### Tracking usage & spend limits

 Do I need a paid dbt plan or credit card to try dbt Wizard?

No. You need a free dbt account to manage usage, billing, and spend limits, but you don't need a paid dbt platform plan or credit card to start the trial. If you don't have an account, you can [create one](https://www.getdbt.com/signup) during setup.

 How can I track my dbt Wizard usage and remaining trial credit?

From September 1st, 2026, you'll be able to track in dbt platform by going to **Account settings** > **Billing & Usage**.

The overview and Wizard usage-based feature pages will show your current consumption pool/usage, trial balance, and spend controls across the platform and CLI.

 Where can I find the current token rates for each supported model?

Refer to the [Model Provider Rate Table](https://www.getdbt.com/legal/dbt-wizard-token-costs-by-model) and [Service Consumption Table](https://www.getdbt.com/legal/service-consumption-table), which includes the current input, cache-write, cached-read, and output token rates. Rates vary by model and can change over time.

 How does the spend limit work?

You'll be able to set a monthly spend limit for dbt Wizard in dbt platform. You'll be alerted as you approach it, and usage pauses if you reach it until the limit is raised or the next billing period begins.

### Bring Your Own Key (BYOK)

 Does bring your own key (BYOK) usage consume dbt Wizard consumption pools?

No. With BYOK, your AI provider bills you directly. BYOK usage doesn't draw from your dbt-managed consumption pools.

 How does BYOK work?

With BYOK, you connect your own AI provider credentials and pay the provider directly. BYOK usage doesn't consume your dbt Wizard consumption pool. Refer to the [BYOK setup guide](../platform/wizard-byok-platform.md) for configuration details.

Refer to [Model Provider Rate table](https://www.getdbt.com/legal/dbt-wizard-token-costs-by-model) and [Service Consumption Table](https://www.getdbt.com/legal/service-consumption-table) for more information.

## dbt-managed inference

 Why use dbt-managed inference instead of bringing my own key?

With dbt-managed inference, there's nothing to configure or maintain. dbt Labs selects and maintains the underlying models for cost, speed, and accuracy, so your team focuses on data work, not agent upkeep.

Usage is billed through your existing dbt account and covered by your consumption pool, so there's one bill instead of a second vendor relationship to manage.

 Which models are available with dbt-managed inference, and who picks them?

dbt-managed inference includes several frontier models, including models from OpenAI and Anthropic, plus a set of open weight models. dbt Labs maintains and updates this list, so new models become available without you having to evaluate or configure a new provider yourself.

Refer to [Model Provider Rate table](https://www.getdbt.com/legal/dbt-wizard-token-costs-by-model) and [Service Consumption Table](https://www.getdbt.com/legal/service-consumption-table) for more information.

 Do I need to worry about rate limits or provider outages with dbt-managed inference?

No — dbt manages the underlying provider relationships and infrastructure for dbt-managed inference. You interact with a single consumption pool and spend limit in dbt platform, regardless of which model is handling a given request.

 Can I mix dbt-managed inference and BYOK?

Yes. BYOK usage is billed by your provider and never draws from your dbt-managed consumption pool, so you can use dbt-managed inference for some work and BYOK for other work without either affecting the other's usage or billing.

 Is dbt-managed inference more expensive than using my own provider key?

Cost depends on the model and your usage pattern. dbt-managed inference is metered per token at the model's rate, while BYOK usage is billed directly by your provider at their own rates. Compare the two based on which models and volume you expect to use.

Refer to [Model Provider Rate table](https://www.getdbt.com/legal/dbt-wizard-token-costs-by-model) and [Service Consumption Table](https://www.getdbt.com/legal/service-consumption-table) for more information.

 Does read-only user usage in Explore mode count against my consumption pool?

Yes. Questions asked in [Explore mode](./wizard-ide.md#agent-modes) — including by [read-only users](../platform/wizard-read-only-users.md) — use dbt-managed inference and draw from your account's dbt Wizard consumption pool, the same as any other dbt Wizard usage.

## Related docs

* [Manage AI features in dbt platform](../platform/manage-dbt-ai.md) to turn AI features on or off
* [How dbt Wizard works](./wizard-how-it-works.md)
* [dbt AI usage](../platform/billing/dbt-ai-usage.md) for how dbt AI usage is metered and limited
* [BYOK for the dbt platform](../platform/wizard-byok-platform.md) or [BYOK for the CLI](./wizard-byok.md)
* [Billing](../platform/billing.md) for general dbt platform billing
