# skill-paths

dbt\_project.yml

```yml
skill-paths: [directorypath]
```

## Definition

Optionally specify a custom list of directories where [agent skills](../../docs/dbt-ai/package-skills.md) are located.

Every subdirectory containing a `SKILL.md` file is treated as one skill. dbt scans `skill-paths` in your root project and in each installed package when it installs packages.

## Default

By default, dbt looks for skills in a directory named `skills` in the root of your project.

Paths specified in `skill-paths` must be relative to the location of your `dbt_project.yml` file. Avoid using absolute paths like `/Users/username/project/skills`, as it will lead to unexpected behavior and outcomes.

* ✅ **Do**

  * Use relative path:

    ```yml
    skill-paths: ["skills"]
    ```

* ❌ **Don't**

  * Avoid absolute paths:

    ```yml
    skill-paths: ["/Users/username/project/skills"]
    ```

## Examples

### Use a subdirectory named `agent-skills` for skills

dbt\_project.yml

```yml
skill-paths: ["agent-skills"]
```

### Use more than one directory for skills

dbt\_project.yml

```yml
skill-paths: ["skills", "team-skills"]
```
