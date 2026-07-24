# dbt Wizard in the dbt platform privacy and data [Beta](https://docs.getdbt.com/docs/dbt-versions/product-lifecycles "Go to https://docs.getdbt.com/docs/dbt-versions/product-lifecycles")

dbt platform | Starter, Enterprise, Enterprise+ⓘ

dbt Labs is committed to protecting your privacy and data. This page explains how dbt Wizard in the dbt platform handles your data.

 Does dbt Wizard access my warehouse data?

dbt Wizard can run dbt commands and queries on your behalf, and every query needs your explicit permission first. When a query runs, dbt Wizard sends those results \&mdsah; which may include row-level data — to the AI provider so it can respond in your session.

For dbt-managed AI providers, we have zero data retention (ZDR) agreements in place that prevents the provider from retaining or using this data for training. If you bring your own AI provider (BYOK), that provider's terms will govern retention and training. Always review AI output for accuracy.

 Does dbt Wizard store or use personal data?

dbt Wizard stores your conversation history — including your prompts, responses, and any query results returned during your session — so you can revisit past chats. Conversation history is retained for 90 days; feedback you submit on a dbt Wizard conversation is retained for 400 days. You can delete your conversation history or feedback at any time in the product. dbt Labs does not use your prompts, chat history, command results, or feedback for model training. 

 Is my data used by dbt Labs to train AI models?

No. dbt Labs does not use customer content processed by dbt Wizard — including warehouse query results, prompts, or conversation history — for AI model training. A zero data retention (ZDR) policy is also in place with AI providers, which prevents training on the provider side as well.

 Does dbt Labs share my personal data with third parties?

dbt Labs only shares client personal information as needed to perform the services, under client instructions, or for legal, tax, or compliance reasons.

 Can dbt Wizard data be deleted upon client written request?

Yes. dbt Wizard conversation history is retained for 90 days by default, feedback you submit on a dbt Wizard conversation is retained for 400 days, and you can delete this information in the product at any time. To the extent a client identifies personal or sensitive information uploaded to dbt Labs systems, that data can be deleted within 30 days of written request.

## Related docs[​](#related-docs "Direct link to Related docs")

* [About dbt Wizard in the dbt platform](https://docs.getdbt.com/docs/platform/wizard-platform.md)
* [dbt Wizard in Studio IDE](https://docs.getdbt.com/docs/dbt-ai/wizard-ide.md)
* [dbt Wizard CLI Data & privacy](https://docs.getdbt.com/docs/dbt-ai/wizard-telemetry.md)
