On this page

# The devil is in the docs

May 15, 2026 · 12 min read

[![Mirna Wong](/img/blog/authors/mwong.png)](./authors/mirna-wong.md)

[Mirna Wong](./authors/mirna-wong.md)

Senior Technical Writer at dbt Labs

> *"By all means, move at a glacial pace."* — Miranda Priestly, *[The Devil Wears Prada](https://en.wikipedia.org/wiki/The_Devil_Wears_Prada_\(film\))*

There's another scene in *The Devil Wears Prada* that I think about more than that one. If you haven't seen it yet, I'll try not to spoil it for you. Miranda turns to Andy (Miranda's assistant) and explains, with such impatience, that the [cerulean blue](https://en.wikipedia.org/wiki/Cerulean) in Andy's "lumpy sweater" didn't come out of thin air — it's traced back through a chain of deliberate fashion decisions made years earlier, by people who thought carefully about every choice. The sweater Andy bought from the store was the end of a long chain of deliberate *curation*, carefully veiled to protect the illusion that the sweater effortlessly came into existence.

That's how I'd describe documentation, especially in the AI era. A user — could be a developer, analyst, data engineer, tech writer 😜 — asks an AI tool a question and gets an answer. They don't *need* to see every decision in the chain behind it: what to include, how to structure it, where the gaps are, what needs updating. But those decisions shape every answer they get. Somewhere upstream, a docs team is making careful, deliberate choices that ripple all the way down to the moment the answer lands in the user's editor.

This blog discusses how docs teams are trying to bring those decisions closer to users — and why that architecture matters more than ever in the AI era.

Ready to try it?

Get started with the [dbt MCP quickstart guide](../docs/dbt-ai/mcp-quickstart-oauth.md) or check out the [product docs tools reference](../docs/dbt-ai/mcp-available-tools.md#product-docs) if you're already connected.

## The gap: docs were there, but not natively delivered

Think of Andy in *The Devil Wears Prada* — constantly fielding requests for information she could theoretically find, but scrambling each time because it isn't at her fingertips. She's not uninformed. She's just not connected to the right source when it counts.

That is a little similar to the relationship between the [dbt MCP server](../docs/dbt-ai/about-mcp.md) and the docs:

* The open source documentation at [docs.getdbt.com](https://docs.getdbt.com) is carefully maintained (by the docs team *and* our amazing dbt community), up-to-date, and formatted for humans and now, AI.

  * There's an `llms.txt` index, a full-content flat file, and Markdown output on every page.
  * We have an `AGENTS.md` file that lists how to access the docs via web requests.
  * The source was solid and accessible.

* The dbt MCP server is a powerful feature that dbt users interact with dbt through AI tools. It also had a connection to the docs via web searching and training data.

  * It has eight toolset categories: CLI, Semantic Layer, Discovery, Admin API, SQL, Codegen, Fusion, Server Metadata.
  * But none of them connected directly to the live docs *by default*. It didn't have our docs as its main source.

So when an agent was asked *"how do I configure incremental models?"*, it could absolutely in theory reach the docs with its own browsing tools, but the experience was inconsistent. Sometimes it fetched a page via web search, sometimes it leaned on training data, or pieced an answer together; and when it *did* fetch, it usually pulled rendered HTML rather than the Markdown source — heavier, noisier, and less token-efficient. There was no direct, native path inside the server that pointed agents at the canonical docs in their cleanest form by default.

![Miranda mad at AI for hallucinating about incremental models (but for real an actual response I received from AI when I was adding the docs tools to the MCP server)](/img/blog/2026-04-30-the-devil-is-in-the-docs/dwp-meme.png?v=2 "Miranda mad at AI for hallucinating about incremental models (but for real an actual response I received from AI when I was adding the docs tools to the MCP server)")Miranda mad at AI for hallucinating about incremental models (but for real an actual response I received from AI when I was adding the docs tools to the MCP server)

## The research: using the data to make better decisions

Remember when Andy finally does her homework at Runway — studying the designers, the history, learning who's who — so she can anticipate Miranda's needs before being asked? That's the energy this stage called for: pulling the data, weighing the options, and making a deliberate call before committing to a solution.

Earlier this year we'd been discussing docs data and how AI tools are now fetching them to answer questions. The term 'canonical docs' was mentioned and I struggled to understand what it meant and how it was different from the docs we were already building. But then [Google announced their new docs API and MCP server](https://developers.googleblog.com/introducing-the-developer-knowledge-api-and-mcp-server/) — then it clicked! Docs are now the source of truth (canonical source) and pretty important in the AI era 💃💃💃!

So we chatted about this and toyed with the idea of either:

* Building a new docs MCP server, which meant starting from scratch and building all the infrastructure from the ground up
* Adding docs tools to the existing dbt MCP server, which meant we would need to add the docs toolset category to the existing dbt MCP server (which already has users and is set up)
* Leaving it alone since we have the docs in [dbt agent skills](https://github.com/dbt-labs/dbt-agent-skills) (shout out to the amazing work from the DX team!), which is the way forward for users who install the dbt agent skills

But that still left us with the question of how to get the docs closer to users in a way that is easy to use and maintain.

We ran some SQL queries in our internal analytics project in dbt platform using Fusion in the [dbt VS Code extension](../docs/install-dbt-extension.md) and also used [Insights](../docs/explore/dbt-insights.md) for exploratory analysis.

We saw that the dbt MCP server had these benefits:

* Great adoption already, with a ton of infrastructure in place. Adding the docs tools would be a natural extension of that, bringing docs directly to users where they already work.
* Eight toolset categories already but none of them could directly access docs.getdbt.com.
* Open source and free, which also meant every user would get docs access automatically.

But the problem was when an agent needed to answer *"how do I configure incremental models?"* — there was no guaranteed path to the main source of truth, the current docs.

As mentioned above, we already had a solid foundation for AI-readable docs:

* `docs.getdbt.com/llms.txt` — a page index for AI systems
* `docs.getdbt.com/llms-full.txt` — full docs content in a single file
* `.md` suffix support on any docs page for clean Markdown output
* `AGENTS.md` file that lists how to access the docs via web requests
* A published [`fetching-dbt-docs` skill](https://skills.sh/dbt-labs/dbt-agent-skills/fetching-dbt-docs) that teaches AI agents how to access our docs via web requests, installed across Claude Code, Copilot, Gemini CLI, and others

The skill approach works great and relies on the someone installing it and the agent knowing the skill existed. However, we were curious to see what would help bring docs even closer to users.

## The solution: delivering The Book

In *The Devil Wears Prada*, there's a ritual. Every evening, "The Book" — a secret binder containing the mockup of the upcoming Runway issue — is delivered to Miranda's apartment. Photo proofs, article layouts, ad placements, every choice the editors, stylists, and photographers made that day, bound together in one place. She doesn't go to the office to get it. It arrives, in her space, exactly when she needs it. She reviews it, makes her notes, and what she approves is what gets printed and goes out into the world.

The Book is what carrying a chain of careful decisions actually looks like — bound into one place, delivered to where the next decision gets made.

That's the standard we were trying to meet. Developers shouldn't have to leave their workflow to find docs. The docs — and all the careful decisions baked into them — should arrive in the tool they're already using, at the moment they need to evaluate something and move forward.

The dbt MCP server could already handle lineage lookups, test runs, Semantic Layer queries, job debugging. But ask it *"what's the syntax for a snapshot strategy?"* and it might get it right or it might get it wrong or it might improvise. Fetching from the web. Training data, or best guess. The Book wasn't being "delivered" consistently.

✨ **The solution:** We added two tools to the dbt MCP server under a new **"Product Docs"** category — the ninth toolset✨:

* **[`search_product_docs`](../docs/dbt-ai/mcp-available-tools.md?version=2.0#product-docs)** — searches docs.getdbt.com and returns titles, URLs, and relevance-ranked descriptions for pages matching your query.
* **[`get_product_doc_pages`](../docs/dbt-ai/mcp-available-tools.md?version=2.0#product-docs)** — fetches the full Markdown content of one or more docs pages by path or URL.

The workflow mirrors how a human would use the docs: Search first to find what's relevant, then fetch the full content. The difference is that it happens inside whatever AI tool you're already using, without a context switching headache.

Alongside this, [dbt agent skills](https://skills.sh/dbt-labs/dbt-agent-skills) remain the best path for agents not connected via MCP. The two complement each other: the skill proved the demand; the MCP tools are the native solution.

## How this changes your day-to-day

* Before: Write a macro, hit an unfamiliar function, alt-tab to a browser, search docs, read the page, return to your editor, try to remember where you were. Or ask AI and get an answer that may not be grounded in the canonical, current docs.
* After: Ask Claude or your AI tool directly, get an answer grounded in the canonical, current documentation, keep coding! Docs 🤝 canonical source.

We also added the product docs toolset to dbt's [Developer agent](../docs/dbt-ai/wizard-ide.md) experience — bringing the docs closer to users in dbt platform and the Studio IDE. 🎉

![Developer agent using the product docs tools](/img/blog/2026-04-30-the-devil-is-in-the-docs/dwp-dev-agent.png?v=2 "Developer agent using the product docs tools")Developer agent using the product docs tools

The Book arrived. No context switch required.

So since we've added the docs toolsets in March, over **1000 unique dbt accounts** have reached for the docs through the dbt MCP server — and every one of those calls means that a user didn't have to leave their workflow to find the docs they needed. That's a win for users and a win for the docs team.

And it's growing fast: **338** docs-tool calls in March (the month they launched), **1,113** in April, and over **6,000** so far in May.

[dbt MCP docs-tool calls per month](https://docs.getdbt.com/charts/mcp-docs-usage)

And those calls are mostly coming from *agents* — autonomous, multi-step tools fetching the docs on a user's behalf while they work through a task. That's a different shape of usage than a chat assistant answering a one-off question: the docs aren't just being looked up, they're being pulled into the work itself.

For analysts exploring a shared project, it means understanding what a model does without navigating to a separate tab. For teams working across different dbt setups, it means consistent, authoritative answers regardless of where they're working or who's asking. For the docs team, it means the work we put into writing and maintaining docs.getdbt.com is doing more than it was before — reaching users where they actually are.

![Emily happier that docs are in the MCP server](/img/blog/2026-04-30-the-devil-is-in-the-docs/dwp-wear-chanel.png?v=2 "Emily happier that docs are in the MCP server")Emily happier that docs are in the MCP server

## What's next

This is the first page of many in this new chapter. But we're planning a few things for the next pages as we continue to try to improve the docs experience:

* **Version-aware docs fetching**: Right now these tools return current docs. A developer on dbt Core v1.10 asking about incremental strategies gets 2026 docs. Version-aware routing — returning the right page for the right dbt version — is the next meaningful improvement, and we're [working](https://github.com/dbt-labs/dbt-mcp/pull/638) through it now!
* **Smarter search ranking**: Relevance is good. Relevance tuned to dbt-specific concepts and query patterns would be better.
* **Coverage gaps as a signal**: Usage data tells us which pages answer well and which leave users hunting. Spotting the missing, thin, or outdated ones and fixing them means cleaner answers the next time someone asks.
* **A dedicated dbt docs API or MCP server**: Maybe? It's not the right call right now — one coherent product with an active existing server is the right place to start — but in the future, maybe a standalone docs server could reach every MCP client regardless of whether they use the dbt MCP server at all?

## Give it a try!

* If you're new to the MCP server, the [quickstart guide](../docs/dbt-ai/mcp-quickstart-oauth.md?version=2.0) walks through both local and remote configuration.

* If you're already connected to the dbt MCP server, the product docs tools are available now. Check out the [MCP available tools reference](../docs/dbt-ai/mcp-available-tools.md?version=2.0#product-docs) for details.

We'd love to hear your feedback! Open an issue at [github.com/dbt-labs/docs.getdbt.com](https://github.com/dbt-labs/docs.getdbt.com/issues) to flag any bugs, typos, wrong info, etc. — or find me in the [dbt community Slack](https://www.getdbt.com/community/join-the-community).

## Conclusion

Miranda's point about the cerulean sweater wasn't really about fashion. It was about invisible chains — every deliberate decision ripples further than the person making it can see, and the quality of the source shapes everything downstream.

That's where docs are now. The docs team's decisions — what to include, how to structure it, where the gaps are — used to ripple slowly. A developer might hit them through a Google search, or a Stack Overflow link, or maybe never! Now those decisions arrive in the developer's AI tool, bound into something they can act on, the same way The Book arrives at Miranda's apartment. They flip through, make their call, ship their code. They don't need to see *every* decision in the chain. They just need the chain to reach them.

We're no longer moving at a glacial pace. The current runs *fast*. And the devil? Well, it's in the docs.

*Mirna Wong is a technical writer at dbt Labs who loves em-dashes, pop culture references, and writing about dbt and AI. dbt's product docs are open source — contributions and issues are **always** welcome at [github.com/dbt-labs/docs.getdbt.com](https://github.com/dbt-labs/docs.getdbt.com/issues).*

**Tags:**

* [ai](./tags/ai.md)
* [docs](./tags/docs.md)
* [mcp](./tags/mcp.md)

[Newer post](./2026-06-01-dbt-core-v2-is-here.md)

[dbt Core v2 is here: still open source, now rebuilt for what's next](./2026-06-01-dbt-core-v2-is-here.md)

[Older post](./2026-04-15-building-a-better-data-agent-benchmark.md)

[Building a better data agent benchmark](./2026-04-15-building-a-better-data-agent-benchmark.md)
