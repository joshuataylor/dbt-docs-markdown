# Arrow ADBC and Fusion

This document provides technical guidance for dbt partners and vendors on how to design, build, and maintain ADBC (Apache Arrow Database Connectivity) drivers for Fusion, the new dbt engine.

Fusion leverages ADBC as a unified driver layer for seamless, high-performance integration with data platforms. Building an ADBC driver is the first step to connecting Fusion with a new platform.

## Why Fusion uses ADBC[​](#why-fusion-uses-adbc "Direct link to Why Fusion uses ADBC")

The dbt Fusion engine represents a major evolution in the dbt engine with minimal changes to the authoring layer. Built in Rust, Fusion delivers speed, language understanding, and seamless integration with numerous data warehouses. A key aspect of the new engine is its adoption of ADBC — a modern, open standard from the Apache Arrow project that simplifies columnar data interchange across platforms.

Historically, dbt Core adapters required bespoke connection logic for each data platform. Fusion improves on this model with a unified ADBC driver layer that offers several key advantages:

* **Standardization**: ADBC standardizes common platform features across a single interface.
* **Performance**: Drivers leverage Arrow's columnar memory format for efficient query execution with minimal transformations.
* **Maintainability**: ADBC drivers follow a shared specification, reducing the complexity of implementing new adapters.

## Technical overview[​](#technical-overview "Direct link to Technical overview")

This technical specification covers the ADBC specification. The specification maintains backwards compatibility, so guidance here remains valid as the spec evolves. For the latest information and detailed documentation, refer to the [ADBC documentation](https://arrow.apache.org/adbc/current/), which is the source of truth.

The ADBC API provides a powerful array of features, but you don't need to implement all of them. This section covers the API surface required for Fusion compatibility.

### Programming language[​](#programming-language "Direct link to Programming language")

**tl;dr: Use Go.**

One distinct advantage of Arrow ADBC is portability. You can write drivers in various languages and load them via driver managers. This portability allows Fusion (written in Rust) to leverage drivers written in other languages.

For Fusion compatibility, drivers must:

* Compile into shared libraries that can be loaded from any program
* Produce a platform-specific, standalone binary

We recommend **Go** as the language of choice, though Rust or C++ also work. Go has a runtime and garbage collector, but it's engineered to compile into well-behaved shared libraries—unlike languages like C# or Java. A standalone binary allows users to download and run the driver out of the box without setting up an interpreter. Compiled languages like Go also enable Fusion and its drivers to share memory directly over FFI without external dependencies.

### ADBC specifications[​](#adbc-specifications "Direct link to ADBC specifications")

This section covers the minimum requirements for a Fusion-compatible ADBC driver. For complete details on the ADBC specification and driver development, refer to the [ADBC driver authoring guide](https://arrow.apache.org/adbc/current/driver/authoring.html).

Drivers consist of several key abstractions:

1. **Driver**: Load a driver to create databases.
2. **Database**: Create databases and set configuration options (including authentication). Use databases to open connections.
3. **Connection**: Establish connections to the warehouse. Connections create statements.
4. **Statement**: Set options and SQL queries on statements, then execute them against the warehouse.

Fusion achieves high performance through aggressive parallelism, so expect many simultaneous connections during project execution.

### Authentication[​](#authentication "Direct link to Authentication")

Drivers handle authentication through key-value options set on the database. Fusion translates options from user-authored `profiles.yml` files before passing them to the driver. For example, what dbt calls `client_secret` in a Snowflake profile gets set on the driver as `adbc.snowflake.sql.client_option.client_secret`.

For a complete example of how Fusion translates profile options, see the [Snowflake authentication source code](https://github.com/dbt-labs/dbt-fusion/blob/main/crates/dbt-auth/src/snowflake/mod.rs).

For more information on profile configuration, refer to [dbt profiles](https://docs.getdbt.com/docs/local/profiles.yml.md).

#### Credential caching[​](#credential-caching "Direct link to Credential caching")

Simple authentication methods (like username/password stored in `profiles.yml`) support fully parallel connection creation with no special handling required.

For authentication methods that require browser interaction (user-to-machine OAuth, SSO, or MFA), implement credential caching. Due to Fusion's highly parallel execution, without caching, every new connection prompts the user for authentication repeatedly.

Your credential cache for browser-based authentication must:

* Block new connections until an initial connection establishes and stores a token in memory (avoiding the thundering herd problem).
* Handle token refresh using the same blocking principle when invalidation occurs.
* Use interprocess, file-system-based storage to support the LSP, which runs in a separate process.

This caching is critical for any browser-based or MFA authentication option, but is not needed for simple credential-based authentication.

## Required APIs[​](#required-apis "Direct link to Required APIs")

This section covers the minimum API set for Fusion compatibility. The requirements are:

* Authentication via options.
* SQL query execution.
* Metadata queries to understand remote warehouse state (certain connection-level metadata functions can use cheaper or more performant APIs to pull table schemas).

These requirements are not exhaustive. dbt Labs encourages implementing the full ADBC specification to benefit both Fusion and the broader ADBC community.

#### Driver[​](#driver "Direct link to Driver")

| Method        | Description                     |
| ------------- | ------------------------------- |
| `NewDatabase` | Create a new database instance. |

Search table...

|                  |   |   |   |   |
| ---------------- | - | - | - | - |
| Loading table... |   |   |   |   |

#### Database[​](#database "Direct link to Database")

| Method       | Description                                           |
| ------------ | ----------------------------------------------------- |
| `SetOptions` | Set configuration options (including authentication). |
| `Open`       | Open a connection.                                    |
| `Close`      | Close the database.                                   |

Search table...

|                  |   |   |   |   |
| ---------------- | - | - | - | - |
| Loading table... |   |   |   |   |

#### Connection[​](#connection "Direct link to Connection")

| Method           | Description                             |
| ---------------- | --------------------------------------- |
| `GetObjects`     | Pull metadata from the warehouse.       |
| `GetTableSchema` | Pull schema metadata for tables.        |
| `NewStatement`   | Create a statement for query execution. |
| `Close`          | Close the connection.                   |

Search table...

|                  |   |   |   |   |
| ---------------- | - | - | - | - |
| Loading table... |   |   |   |   |

#### Statement[​](#statement "Direct link to Statement")

| Method          | Description                         |
| --------------- | ----------------------------------- |
| `SetOption`     | Set options on queries.             |
| `SetSqlQuery`   | Set the SQL query text.             |
| `ExecuteQuery`  | Execute a query and return results. |
| `ExecuteUpdate` | Execute DML queries.                |
| `Close`         | Close the statement.                |

Search table...

|                  |   |   |   |   |
| ---------------- | - | - | - | - |
| Loading table... |   |   |   |   |

## Was this page helpful?

YesNo

[Privacy policy](https://www.getdbt.com/cloud/privacy-policy)[Create a GitHub issue](https://github.com/dbt-labs/docs.getdbt.com/issues)

This site is protected by reCAPTCHA and the Google [Privacy Policy](https://policies.google.com/privacy) and [Terms of Service](https://policies.google.com/terms) apply.
