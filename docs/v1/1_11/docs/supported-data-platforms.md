# Supported data platforms

dbt connects to and runs SQL against your database, warehouse, lake, or query engine. These SQL-speaking platforms are collectively referred to as *data platforms*. dbt connects with data platforms by using a dedicated adapter plugin for each. Plugins are built as Python modules that dbt Core discovers if they are installed on your system. Refer to the [Build, test, document, and promote adapters](../guides/adapter-creation.md) guide for details.

## Types of Adapters

There are two types of adapters available today:

* **Trusted** — [Trusted adapters](./trusted-adapters.md) are those where the adapter maintainers have decided to participate in the Trusted Adapter Program and have made a commitment to meeting those requirements. For adapters supported in dbt, maintainers have undergone an additional rigorous process that covers contractual requirements for development, documentation, user experience, and maintenance.
* **Community** — [Community adapters](./community-adapters.md) are open-source and maintained by community members. These adapters are not part of the Trusted Adapter Program and could have usage inconsistencies.

Considerations for depending on an open-source project

1. Does it work?
2. Does anyone "own" the code, or is anyone liable for ensuring it works?
3. Do bugs get fixed quickly?
4. Does it stay up-to-date with new dbt Core features?
5. Is the usage substantial enough to self-sustain?
6. Do other known projects depend on this library?
