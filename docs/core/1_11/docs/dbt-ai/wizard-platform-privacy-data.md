# dbt Wizard in the dbt platform privacy and data [Beta](https://docs.getdbt.com/docs/dbt-versions/product-lifecycles "Go to https://docs.getdbt.com/docs/dbt-versions/product-lifecycles")

dbt Labs is committed to protecting your privacy and data. This page explains how dbt Wizard in the dbt platform handles your data.

 Does dbt Wizard access my warehouse data?

dbt Wizard can initiate dbt commands and run queries on your behalf. When those actions run, dbt Wizard can view the resulting outputs — which may include row-level data — to respond in your session. dbt Wizard does not extract raw warehouse data outside of your controlled environment. Any warehouse query requires your explicit permission before it runs. You should always review AI output for completeness and accuracy.

 Does dbt Wizard store or use personal data?

dbt Wizard stores your conversation history — including your prompts, responses, and any query results returned during your session — so you can revisit past chats. Conversation content is encrypted at rest and in transit. History is retained for 90 days and is only visible to you; dbt Labs cannot access or decrypt it. You can delete your conversation history at any time in the product. dbt Labs does not use your prompts, chat history, or command results for model training.

 Is my data used by dbt Labs to train AI models?

No. dbt Labs does not use customer content processed by dbt Wizard — including warehouse query results, prompts, or conversation history — for AI model training. A zero data retention (ZDR) policy is also in place with AI providers, which prevents training on the provider side as well.

 Does dbt Labs share my personal data with third parties?

dbt Labs only shares client personal information as needed to perform the services, under client instructions, or for legal, tax, or compliance reasons.

 Can dbt Wizard data be deleted upon client written request?

Yes. dbt Wizard conversation history is retained for 90 days by default, and you can delete chats in the product at any time. Conversation data — including query results stored in conversation history — is encrypted and only decrypted at your request. To the extent a client identifies personal or sensitive information uploaded to dbt Labs systems, that data can be deleted within 30 days of written request.

## Related docs[​](#related-docs "Direct link to Related docs")

* [About dbt Wizard in the dbt platform](https://docs.getdbt.com/docs/platform/wizard-platform.md)
* [dbt Wizard in Studio IDE](https://docs.getdbt.com/docs/dbt-ai/wizard-ide.md)
* [dbt Wizard CLI Data & privacy](https://docs.getdbt.com/docs/dbt-ai/wizard-telemetry.md)
