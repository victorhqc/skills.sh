# skills

Agent skills for AI coding agents, following the open
[Agent Skills](https://agentskills.io) specification. Published on
[skills.sh](https://skills.sh).

## Skills

| Skill | Description |
| ----- | ----------- |
| [minimal-comments](skills/minimal-comments/) | Comments are a last resort — the code is the comment. Enforces a minimal-comment policy in any language, with an exhaustive list of the only valid uses for a comment. |

## Install

Use the [skills CLI](https://skills.sh) to install into your agent
(Claude Code, Cursor, Codex, Zed, OpenCode, and 70+ others):

```bash
# All skills in this repo
npx skills add victorhqc/skills

# A single skill
npx skills add victorhqc/skills --skill minimal-comments
```

Install globally (available in every project) with `--global`, or into a
specific agent with `--agent <name>`:

```bash
npx skills add victorhqc/skills --skill minimal-comments --global --agent claude-code
```

## Add a skill to this repo

1. Create a directory under `skills/` named after the skill
   (lowercase letters, numbers, and hyphens — the directory name must match
   the `name` field in the frontmatter):

   ```
   skills/
   └── my-skill/
       └── SKILL.md
   ```

2. `SKILL.md` starts with YAML frontmatter (`name` and `description` are
   required) followed by the agent instructions:

   ```markdown
   ---
   name: my-skill
   description: What this skill does and when to use it.
   license: MIT
   metadata:
     author: Victor Quiroz
     version: "1.0.0"
   ---

   # My Skill

   Instructions for the agent.
   ```

3. Keep `SKILL.md` under ~500 lines; move deeper material into
   `references/`, `scripts/`, or `assets/` subdirectories if needed.

## License

[MIT](LICENSE)
