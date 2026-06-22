# Other artifact files

### index.html[​](#indexhtml "Direct link to index.html")

**Produced by:** [`docs generate`](https://docs.getdbt.com/reference/commands/cmd-docs.md)

This file is the skeleton of the [auto-generated dbt documentation website](https://docs.getdbt.com/docs/explore/build-and-view-your-docs.md). The contents of the site are populated by the [manifest](https://docs.getdbt.com/reference/artifacts/manifest-json.md) and [catalog](https://docs.getdbt.com/reference/artifacts/catalog-json.md).

Note: the source code for `index.json` comes from the [dbt-docs repo](https://github.com/dbt-labs/dbt-docs). Head over there if you want to make a bug report, suggestion, or contribution relating to the documentation site.

### partial\_parse.msgpack[​](#partial_parsemsgpack "Direct link to partial_parse.msgpack")

**Produced by:** [manifest commands](https://docs.getdbt.com/reference/artifacts/manifest-json.md) + [`parse`](https://docs.getdbt.com/reference/commands/parse.md)

This file is used to store a compressed representation of files dbt has parsed. If you have [partial parsing](https://docs.getdbt.com/reference/parsing.md#partial-parsing) enabled, dbt will use this file to identify the files that have changed and avoid re-parsing the rest.

### graph.gpickle[​](#graphgpickle "Direct link to graph.gpickle")

**Produced by:** commands supporting [node selection](https://docs.getdbt.com/reference/node-selection/syntax.md)

Stores the network representation of the dbt resource DAG.

### graph\_summary.json[​](#graph_summaryjson "Direct link to graph_summary.json")

**Produced by:** [manifest commands](https://docs.getdbt.com/reference/artifacts/manifest-json.md)

This file is useful for investigating performance issues in dbt Core's graph algorithms.

It is more anonymized and compact than [`manifest.json`](https://docs.getdbt.com/reference/artifacts/manifest-json.md) and [`graph.gpickle`](#graph.gpickle).

It includes that information at two separate points in time:

1. `linked` — immediately after the graph is linked together, and
2. `with_test_edges` — after test edges have been added.

Each of those points in time contains the `name` and `type` of each node and `succ` contains the keys of its child nodes.

## Was this page helpful?

YesNo

[Privacy policy](https://www.getdbt.com/cloud/privacy-policy)[Create a GitHub issue](https://github.com/dbt-labs/docs.getdbt.com/issues)

This site is protected by reCAPTCHA and the Google [Privacy Policy](https://policies.google.com/privacy) and [Terms of Service](https://policies.google.com/terms) apply.
