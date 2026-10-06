# coding-standards

My personal coding tools and standards: agent skills I find useful and reusable project configurations.

## Layout

```
skills/          Agent skills, one folder per skill (installable with `npx skills`)
  <name>/
    SKILL.md     Skill definition (YAML frontmatter + instructions)
configs/         Project configurations, one folder per config, each with its own README
```

## Skills

| Skill | Description |
| --- | --- |
| [code-quality](skills/code-quality/SKILL.md) | Language-agnostic principles for simple, explicit, readable, testable code. |

### Installing

List the skills in this repo:

```bash
npx skills add samuelkitazume/coding-standards --list
```

Install a specific skill:

```bash
npx skills add samuelkitazume/coding-standards --skill code-quality
```

Install everything:

```bash
npx skills add samuelkitazume/coding-standards
```

Add `-g` to install globally (user level) instead of into the current project.

### Adding a skill

1. Create `skills/<name>/SKILL.md`. The folder name must match the `name` field in the frontmatter.
2. The frontmatter needs at least `name` and `description`:

   ```markdown
   ---
   name: my-skill
   description: What the skill does and when the agent should use it.
   ---
   ```

3. Put any supporting files (scripts, references, templates) inside the same folder.
4. Add a row to the table above.

## Configs

Each config lives in `configs/<name>/` with a `README.md` explaining what it is, which files to copy, and where they go in a target project.

_None yet._
