# Headless mode [Beta](https://docs.getdbt.com/docs/dbt-versions/product-lifecycles "Go to https://docs.getdbt.com/docs/dbt-versions/product-lifecycles")

dbt Wizard can run without the interactive TUI — useful for scripts, CI pipelines, pre-commit hooks, and any workflow where you want a one-shot result without human-in-the-loop approval.

See it in action and share your feedback

Want to see dbt Wizard in action? Check out the [demo video](https://www.youtube.com/watch?v=-lIzh1xQWMA).

We'd love to hear how dbt Wizard is working for you. Share your feedback by either running the `/feedback` slash command in your interactive terminal session or by going to the [#dbt-wizard](https://getdbt.slack.com/archives/C0B6KLW6T26) channel in the [dbt Community Slack](https://docs.getdbt.com/community/join?version=2.0).

Thanks so much for your help in improving dbt Wizard and dbt data development!

## `exec` — one-shot prompts[​](#exec--one-shot-prompts "Direct link to exec--one-shot-prompts")

Run a single prompt and exit:

```bash
wizard exec "list all models with no tests"
```

Pipe input via stdin:

```bash
echo "which sources have stale freshness?" | wizard exec -
```

Use `exec` in CI to gate on quality checks:

```bash
# Check test coverage before merging
wizard exec "are there any models in models/marts/ with no tests?"
```

### JSON output[​](#json-output "Direct link to JSON output")

For downstream processing, emit a structured JSON event stream:

```bash
wizard exec --json "summarize test coverage by schema" > coverage.json
```

With a JSON Schema to constrain the response shape:

```bash
wizard exec \
  --json \
  --output-schema ./schemas/coverage-response.json \
  "summarize test coverage by schema"
```

Write the final message to a file:

```bash
wizard exec \
  --output-last-message ./review-output.md \
  "review the changes in this branch for correctness"
```

## `review` — automated code review[​](#review--automated-code-review "Direct link to review--automated-code-review")

Review uncommitted changes:

```bash
wizard review --uncommitted
```

Review a branch diff in CI:

```bash
wizard review --base main
```

Review a specific commit:

```bash
wizard review --commit abc1234
```

### Example: GitHub Actions code review[​](#example-github-actions-code-review "Direct link to Example: GitHub Actions code review")

```yaml
- name: dbt Wizard review
  run: |
    wizard review \
      --base ${{ github.base_ref }} > review.md
  env:
    OPENAI_API_KEY: ${{ secrets.OPENAI_API_KEY }}
```

## Permissions in headless mode[​](#permissions-in-headless-mode "Direct link to Permissions in headless mode")

In headless `exec` mode, Wizard runs without interactive approval prompts. Pre-grant the sandbox permissions you need:

```bash
# Read-only analysis (default — safe for CI)
wizard exec "list models with no documentation"

# Allow file writes inside the workspace
wizard exec -s workspace-write "add not_null tests to all primary keys in staging"

# Allow shell commands like dbt compile
wizard exec -s workspace-write "compile and validate fct_orders"
```

For read-only analysis tasks (coverage checks, impact queries, documentation gaps), the default permissions are sufficient. For tasks that write files or run dbt commands, pass the appropriate flags explicitly.

## Related docs[​](#related-docs "Direct link to Related docs")

* [dbt Wizard command reference](https://docs.getdbt.com/docs/dbt-ai/wizard-cli-reference.md)
* [Use cases and examples](https://docs.getdbt.com/docs/dbt-ai/wizard-use-cases.md)

## Was this page helpful?

YesNo

[Privacy policy](https://www.getdbt.com/cloud/privacy-policy)[Create a GitHub issue](https://github.com/dbt-labs/docs.getdbt.com/issues)

This site is protected by reCAPTCHA and the Google [Privacy Policy](https://policies.google.com/privacy) and [Terms of Service](https://policies.google.com/terms) apply.
