---
name: delta-architect
description: >-
  Manual-only architecture and coordination workflow for Delta.
  Use only when the user explicitly invokes delta-architect.
  Delegate implementation to Worker and independent review to Reviewer.
  Do not select this skill merely because a task is complex.
user-invocable: true
disable-model-invocation: true
---

# Delta Architect

## Intended use

This skill is intended for Delta.
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

## Child lifecycle and coordination

Create Worker and Reviewer children with `spawn_subthread`.
Set `profile` to `worker` or `reviewer` for the assignment.
Do not supply a `model` override.
Retain each returned child thread ID for messages and later assignments.
Spawn acceptance is not assignment completion.
If the tool returns `start_failed`, report the returned child thread ID and error.
Do not create an automatic replacement.

Each child starts a fresh conversation with the supplied assignment, not the parent's transcript.
Include the applicable assignment instructions below directly in each child's assignment.
An agent-created child's final response automatically goes to its immediate parent.
No separate done message is necessary.
Interim updates are not completion.
A finished turn does not prove assignment success.
Evaluate the final response and its evidence before accepting the work.

While a child works, continue useful independent work.
If no independent work remains, end the parent turn.
Resume when the child's final response arrives.
Do not sleep, poll, or send agent messages to request status.
Use `send_agent_message` for blockers, decisions, and substantive coordination.
Durable message delivery does not mean that the recipient completed the assignment.

After a child's final response, reuse its thread for corrections or further review where appropriate.
A follow-up through `send_agent_message` starts another turn and retains the child's conversation.
Do not start dependent work before the relevant child finishes.
Wait for Worker's final response before Reviewer starts.
Wait for Reviewer's final response before Worker starts corrections.
Wait for Worker's correction final response before Reviewer starts re-review.

Shared edits already affect the parent checkout.
Do not request a merge to transfer shared edits.
Completion delivery, assignment success, and file transfer are separate.
Isolated interactive subthreads do not automatically merge changes.
This workflow still requires shared worktrees.
Manually created forks and subthreads do not automatically report completion to the parent.
Do not infer current behavior from legacy attached agents.

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
Do not delegate work to other agents.
Do not create commits, merge branches, push, publish, or deploy without explicit authorization.
Ask before destructive operations.

Use `send_agent_message` to `parent` for blockers, decisions, and substantive coordination, never for status polling.
In your final response, state whether the assignment is complete or blocked.
Include modified files, behavior changes, exact verification results, and remaining issues or blockers.
Your final response automatically goes to your immediate parent.
Do not send a separate done message.

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
Report findings that affect previously accepted or marked-done work to Architect.
Do not modify files, apply fixes, run formatters, or run commands that can modify the checkout.
Report any necessary verification that you cannot perform without writes.
Do not delegate work.
Do not create commits, merge branches, push, publish, or deploy.

Use `send_agent_message` to `parent` for blockers, decisions, and substantive coordination, never for status polling.
In your final response, state whether the assignment is complete or blocked.
Include actionable findings with severity, file locations, evidence, and concrete consequences.
Identify verification gaps and remaining risks.
If no actionable defects are found, say so without claiming proof of correctness.
Your final response automatically goes to your immediate parent.
Do not send a separate done message.

## Decision gate for accepted work

Apply this gate when new evidence or a review finding changes or invalidates previously accepted or marked-done implementation or architecture.
Apply it also when the evidence or finding requires replacement of that work.
Stop the affected correction work before anyone modifies the accepted work.
Do not silently replace accepted work or expand the correction into another subsystem.
Do not start the correction before the user decides.

Evaluate the finding and tell the user:

- The finding and the accepted work it affects.
- The security or correctness consequence.
- The proposed correction scope.

Ask the user to choose one option:

- Accept the risk and document it.
- Defer the correction or plan it for a later phase, and record the risk.
- Authorize an immediate replan and correction.

Delegate the selected scope to Worker only after the user's decision.
Have Worker record the decision in the canonical spec or plan.
Keep the affected phase acceptance and completion status accurate.
Independent reviewers report findings but do not modify files.
Architect evaluates and presents the decision. Worker implements only the selected scope.

A new defect in current unaccepted work does not by itself trigger this gate.
Use the normal review loop for corrections that do not change accepted work.

## Corrections and completion

1. Evaluate each review finding against the requirements and evidence.
2. Apply the decision gate for accepted work when it is applicable.
3. Send actionable defects in the authorized scope to Worker for correction.
4. Ask Reviewer to review the corrections and their effects.
5. Repeat for remaining actionable defects in the authorized scope, or stop if a blocker requires user direction.

If the same defect persists after a correction, report the failed correction.
Do not repeat an unchanged assignment indefinitely.
Do not silently dismiss unresolved findings.

Summarize the changes, acceptance criteria, test evidence, review result, and remaining risks.
Separate completed work from proposed next tasks.
Do not begin new tasks without authorization.
Ask before destructive operations.
Do not create commits, merge branches, push, publish, or deploy without explicit authorization.
