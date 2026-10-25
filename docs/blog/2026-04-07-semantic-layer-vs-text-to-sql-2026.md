On this page

# Semantic Layer vs. Text-to-SQL: 2026 Benchmark Update

April 7, 2026 · 11 min read

[![Jason Ganz](/img/blog/authors/jasnonaz.jpeg)](./authors/jason-ganz.md)

[Jason Ganz](./authors/jason-ganz.md)

Director of Community, Developer Experience & AI at dbt Labs

[![Benoit Perigaud](/img/blog/authors/benoit-perigaud.jpeg)](./authors/benoit-perigaud.md)

[Benoit Perigaud](./authors/benoit-perigaud.md)

Staff Developer Experience Advocate at dbt Labs

There are two primary ways to get answers from your data using LLMs today: have the model write SQL directly, or have it query through a structured ontology like the dbt Semantic Layer. Both work. Companies are getting real value from each. But they fail in very different ways, and understanding those failure modes is what actually matters when you're deciding which to use.

In 2023, [we ran a benchmark](https://roundup.getdbt.com/p/semantic-layer-as-the-data-interface) comparing the two approaches and the Semantic Layer won handily. But 2023 is roughly 10 million years ago in LLM time. Models have gotten dramatically better at writing SQL. So we reran the benchmark with the latest generation models to see whether the gap has closed.

## TL;DR

|   |
| - |

| Model             | Method         | Accuracy % |
| ----------------- | -------------- | ---------- |
| claude-sonnet-4-6 | Text-to-SQL    | 90.0%      |
| claude-sonnet-4-6 | Semantic Layer | 98.2%      |
| gpt-5.3-codex     | Text-to-SQL    | 84.1%      |
| gpt-5.3-codex     | Semantic Layer | 100.0%     |

* **Models are getting much better at text-to-SQL.** The improvement from GPT-4 era to today is dramatic.
* **For queries covered by a well-modeled Semantic Layer, accuracy approaches or hits 100%.** The Semantic Layer's deterministic query generation means the LLM can't produce subtly wrong results.
* **Data modeling quality matters enormously for both approaches.** Adding even minimal modeling on top of raw tables improved results across the board.
* **Our recommendation:** Text-to-SQL for ad hoc analyses and smaller datasets. Semantic Layer for enterprise use where accuracy is critical and datasets are large, complex, or messy.

These numbers will keep improving, but we expect the underlying principles to have staying power. **If you just want to try the benchmark yourself or see more detailed data and the associated costs, head to [the repo](https://github.com/dbt-labs/dbt-llm-sl-bench).** Otherwise, keep reading for the full methodology and what we learned.

## How the two approaches actually work

Before we get into the numbers, a quick refresher on what's happening under the hood.

**Text-to-SQL** is the more straightforward pattern. You provide an LLM with some amount of schema information and ask it to generate a SQL query that answers a natural language question. The LLM has to infer the semantics of your data from structural clues: table names, column names, relationships. It then writes a query from scratch every time. This makes it flexible (any question is fair game as long as the data exists), but also fragile. The LLM might join tables incorrectly, misinterpret a column's meaning, or produce a query that runs successfully but returns wrong results. There's no guardrail between the question and the generated SQL.

Ontology-driven approaches, like the **dbt Semantic Layer**, work differently. Instead of asking the LLM to write raw SQL, you define a structured ontology (metrics, dimensions, entities, and the relationships between them) that encodes your business logic. The LLM's job is then reduced to decomposing a natural language question into the correct combination of metrics and dimensions. dbt's Semantic Layer uses its engine MetricFlow to handle the actual query generation deterministically. This means the LLM can't produce an incorrect join or a bad aggregation: if it picks the right metric and dimensions, the query is guaranteed to be correct. Perhaps more importantly, it can't produce *correct-looking numbers that are subtly different across runs*: the logic is codified and deterministic. The trade-off is coverage: the Semantic Layer can only answer questions that fall within the scope of what's been modeled.

Both methods have their place. But where exactly does each one shine, and where does it fall down? We tested it.

## The benchmark

We ran our experiment against the [ACME Insurance benchmark](https://github.com/datadotworld/cwd-benchmark-data) originally created by Juan Sequeda et al. from data.world, a semi-complex dataset meant to mimic real-world analytical problems. 11 questions, each run 20 times, across multiple LLMs.

We tested four configurations:

* **Text-to-SQL:** The agent gets schema information and writes queries from scratch.
* **Minimal Semantic Layer:** A light-touch dbt project on top of the original (highly normalized) tables.
* **Modeled Semantic Layer:** A reworked project with proper modeling conforming to dbt best practices.
* **Text-to-SQL on top of modeled data:** The agent is asked to create queries from scratch but it now has access to the same new models created for the previous use case.

![Diagram showing the four benchmark configurations: Text-to-SQL, Minimal Semantic Layer, Modeled Semantic Layer, and Text-to-SQL on modeled data](/img/blog/2026-04-07-semantic-layer-vs-text-to-sql-2026/benchmark-configurations.png?v=2)

The most "real world" comparison for dbt users is text-to-SQL vs. Semantic Layer on the modeled project. An important caveat: to make text-to-SQL work, we loaded the entire schema as context, which isn't practical for larger datasets. Keep that in mind as you read the numbers.

## Which model should you use? (It matters less than you'd think)

Before running the full benchmark, we wanted to know: does the choice of model or reasoning effort level make a meaningful difference?

We tested Opus 4.6, Sonnet 4.6, GPT-5.3 Codex, and GPT-5.2 (GPT-5.4 wasn't available yet via API) across multiple reasoning levels: Anthropic's `low` to `max` and OpenAI's `none` to `xhigh`. Here's what 16 permutations looked like for questions answerable via the Semantic Layer:

|   |
| - |

| Model              | Effort | SL Accuracy % | Text-to-SQL Accuracy % | SL Advantage (pp) |
| ------------------ | ------ | ------------- | ---------------------- | ----------------- |
| gpt-5.3-codex      | none   | 100.0         | 50.0                   | 50.0              |
| gpt-5.2-2025-12-11 | medium | 100.0         | 52.5                   | 47.5              |
| gpt-5.3-codex      | low    | 100.0         | 52.5                   | 47.5              |
| gpt-5.3-codex      | medium | 100.0         | 52.5                   | 47.5              |
| gpt-5.2-2025-12-11 | none   | 95.0          | 50.0                   | 45.0              |
| gpt-5.2-2025-12-11 | low    | 100.0         | 55.0                   | 45.0              |
| gpt-5.2-2025-12-11 | high   | 100.0         | 55.0                   | 45.0              |
| claude-sonnet-4-6  | low    | 100.0         | 62.5                   | 37.5              |
| claude-sonnet-4-6  | medium | 100.0         | 62.5                   | 37.5              |
| claude-sonnet-4-6  | max    | 100.0         | 62.5                   | 37.5              |
| gpt-5.3-codex      | high   | 97.5          | 60.0                   | 37.5              |
| claude-sonnet-4-6  | high   | 97.5          | 62.5                   | 35.0              |
| gpt-5.2-2025-12-11 | xhigh  | 100.0         | 65.0                   | 35.0              |
| gpt-5.3-codex      | xhigh  | 100.0         | 65.0                   | 35.0              |
| claude-opus-4-6    | low    | 87.5          | 62.5                   | 25.0              |
| claude-opus-4-6    | max    | 87.5          | 62.5                   | 25.0              |
| claude-opus-4-6    | medium | 87.2          | 62.5                   | 24.7              |
| claude-opus-4-6    | high   | 87.2          | 62.5                   | 24.7              |
