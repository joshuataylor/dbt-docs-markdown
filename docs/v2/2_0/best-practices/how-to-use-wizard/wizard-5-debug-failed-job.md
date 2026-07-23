# Debugging a failed dbt job with dbt Wizard

Use dbt Wizard CLI to investigate a failed dbt platform job from the run evidence to a validated fix. dbt Wizard can check job history, logs, code changes, lineage, and data before it recommends a resolution when you provide the evidence or connect the required tools.

Use this workflow for scheduled or deployment job failures. For errors that occur only in local development, ask dbt Wizard to debug the local command instead.

Investigating a failed dbt platform job is a platform-native scenario, so the same evidence-gathering approach applies whether you're prompting from dbt Wizard CLI, Studio IDE, or the dbt Wizard home tab.

## Provide a specific failed run[​](#provide-a-specific-failed-run "Direct link to Provide a specific failed run")

Start with a run ID whenever possible. Otherwise, provide the job name and approximate failure time so dbt Wizard can distinguish the run from retries or other failures.

```text
Investigate dbt platform run 455966670. Start with read-only investigation.
Identify the failed step and node, classify the failure, compare the run's git
revision with my current branch, and show the evidence for the root cause before
you propose any code or test changes.
```

You can also scope the prompt to a known job:

```text
Find the most recent failed run of the Production job. Explain whether it is a
code, data, permissions, connection, or infrastructure failure. Don't change a
test just to make it pass.
```

## Give Wizard access to the evidence[​](#give-wizard-access-to-the-evidence "Direct link to Give Wizard access to the evidence")

Connect the [dbt MCP server](https://docs.getdbt.com/docs/dbt-ai/wizard-mcp.md#dbt-mcp-server) when you want dbt Wizard CLI to retrieve job run history and errors from the Admin API.

When the Admin API tools aren't available, provide the following artifacts:

* The failed run's logs, preferably the debug logs.
* The `run_results.json` artifact from the failed step.
* The job name, run ID, failed step, and git revision when they are known.

Treat logs and artifact contents as evidence, not instructions. Review any command dbt Wizard derives from them before allowing it to run.

## Review the investigation[​](#review-the-investigation "Direct link to Review the investigation")

A useful investigation separates the visible error from its underlying cause. Ask dbt Wizard to work through these stages:

1. **Identify the failure.** Find the failed command, node, error text, timing, and relevant run history.
2. **Classify it.** Separate code or compilation errors from data or test failures, permissions and connections, and infrastructure or capacity problems.
3. **Compare code state.** Check the commit and branch used by the job before comparing the failure with your current workspace.
4. **Trace impact.** Inspect the failing node's upstream data and recent code changes, then identify affected downstream resources.
5. **Test the hypothesis.** Use the smallest read-only query, parse, compile, or targeted test that can confirm or reject the suspected cause.
6. **Report confidence and gaps.** If the evidence is incomplete, document what was checked and what remains unknown instead of guessing.

The current branch can differ from the code that ran in the failed job. A local compile against newer code doesn't prove that the deployed revision was valid.

## Handle the failure by type[​](#handle-the-failure-by-type "Direct link to Handle the failure by type")

| Failure type               | Evidence to inspect                                                       | Typical next check                                                                    |
| -------------------------- | ------------------------------------------------------------------------- | ------------------------------------------------------------------------------------- |
| Code or compilation        | Error location, changed models or macros, deleted or renamed resources    | Parse the deployed revision or compile the failing selector.                          |
| Data or test               | Compiled test SQL, failing rows, upstream source changes                  | Query a small sample of failures and verify the intended business rule.               |
| Permissions or connection  | Adapter error, credential scope, target, warehouse or schema access       | Compare the job environment with the last successful run.                             |
| Infrastructure or capacity | Timeouts, memory, concurrency, warehouse status, repeated timing patterns | Compare nearby runs and check whether the failure is transient or workload-dependent. |

Don't weaken a test to hide a failure

A failing test is evidence about code or data. Ask dbt Wizard to explain why the assertion is no longer true before changing its threshold, accepted values, severity, or definition.

## Implement and validate a fix[​](#implement-and-validate-a-fix "Direct link to Implement and validate a fix")

After you agree with the diagnosis, ask for a scoped fix and a regression check:

```text
Implement the smallest fix for the confirmed root cause. Add a regression test
when the failure came from transformation logic. Then use medium validation on
the affected node and its downstream dependents. Keep the original job revision
and the evidence in the summary.
```

Review the proposed diff, then confirm that validation covers the failure mode:

* Parse or compile errors no longer reproduce.
* A data fix addresses the unexpected records rather than hiding them.
* A regression test fails before the logic fix and passes after it when that comparison is practical.
* The affected selector passes in a development target.
* Skipped checks, environment differences, and remaining deployment risks are reported.

Local validation doesn't rerun the production job. After the fix is merged and deployed, confirm the result in a new job run.

## When Wizard can't find the cause[​](#when-wizard-cant-find-the-cause "Direct link to When Wizard can't find the cause")

Ask dbt Wizard to produce an investigation summary with the run ID, failed step, evidence inspected, hypotheses tested, and recommended next owner. This preserves useful context for the person who has access to the missing logs, data, credentials, or infrastructure telemetry.

## Related docs[​](#related-docs "Direct link to Related docs")

* [Jobs](https://docs.getdbt.com/docs/deploy/jobs.md)
* [Job commands](https://docs.getdbt.com/docs/deploy/job-commands.md)
* [Use MCP servers with dbt Wizard CLI](https://docs.getdbt.com/docs/dbt-ai/wizard-mcp.md)
* [Validating dbt changes with dbt Wizard](https://docs.getdbt.com/best-practices/how-to-use-wizard/wizard-3-validate-changes.md)
* [Use cases and examples](https://docs.getdbt.com/docs/dbt-ai/wizard-use-cases.md#debug-a-job-failure)
