---
name: minimal-comments
description: |-
  Enforce a minimal-comment policy whenever writing, editing, or reviewing ANY code in ANY language — new files, functions, tests, configs, scripts, queries. Comments are a last resort: the code itself must carry the meaning. Only allow comments that state a non-obvious domain or business rule, an external system's quirk or contract, a deliberate deviation from expectations, or the why behind a necessary suppression or cast.
license: MIT
metadata:
  author: Victor Quiroz
  version: "1.0.0"
---

# Minimal Comments

## The rule

**Comments are a last resort — the code is the comment.** Most functions,
classes, and methods need zero comments. If you feel the urge to explain
*what* the code does, rename something or extract a function instead.

The default is: **no comment**. A comment is an admission that the code could
not carry the meaning on its own, so it must earn its place.

This applies to every language — TypeScript, JavaScript, Python, Ruby, Go,
Java, SQL, HCL, shell, YAML, CSS, and everything else. No exceptions.

## What to do instead of commenting

- Rename the variable, function, or class so the intent lives in the name.
- Extract a function or constant so the behavior is described by its name.
- Let types, signatures, and data structures carry the meaning.
- If a comment seems necessary, first ask: can the code say it better?

## The only valid uses

This list is exhaustive. If your comment does not fit one of these, delete it.

1. **A non-obvious domain or business rule** the code cannot express alone.
   Billing rules, dunning windows, money-handling invariants, security
   constraints — the *reason* the rule exists is not derivable from the code.
2. **An external system's quirk or contract**, stated as a fact about current
   behaviour: at-least-once delivery, gapless ordering, a third-party API's
   surprising behaviour, a framework quirk that would mislead the reader.
3. **A deliberate deviation** from what a competent reader would expect —
   a query shaped for the planner, an ordering that prevents a race, a bound
   that exists to keep an index in play.
4. **A necessary suppression or cast** (`// eslint-disable`, `@ts-ignore`,
   `# noqa`, `# type: ignore`, an `unknown` cast, etc.) — must always say
   *why* it is unavoidable.

## Comment hygiene

- Comments say **why**, never **what**. A comment that restates the code is
  deleted, not kept.
- Write comments like Hemingway: short sentences, plain words, one idea per
  line, active voice. If a comment needs stacked clauses to explain itself, split it or
  cut it.
  - Bad:  `// Due to the fact that retries may compound interest errors, we
    deliberately avoid re-running the charge when a partial capture exists.`
  - Good: `// Retries would double-charge. Skip if a partial capture exists.`
- State facts about current behaviour, not intentions or plans.
- One line when possible. No decorative banners, dividers, or ASCII art.
- No commented-out code — delete it, version control keeps the history.
- No stale comments: when the code changes, the comment changes or goes.
- TODOs name the concrete problem; they are not placeholders for lazy code.
