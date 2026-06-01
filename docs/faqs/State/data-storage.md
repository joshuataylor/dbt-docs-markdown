# How is data stored in dbt State?


dbt State sends last-modified timestamps and SQL statements to dbt Labs servers. SQL statements are hashed before transmission, so dbt Labs cannot see the contents. These hashes are used to identify whether a statement has changed by comparing them on each run.
