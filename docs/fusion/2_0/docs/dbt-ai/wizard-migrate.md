# Migrate to dbt Wizard

Move from Claude Code to dbt Wizard while keeping your project conventions.

dbt Wizard automatically imports Claude Code instructions, skills, and settings from your repo. Use this page if you already use Claude Code on dbt projects and want to bring over project context, skills, or model settings. If you're new to AI agents, start with the [Quickstart](https://docs.getdbt.com/docs/dbt-ai/wizard-quickstart.md).

## Prerequisites[​](#prerequisites "Direct link to Prerequisites")

You'll need:

* [dbt Wizard CLI](https://docs.getdbt.com/docs/dbt-ai/wizard-cli.md) installed or dbt Wizard enabled in the [dbt platform](https://docs.getdbt.com/docs/platform/wizard-platform.md)
* [BYOK](https://docs.getdbt.com/docs/dbt-ai/wizard-byok.md) configured for a supported CLI provider (OpenAI, Anthropic, AWS Bedrock, or Snowflake Cortex in preview)
* Any existing Claude Code files you want to migrate, such as `CLAUDE.md`, `.claude/CLAUDE.md`, or `.claude/skills/`

## Review project context and skills[​](#review-project-context-and-skills "Direct link to Review project context and skills")

dbt Wizard automatically reads common agent instruction files and skills from your repo. You usually don't need to move existing project context before using dbt Wizard.

| File or folder                           | What Wizard does                                               | Migration needed? |
| ---------------------------------------- | -------------------------------------------------------------- | ----------------- |
| `AGENTS.override.md` at the project root | Checks this first for local override instructions.             | No                |
| `AGENTS.md` at the project root          | Uses this as the primary project instruction file.             | No                |
| `CLAUDE.md` at the project root          | Uses this as a built-in fallback when `AGENTS.md` is absent.   | No                |
| `.claude/CLAUDE.md`                      | Uses this as another fallback instruction file.                | No                |
| `.claude/skills/NAME/SKILL.md`           | Auto-discovers these as skills.                                | No                |
| Reusable prompts or new skills           | Place them in `.agents/skills/NAME/SKILL.md` at the repo root. | Yes               |

dbt Wizard also walks the directory tree from your project root to your current working directory, picks up instruction files at each level, and combines them for the session.

For the full skill format, refer to [Skills](https://docs.getdbt.com/docs/dbt-ai/wizard-skills.md).

### Convert reusable prompts to skills[​](#convert-reusable-prompts-to-skills "Direct link to Convert reusable prompts to skills")

Use skills for reusable workflows or specialized instructions, not for general project context that already lives in `AGENTS.md` or `CLAUDE.md`. Each repo-level skill lives in `.agents/skills/SKILL_NAME/SKILL.md`.

```bash
mkdir -p .agents/skills/my-team-style
touch .agents/skills/my-team-style/SKILL.md
```

Then add frontmatter and instructions:

```markdown
---
name: my-team-style
description: Apply team-specific dbt modeling conventions when creating, editing, refactoring, testing, or documenting models in this project.
---

# Team style

(add your reusable instructions here)
```

Write a specific `description:` because dbt Wizard uses it to decide when to load the skill.

## Configure model settings[​](#configure-model-settings "Direct link to Configure model settings")

Set provider credentials in [Configure BYOK](https://docs.getdbt.com/docs/dbt-ai/wizard-byok.md).

To set a default model across sessions, add this to `~/.dbt/wizard/config.toml`:

```toml
model = "claude-sonnet-4-6"   # use a model ID from `wizard debug models`
```

You can also pick a model in the TUI with `/model` without editing config or the `--config` flag. Run `wizard --help` to see all available flags.

## Verify the migration[​](#verify-the-migration "Direct link to Verify the migration")

From your dbt project root, start a new session:

```bash
wizard
```

Ask dbt Wizard to do something that should use your migrated skill, such as:

```text
create a new staging model for raw_invoices
```

And if your conventions aren't applied, check:

* General project context is in an instruction file such as `AGENTS.md` or `CLAUDE.md`
* Reusable skills are under `.agents/skills/` at the project root or `~/.agents/skills/` for CLI-global skills
* The skill has a clear `description:`
* You started a new session after adding the skill

## Related docs[​](#related-docs "Direct link to Related docs")

* [Quickstart](https://docs.getdbt.com/docs/dbt-ai/wizard-quickstart.md)
* [Skills](https://docs.getdbt.com/docs/dbt-ai/wizard-skills.md)
* [Configure BYOK](https://docs.getdbt.com/docs/dbt-ai/wizard-byok.md)
* [Use cases and examples](https://docs.getdbt.com/docs/dbt-ai/wizard-use-cases.md)
