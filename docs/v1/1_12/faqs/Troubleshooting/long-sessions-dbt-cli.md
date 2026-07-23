# I'm getting a "Session occupied" error in dbt CLI?

If you're receiving a `Session occupied` error in the dbt CLI or if you're experiencing a long-running session, you can use the `dbt invocation list` command in a separate terminal window to view the status of your active session. This helps debug the issue and identify the arguments that are causing the long-running session.

To cancel an active session, use the `Ctrl + Z` shortcut.

To learn more about the `dbt invocation` command, see the [dbt invocation command reference](https://docs.getdbt.com/reference/commands/invocation.md).

Alternatively, you can reattach to your existing session with `dbt reattach` and then press `Control-C` and choose to cancel the invocation.
