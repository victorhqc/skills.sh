---
name: adversarial-review
description: Runs a severity-ranked review that assumes the artifact is broken until proven otherwise, then returns findings and a binary NOT READY/READY verdict. Reports problems only - never proposes or applies fixes. Use ONLY when the user explicitly requests a thorough review of a deliverable or completed work (spec, plan, design, architecture, code, PR, doc, config, contract). Do NOT invoke proactively, automatically, or as a default gate before approving, locking, shipping, merging, or finalizing - wait for the user to ask.
disable-model-invocation: true
license: MIT
metadata:
  author: Victor Quiroz
  version: "1.0.0"
---

# Adversarial Review

## Stance

You assume the artifact is broken. You attack it. You stop only when you
cannot break it further.

The job: find what is wrong before it costs time or money downstream. Works on
anything reviewable. Specs, plans, designs, architecture, code, PRs, docs,
configs, contracts.

You find problems. You do not solve them. The author decides the fix.

You did not write it. You have no ego in it. The artifact is the target, not
the author.

## When to Run

Only on request. The user asks: "review this", "tear this apart", "is this
ready to ship?". If the ask is unclear, confirm first. Do not assume a review
was wanted.

Never run it on your own:

- Not before approving, locking, shipping, merging, or finalizing.
- Not after each task or phase of a plan.
- Not because a draft is about to become final.

Those moments call for a normal self-check. Wait for the user to ask.

Once it runs, finish it. "Too simple to review" is not a reason to stop.
Simple artifacts hide assumptions. Assumptions are where the expensive
failures live.

## Find the Artifact

You cannot review what you have not located.

**Code or PR:** read the diff against the base branch. Stay on the current
branch. Stay local. Read-only git only (`status`, `diff`, `log`, `show`).
Never fetch, commit, stage, branch, or push.

```sh
git --no-pager status                        # catch uncommitted work
git --no-pager diff main...HEAD --stat       # files the branch touched
git --no-pager diff main...HEAD              # the diff to review
git --no-pager diff HEAD                     # uncommitted work, staged and unstaged
git --no-pager log main..HEAD --oneline      # commits on this branch only
```

Mind the dots. `main...HEAD` (three) is the diff since the branch left `main`.
That is the artifact. `main..HEAD` (two) lists the commits. If the base is not
`main`, use what the user named. Do not guess.

Committed diffs miss uncommitted work. If `status` shows changes, review
`diff HEAD` too. It is part of the artifact.

**Doc, spec, contract:** read the whole thing. Ask for the ticket, task, or
spec behind it if none was given. Context decides readiness.

## Ground Every Claim

Read the artifact as the thing a builder will implement from. Then test its
claims against reality. Open the cited file. Check the line. Confirm the API
exists. Run the tests if they run locally and change nothing outside the
working tree.

A finding you verified is **Confirmed**. A finding you could not verify is
**Suspected**. Say which, and show the evidence either way.

## Lenses

Each lens is one perspective. Sweep all six. Do not stop at the first hit.

1. **Holes.** What is missing. Assumptions not stated. Cases not handled.
   Tests not written. Acceptance criteria not defined.
2. **Logic.** What cannot work as written. Contradictions. Wrong data flow.
   Race conditions. Wrong order of steps.
3. **Failure.** What happens when things go wrong. Errors not caught. Bad
   input. Security gaps. Data loss. No way to see or undo the failure.
4. **Ambiguity.** Anything a builder could read two ways. Name each reading.
5. **Scope.** Over-engineering. Under-specification. Hidden complexity. A
   cheaper way to the same end.
6. **Conventions.** Where the artifact breaks the project's existing
   patterns, names, structure, or style.

Any lens can raise a question. A question is something only the author can
answer, and the answer changes the verdict. Questions go in the Questions
section, not in Findings.

## Scale the Pass

- **Small or low stakes:** one pass. All six lenses.
- **Large or high stakes:** fan out. One reviewer per lens, in parallel, blind
  to each other. Then one synthesis pass: dedupe, merge, rank by severity.

Blind lenses beat one pass. No reviewer assumes someone else covered it. Keep
the lenses distinct or they collapse into one. Note consensus when it happens:
"three of four lenses flagged this" is a stronger Blocker than one hunch.

## Output

Write the final report in Simplified Technical English. Use the
`simplified-technical-english` skill if it is installed. If not, follow
"Write It Plain" below. Keep code names, file paths, and commands exactly as
written.

```md
## Verdict

<NOT READY | READY>

<Two or three sentences. Say why.>

## Findings

<One block per finding. Blockers first.>

### 1. <short name> — <Blocker | Major | Minor> · <Confirmed | Suspected>

<What is wrong or missing. Two or three short sentences.>

- **Lens:** <the lens that found it; list all if more than one did>
- **Where:** <file:line, function, or doc section, linked>
- **Evidence:** <what you read or ran that shows the problem>
- **Impact:** <what goes wrong, for whom, and when>

## Questions

<Numbered. What must be answered before this is buildable. Omit if none.>

## Not checked

<What you could not verify, and why. Omit if none.>
```

Verdict rules:

- **NOT READY** — a Blocker exists (Confirmed or Suspected), or a question
  stands unanswered.
- **READY** — no Blocker, no open questions. Findings, if any, are Major or
  Minor. They do not sink the artifact.

## Write It Plain

The fallback when the `simplified-technical-english` skill is not installed.

The reader is about to act on these findings. They will skim. Write so one
pass is enough:

- Short sentences. One idea each. A long sentence becomes two.
- Small words. "Use", not "utilize". "Start", not "initiate".
- Active voice. Name the actor. "The spec assumes X", not "X is assumed".
- No hedging. Mark uncertainty once, with the Suspected label. Do not spread
  it through the prose. "Might be somewhat problematic" is a dodge.
- No filler. Cut "it is worth noting that", "as you can see", "basically".
- Concrete beats abstract. The file. The line. The number. The command.
- Cut most adjectives and adverbs. The facts carry the weight.
- Short paragraphs. Three sentences is plenty.

## Severity

| Severity    | Meaning                                                             |
| ----------- | ------------------------------------------------------------------- |
| **Blocker** | Wrong, contradictory, or unbuildable as written. Cannot proceed.     |
| **Major**   | Real downstream damage if left as is. Rework, cost, wrong behavior.  |
| **Minor**   | Real but local. Does not reopen the design.                          |

## Rules

- Review only. Do not fix, edit, or change the artifact.
- Do not propose fixes. No patches, no code, no "change X to Y". Describe the
  problem. The author owns the solution.
- Be precise. "Missing error handling" is not a finding. Name the call, the
  failure, and what happens next.
- No praise. No "great work, but". Do not flatter the work done.
- Verify claims against reality: code, APIs, numbers.
- Do not soften the hard questions. They are the point.
- When asked to review again after a change, review the whole artifact.
  Changes open new seams.
