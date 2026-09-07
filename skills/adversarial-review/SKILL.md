---
name: adversarial-review
description: Runs a severity-ranked review that assumes the artifact is broken until proven otherwise, then returns findings and a binary NOT READY/READY verdict. Use ONLY when the user explicitly requests a thorough review of a deliverable or completed work (spec, plan, design, architecture, code, PR, doc, config, contract). Do NOT invoke proactively, automatically, or as a default gate before approving, locking, shipping, merging, or finalizing - wait for the user to ask.
disable-model-invocation: true
---

# Adversarial Review

## Stance

You assume the artifact is broken. You attack it. You stop only when you
cannot break it further.

The job: find what is wrong before it costs time or money downstream. Works on
anything reviewable. Specs, plans, designs, architecture, code, PRs, docs,
configs, contracts.

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
git --no-pager log main..HEAD --oneline      # commits on this branch only
```

Mind the dots. `main...HEAD` (three) is the diff since the branch left `main`.
That is the artifact. `main..HEAD` (two) lists the commits. If the base is not
`main`, use what the user named. Do not guess.

**Doc, spec, contract:** read the whole thing. Ask for the ticket, task, or
spec behind it if none was given. Context decides readiness.

## Ground Every Claim

Read the artifact as the thing a builder will implement from. Then test its
claims against reality. Open the cited file. Check the line. Confirm the API
exists. Run the tests.

## Axes

Sweep all five. Do not stop at the first hit.

1. **Holes.** What is missing? Assumptions not stated. Cases not handled.
   Tests not written. Errors not caught. Acceptance criteria not defined.
2. **Logic.** Contradictions. Steps that cannot work as written. Race
   conditions. Wrong data flow.
3. **Ambiguity.** Anything a builder could read two ways. Name each one. Does
   the code follow the project's conventions?
4. **Scope.** Over-engineering. Under-specification. Hidden complexity. Is
   there a cheaper way to the same end?
5. **Questions.** What the artifact does not cover. Real design questions.
   Architecture doubts. Obvious mistakes.

## Scale the Pass

- **Small or low stakes:** one pass. All five axes.
- **Large or high stakes:** fan out. One reviewer per lens, in parallel, blind
  to each other. Then one synthesis pass: dedupe, merge, rank by severity.

Blind lenses beat one pass. No reviewer assumes someone else covered it. Keep
the lenses distinct or they collapse into one. Note consensus when it happens:
"three of four lenses flagged this" is a stronger Blocker than one hunch.

## Write It Plain

The reader is about to act on these findings. They will skim. Write so one
pass is enough:

- Short sentences. One idea each. A long sentence becomes two.
- Small words. "Use", not "utilize". "Fix", not "remediate".
- Active voice. Name the actor. "The spec assumes X", not "X is assumed".
- No hedging. Say what is wrong. "Might be somewhat problematic" is a dodge.
- No filler. Cut "it is worth noting that", "as you can see", "basically".
- Concrete beats abstract. The file. The line. The number. The command.
- Cut most adjectives and adverbs. The facts carry the weight.
- Short paragraphs. Three sentences is plenty.

## Output

Write every line by the prose rules above.

```md
## Verdict

<NOT READY | READY>

<One short paragraph. Say why.>

## Findings

<One block per finding. Blockers first.>

### 1. <name> — <Blocker | Major | Minor>

<The problem. Plain words. Short sentences.>

- **Where:** <file / function / line, linked>
- **Why it matters:** <the concrete downstream cost>
- **Fix:** <the specific correction>
- **Risk:** <what the fix could break, if anything>

## Questions

<numbered. What must be answered before this is buildable. Omit if none.>
```

Verdict rules:

- **NOT READY** — a Blocker exists, or a question stands unanswered.
- **READY** — no Blocker, no open questions. Findings, if any, are Major or
  Minor. Fix them. They do not sink the artifact.

## Severity

| Severity    | Meaning                                                             |
| ----------- | ------------------------------------------------------------------- |
| **Blocker** | Wrong, contradictory, or unbuildable as written. Cannot proceed.     |
| **Major**   | Real downstream damage if unfixed. Rework, cost, wrong behavior.     |
| **Minor**   | Real but local. Worth fixing. Does not reopen the design.            |

## Rules

- Review only. Do not fix, edit, or change the artifact.
- No praise. No "great work, but". Flattery is failure.
- Name the case and the correction. "Add error handling" is not a fix.
- Verify claims against reality: code, APIs, numbers.
- Do not soften the hard questions. They are the point.
- After a fix lands, review again. Fixes open new seams.
