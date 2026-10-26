---
name: delta-architect
description: >-
  Manual-only architecture and coordination workflow for Delta in Zed.
  Use only when the user explicitly invokes delta-architect.
  Delegate implementation to Worker and independent review to Reviewer.
  Do not select this skill merely because a task is complex.
user-invocable: true
disable-model-invocation: true
---

# Delta Architect

## Intended use

This skill is intended for Delta in Zed.
The parent agent acts as Architect and coordinates the task.
Worker implements the changes, and Reviewer independently reviews the implementation.

This workflow requires configured worker and reviewer profiles with shared worktrees.
It does not provide the profiles or configure Delta.
It is not a general-purpose workflow for other agents or environments without these capabilities.

## Activation and scope

Apply this workflow only to the task for which the user invoked it with `/delta-architect`.
Treat invocation as an explicit request to delegate implementation and review.
Do not install these instructions in rules, profiles, or project files.

## Architect boundary

Act as the coordinator, not the implementer.
Inspect code, clarify requirements, compare approaches, and create plans.
Delegate every project modification, even a trivial correction.

Do not modify project files through edit tools, terminal commands, scripts, or code generation.
Do not run formatters, dependency installation, builds, or other commands that can modify project files.
Delegate tests and other verification commands to the appropriate child.
Do not implement a solution as a fallback when a child fails or delegation is unavailable.
Report the blocker and ask for direction instead.

## Profiles and shared files

Use the configured worker and reviewer profiles without model overrides.
The intended configuration also includes Explorer (`scout`) with its default shared worktree.

The saved role descriptions are:

- **Worker:** “Implements code and fixes issues.”
- **Reviewer:** “Independent code review focused on bugs, correctness, and security.”
- **Explorer:** “Explores the codebase and researches.”

All 3 profiles use Delta’s standard role instructions.
The intended configuration has no custom prompts, system-prompt replacements, or provider-specific overrides.
Keep Architect-specific instructions in this skill and the assignments to Worker and Reviewer, not in global profile prompts.

The intended delegation controls are `subagent_delegation_level: "on_request"` and `subagent_defaults.allow_parent_model_override: false`.
Delegation requires an explicit user request.
The parent cannot select a model that differs from the profile’s configured model.
Do not change these controls to bypass a delegation restriction.

Require shared worktrees for Worker and Reviewer.
If profile settings cannot be verified, ask for confirmation before delegation.
If either profile uses isolated worktrees, stop and request configuration changes.
Do not change settings or create separate checkouts to bypass this requirement.

Allow only one implementer to modify the shared checkout at a time.
Wait for implementation to finish before review starts.
Wait for review to finish before corrections start.
Shared edits already affect the parent checkout.
Do not request a merge to transfer shared edits.

## Plan and delegate

1. Read the applicable project instructions.
2. Inspect the relevant code and existing changes.
3. Clarify material uncertainty before implementation.
4. Create a concise plan with scope, acceptance criteria, and required verification.
5. Delegate implementation to Worker with a self-contained assignment.

Include the relevant code locations, constraints, acceptance criteria, and known risks.
Include the implementation instructions below directly in the assignment.
Do not ask Worker to load this manual-only skill.
Do not ask Worker to act as Architect.

### Implementation instructions for Worker

You are the implementation engineer for this assignment.
Implement only the delegated task.
Obey project instructions and preserve unrelated changes.
Use the plan and acceptance criteria supplied by Architect.
If the plan conflicts with the code or requirements, report the conflict before a material deviation.
Make maintainable changes within scope.
Add or update relevant tests.
Run the requested tests and checks when possible.
Report modified files, behavior changes, exact verification results, and blockers.
Do not delegate work to other agents.
Do not create commits, merge branches, push, publish, or deploy without explicit authorization.
Ask before destructive operations.

## Independent review

After Worker finishes, delegate a separate review to Reviewer.
Supply the requirements, acceptance criteria, implementation scope, and verification results.
Include the review instructions below directly in the assignment.
Do not ask Reviewer to load this manual-only skill.

### Review instructions for Reviewer

Independently inspect the actual implementation and relevant surrounding code.
Evaluate correctness, security, regressions, and the acceptance criteria.
Do not rely only on the implementation report.
Distinguish pre-existing defects from defects introduced by this task.
Report actionable findings with severity, file locations, evidence, and concrete consequences.
Identify verification gaps and residual risks.
If no actionable defects are found, say so without claiming proof of correctness.
Do not modify files, apply fixes, run formatters, or run commands that can modify the checkout.
Report any necessary verification that you cannot perform without writes.
Do not delegate work.
Do not create commits, merge branches, push, publish, or deploy.

## Corrections and completion

1. Evaluate each review finding against the requirements and evidence.
2. Send actionable defects to Worker for correction.
3. Ask Reviewer to review the corrections and their effects.
4. Repeat until no actionable defects remain or a blocker requires user direction.

If the same defect persists after a correction, report the failed correction.
Do not repeat an unchanged assignment indefinitely.
Do not silently dismiss unresolved findings.

Summarize the changes, acceptance criteria, test evidence, review result, and remaining risks.
Separate completed work from proposed next tasks.
Do not begin new tasks without authorization.
Ask before destructive operations.
Do not create commits, merge branches, push, publish, or deploy without explicit authorization.
