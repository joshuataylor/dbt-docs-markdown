# Installing agent skills from dbt packages

Available in v2 | Local development

Ship a set of skills in a dbt package, and `dbt deps` installs them into the directory your coding agent reads from. Your whole team picks up the same skills from a versioned dependency instead of managing them separately.

A skill is a directory containing a `SKILL.md` file that gives your coding agent reusable instructions, in the [Agent Skills](https://agentskills.io/specification) format. Any dbt project can ship skills, and any project that installs that project as a package can install its skills.

Let's say your team has rules about how to name models or where tests go. You can write those rules down as a skill *once*, ship them in a package, and everyone's coding agent picks them up the next time they run `dbt deps`. This means everyone on the team gets the same rules, and you only update them in one place.

Agent skills install locally where your coding agent runs. Skills install only when you configure an `ai_provider`. Without one, `dbt deps` installs your packages as usual and leaves your agent directories untouched.

## Set the ai\_provider flag

Set [`ai_provider` in the `flags`](../../reference/global-configs/about-global-configs.md?version=2#available-flags) block of your root project to tell dbt which directory to write skills to. For example, if you use claude as your AI provider, you'd set it as such:

dbt\_project.yml

```yml
flags:
  ai_provider: claude
```

Report incorrect code

Each provider has a directory it reads skills from. Most providers share `.agents/skills`; claude reads skills via its own directory. Supported providers are:

| `ai_provider` value | Installs to      |
| ------------------- | ---------------- |
| `wizard`            | `.agents/skills` |
| `claude`            | `.claude/skills` |
| `openai`            | `.agents/skills` |
| `codex`             | `.agents/skills` |
| `cursor`            | `.agents/skills` |
| `gemini`            | `.agents/skills` |

Values are case-insensitive, so `wizard`, `Wizard`, and `WIZARD` all resolve the same way.

If you use dbt Wizard, set `wizard` even if you [bring your own key](./wizard-byok.md) or use dbt-managed AI.

You can also list more than one provider, which installs the same skills into each provider's directory:

dbt\_project.yml

```yml
flags:
  ai_provider:
    - wizard
    - claude
```

Report incorrect code

dbt resolves `ai_provider` from three places, in order of precedence:

1. The `--ai-provider` command-line option, which accepts a comma-separated list, for example `dbt deps --ai-provider claude,cursor`.
2. The `DBT_ENGINE_AI_PROVIDER` environment variable.
3. The `flags` block in your root project's `dbt_project.yml`, as shown previously.

Only the root project's `flags` block is read. Setting `ai_provider` in a package has no effect, because the choice of coding agent belongs to the user, not to the package author.

If your project or its packages ship skills and `ai_provider` isn't set, `dbt deps` succeeds and warns that it found skills but installed none. For example:

```text
[warning] [AiProviderUnset (dbt1801)]: Found 3 agent skill(s) in this project and its packages, but 'ai_provider' is not set, so none were installed. Set it in dbt_project.yml (flags: {ai_provider: claude}), via --ai-provider, or with DBT_ENGINE_AI_PROVIDER. Known providers: wizard, claude, openai, codex, cursor, gemini.
```

Report incorrect code

## Ship skills in a package

Create one directory per skill under your project's `skills` directory, each containing a `SKILL.md` file:

```text
skills/
  naming-conventions/
    SKILL.md
  add-a-data-test/
    SKILL.md
```

Report incorrect code

To keep skills somewhere other than `skills`, set [skill-paths](../../reference/project-configs/skill-paths.md).

Each `SKILL.md` needs YAML frontmatter with a `name` that matches its directory name, and a `description` that tells the agent when to apply the skill:

skills/naming-conventions/SKILL.md

```markdown
---
name: naming-conventions
description: Use when creating or renaming a model, so that staging, intermediate, and mart models follow the house naming convention.
---

# Naming conventions

Apply these prefixes when adding a new model:

- Staging models use the prefix `stg_` and live in `models/staging/`.
- Intermediate models use the prefix `int_` and live in `models/intermediate/`.
- Mart models use no prefix and are named for the business concept they expose.
```

Report incorrect code

For more detail on writing effective skills, refer to [Skill file format](./wizard-skills.md#skill-file-format).

dbt reads a package's `SKILL.md` files and never modifies them. Only the installed copy is written to.

A `SKILL.md` that dbt can't read — because it's missing frontmatter, or its `name` doesn't match its directory — produces a warning and is skipped. One malformed skill in a package doesn't stop the rest from installing.

## Install the skills

Add the package to your project as you would any other dependency:

packages.yml

```yaml
packages:
  - local: "../packages/demo_skills"
```

Report incorrect code

Then run `dbt deps`. dbt reports each skill it writes:

```shell
dbt deps
```

Report incorrect code

```text
Installing packages
Installing demo_skills
 Installed demo_skills
 Installed 1 package
Installing add-a-data-test (demo_skills) -> .agents/skills
Installing naming-conventions (demo_skills) -> .agents/skills
```

Report incorrect code

With more than one provider set, dbt writes a copy per directory:

```text
Installing add-a-data-test (demo_skills) -> .agents/skills
Installing add-a-data-test (demo_skills) -> .claude/skills
```

Report incorrect code

dbt installs skills whenever it installs packages, so `dbt build`, `dbt run`, and `dbt parse` install them too. You don't have to run `dbt deps` yourself.

dbt collects skills from your root project as well as from every installed package, including packages installed as transitive dependencies. A `skills` directory in your own project installs alongside the ones your packages ship.

Re-running a command when nothing has changed writes nothing and reports nothing. dbt rewrites the installed copy only when the source has changed.

## Understand how dbt tracks installed skills

When dbt installs a skill, it records what it did in the installed copy's frontmatter, under the `metadata` field:

.agents/skills/naming-conventions/SKILL.md

```markdown
---
name: naming-conventions
description: Use when creating or renaming a model, so that staging, intermediate, and mart models follow the house naming convention.
metadata:
  dbt.managed_by: dbt
  dbt.source: package
  dbt.package: demo_skills
  dbt.source_path: skills/naming-conventions
  dbt.source_hash: sha256:fbdd25363736f004d0599b754323c661ddf79d8ffb76f54c0b8c263e7bc52def
  dbt.installed_at: 2026-09-09T19:21:14.595255+00:00
---
```

Report incorrect code

That record is the only way dbt recognizes a skill as its own, which has three consequences worth knowing:

* **Skills you write by hand are never touched.** dbt only overwrites or removes a skill directory whose `SKILL.md` carries `dbt.managed_by: dbt`. Your own skills can sit in the same directory safely.
* **Removing the record makes dbt forget the copy.** If you delete the `dbt.` entries from an installed skill, dbt no longer recognizes it and leaves it alone from then on, including during [dbt clean](../../reference/commands/clean.md).
* **Modifications to skills are overwritten.** dbt always overwrites skill directories whose `SKILL.md` carries `dbt.managed_by: dbt`. To permanently modify a skill installed via a package, remove the dbt metadata within the frontmatter of the `SKILL.md`.

If the skill's author included their own `metadata` entries, dbt preserves them and adds its own entries after them.

`dbt.version` records the version dbt resolved for the package. Local and tarball packages have no pinned version, so the field is absent for those; `dbt.source_hash` still identifies the exact content that was installed.

## Disable a skill

Use the `skills` config in your root project, keyed by the package that ships the skill:

dbt\_project.yml

```yml
skills:
  demo_skills:
    naming-conventions:
      +enabled: false
```

Report incorrect code

A package's own `skills` config sets the defaults for the skills it ships, and your root project's `skills` config overrides it. This works the same way as [enabling and disabling other resources](../../reference/resource-configs/enabled.md).

## More examples

Set `+enabled` at the package level to control a whole package, or nest it under a skill name to control a single skill.

Disable every skill from one package, and leave all others enabled:

dbt\_project.yml

```yml
skills:
  demo_skills:
    +enabled: false
```

Report incorrect code

Enable skills from only one package, and disable them from all others:

dbt\_project.yml

```yml
skills:
  +enabled: false
  demo_skills:
    +enabled: true
```

Report incorrect code

Enable just two skills, and disable everything else:

dbt\_project.yml

```yml
skills:
  demo_skills:
    +enabled: false
    naming-conventions:
      +enabled: true
    adding-exposures:
      +enabled: true
```

Report incorrect code

Disable just two skills, and leave everything else enabled:

dbt\_project.yml

```yml
skills:
  demo_skills:
    naming-conventions:
      +enabled: false
    adding-exposures:
      +enabled: false
```

Report incorrect code

Skill names must be unique

Because skills install under their own name, two enabled skills with the same name would occupy the same directory. Rather than choose between them, dbt fails the command before writing anything:

```text
Two or more agent skills are named 'naming-conventions' (this project, package
'demo_skills'), so they would install into the same directory. Skill names must
be unique across your project and its packages.
```

Report incorrect code

Resolve it the same way you'd resolve duplicate model names: disable all but one with `+enabled: false`, or ask the package maintainer to rename theirs.

## Remove installed skills

[dbt clean](../../reference/commands/clean.md) removes the skills dbt installed and leaves everything else in place:

```shell
dbt clean
```

Report incorrect code

```text
  Removing add-a-data-test -> .agents/skills
  Removing naming-conventions -> .agents/skills
```

Report incorrect code

## Related docs

* [Use skills with dbt Wizard CLI](./wizard-skills.md)
* [Packages](../build/packages.md)
* [skill-paths](../../reference/project-configs/skill-paths.md)
* [About dbt deps command](../../reference/commands/deps.md)
* [About dbt clean command](../../reference/commands/clean.md)
