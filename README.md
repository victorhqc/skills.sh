# skills

[![skills.sh](https://skills.sh/b/victorhqc/skills.sh)](https://skills.sh/victorhqc/skills.sh)

Agent skills for AI coding agents, following the open
[Agent Skills](https://agentskills.io) specification. Published on
[skills.sh](https://skills.sh).

## Skills

| Skill | Description |
| ----- | ----------- |
| [adversarial-review](skills/adversarial-review/) | Reviews a deliverable when the user asks for a thorough review. Ranks defects by severity and gives a READY or NOT READY result. Reports problems only and does not fix them. |
| [delta-architect](skills/delta-architect/) | Coordinates architecture, implementation, and independent review in Delta in Zed. Requires explicit invocation and configured worker and reviewer profiles with shared worktrees. |
| [minimal-comments](skills/minimal-comments/) | Comments are a last resort — the code is the comment. Enforces a minimal-comment policy in any language, with an exhaustive list of the only valid uses for a comment. |
| [pr-description](skills/pr-description/) | Writes or fills in a pull request body: a short Summary first, a Mermaid flow diagram of the change, and a reading order of the files that follows the data flow. Uses the repo's PR template when it has one. Opens or updates the PR with `gh` after you confirm, or gives you a markdown block to paste. |
| [simplified-technical-english](skills/simplified-technical-english/) | Applies ASD-STE100 Issue 9 rules to technical prose. Preserves facts, uncertainty, and obligations. Flags words that need dictionary verification. |

## Install

Use the [skills CLI](https://skills.sh) to install into your agent
(Claude Code, Cursor, Codex, Zed, OpenCode, and 70+ others):

```bash
# All skills in this repo
npx skills add victorhqc/skills.sh

# A single skill
npx skills add victorhqc/skills.sh --skill minimal-comments
```

Install globally (available in every project) with `--global`, or into a
specific agent with `--agent <name>`:

```bash
npx skills add victorhqc/skills.sh --skill minimal-comments --global --agent claude-code
```

## Use delta-architect in Delta

`delta-architect` is intended for Delta in Zed, not for general use in every
agent supported by the skills CLI.

The skill does not create profiles or configure Delta.

### Intended configuration

#### Profiles

| Setting | Worker | Reviewer | Explorer (`scout`) |
| --- | --- | --- | --- |
| Worktree | **Shared** | **Shared** | Default: shared |
| Custom prompt | None | None | None |
| System-prompt replacement | None | None | None |
| Provider-specific overrides | None | None | None |

The profiles have these saved role descriptions:

- **Worker:** “Implements code and fixes issues.”
- **Reviewer:** “Independent code review focused on bugs, correctness, and security.”
- **Explorer:** “Explores the codebase and researches.”

The profiles use Delta’s standard role instructions. Architect-specific
instructions belong in the `delta-architect` skill and its assignments to Worker
and Reviewer, not in global profile prompts.

Shared worktrees let Worker and Reviewer use the parent’s checkout. No merge
or transfer of changes is necessary.

#### Delegation controls

```json
{
  "subagent_delegation_level": "on_request",
  "subagent_defaults": {
    "allow_parent_model_override": false
  }
}
```

Delegation requires an explicit user request. The parent cannot select a model
that differs from the profile’s configured model. Invocation of this skill is
the request to delegate implementation and independent review.

### Invocation

Invoke the skill with `/delta-architect` and the task requirements. For example:

```text
/delta-architect Add pagination to the orders endpoint.

Requirements:
- Preserve existing filtering and sorting behavior.
- Default to 25 results and enforce a maximum of 100.
- Add tests for pagination boundaries and invalid parameters.

Use Worker for all implementation and Reviewer for independent review.
Do not commit, push, or deploy.
```

The parent agent acts as Architect. It inspects the project and prepares a plan,
but delegates all project changes and tests to Worker. Reviewer independently
reviews the implementation. Architect coordinates corrections before completion.
Implementation, review, and corrections run in sequence because the agents share
the checkout.

If delegation is unavailable or either profile uses an isolated worktree, the
workflow stops. Architect does not implement the changes as a fallback.

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
