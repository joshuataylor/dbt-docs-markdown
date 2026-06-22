# dbt Wizard CLI data use and telemetry

dbt Wizard CLI collects anonymous product telemetry to improve the AI agent experience, understand usage patterns, optimize performance, and attribute compute costs without capturing your code, queries, prompts, responses, or file contents.

## Opt out of client telemetry[​](#opt-out-of-client-telemetry "Direct link to Opt out of client telemetry")

dbt Wizard CLI respects the following opt-out mechanisms, checked in order:

1. Set `DO_NOT_TRACK=1`.
2. Set `DBT_SEND_ANONYMOUS_USAGE_STATS=false`.
3. Store telemetry consent in `~/.dbt/.user.yml`.

Setting any of these disables telemetry from the dbt Wizard CLI client.

## What dbt Wizard does not collect[​](#what-dbt-wizard-does-not-collect "Direct link to What dbt Wizard does not collect")

dbt Wizard CLI does not collect:

* Prompt or response content.
* SQL queries, file paths, or dbt node names.
* MCP tool call arguments or outputs.
* Raw API keys or tokens. Identifiers are hashed before transmission.
* Error messages that contain user content. Errors are limited to class or code.

## Events collected[​](#events-collected "Direct link to Events collected")

| Event                           | When it is collected                                 | Why it is collected                                                             |
| ------------------------------- | ---------------------------------------------------- | ------------------------------------------------------------------------------- |
| LLM request completed or failed | When an LLM request completes                        | Token usage, model, latency monitoring, error rates, and cost attribution       |
| Wizard session started or ended | When a user opens or closes a dbt Wizard CLI session | Weekly active users, session duration, model adoption, and client surface usage |
| Wizard turn completed           | After a user message and AI response complete        | Engagement depth, token consumption, model usage, status, and duration          |
| Wizard tool use                 | Each time the agent invokes a tool                   | Tool adoption, reliability, and performance                                     |

Tool telemetry records the tool type, tool name, whether the call failed, and execution time. Tool arguments and outputs are not collected.

## Data handling[​](#data-handling "Direct link to Data handling")

* Telemetry is transmitted over HTTPS to dbt Labs ingestion infrastructure.
* Events are stored in an internal dbt Labs data warehouse.
* Telemetry is not shared with third parties.
* API keys and tokens are never transmitted in raw form.
* Local development users who opt out with the supported environment variables generate no dbt Wizard CLI client telemetry events.

## Related docs[​](#related-docs "Direct link to Related docs")

* [Install dbt Wizard CLI](https://docs.getdbt.com/docs/dbt-ai/wizard-cli.md)
* [Configure BYOK for dbt Wizard](https://docs.getdbt.com/docs/dbt-ai/wizard-byok.md)
* [dbt Wizard in the dbt platform Data & privacy](https://docs.getdbt.com/docs/dbt-ai/wizard-platform-privacy-data.md)
* [dbt AI FAQs](https://docs.getdbt.com/docs/dbt-ai/dbt-ai-faqs.md)
