---
name: simplified-technical-english
description: |-
  Write or review technical prose with ASD-STE100 Simplified Technical English (Issue 9). Apply it to documentation, comments, tickets, procedures, PR text, and alerts. Preserve the source facts, uncertainty, and obligations. Verify uncertain word choices against the official dictionary before you claim STE compliance. This skill decides how text is worded, not whether it exists.
license: MIT
metadata:
  author: Victor Quiroz
  version: "1.0.0"
---

# Simplified Technical English

## The rule

Use ASD-STE100 Simplified Technical English (STE), Issue 9, for technical text.

STE is a controlled language. Aerospace writers use it so that a reader under
pressure, in a second language, cannot misread a procedure. Our readers are the
same: an on-call engineer at 04:00, a reviewer who does not know this service, a
person whose first language is not English.

The goal is **one reading only**. A sentence that permits two readings is a
defect, even when the two readings are true.

Rule numbers in brackets, for example (STE 3.6), point to the ASD-STE100 rule.

Preserve each fact, degree of certainty, and obligation in the source. Do not add
a measurement, date, actor, cause, result, or procedure step to make text more
specific. If the source lacks a necessary fact, ask for it or identify the gap.
If STE cannot express the same meaning, report the conflict and keep the source
meaning. Do not silently make advice mandatory or turn uncertainty into fact.

## Where this applies

Apply STE to:

- Code comments and TSDoc.
- ADRs, specs, and design documents.
- JIRA tasks, epics, and acceptance criteria.
- PR descriptions and commit message bodies.
- READMEs, runbooks, and migration notes.
- Gherkin steps and scenario names.
- Datadog monitor messages and alert text.
- All other Markdown.

Do **not** rewrite:

- Code. Identifiers obey the naming rules of the repo, not STE.
- Quoted text: log lines, stack traces, error strings, UI labels, third-party
  text, and the words of a person. STE keeps quoted text as it is (STE 8.6).
- Text that you do not otherwise edit. Do not open a file only to rewrite prose.
- Chat replies to the user. These are a conversation, not an artifact.

## 1. Words

**Use approved words only, with their approved meaning** (STE 1.1, 1.3). STE
approves a small set of words, and each approved word has one meaning. The
selected substitutions are in [the word-choice list](references/word-choice.md).
That list is not the STE dictionary. When approval, meaning, or part of speech
is uncertain, consult the official Issue 9 dictionary. If it is unavailable,
keep the meaning and state that you could not verify full STE compliance.

- `follow` means "come after". Write `obey the procedure`, not `follow the
  procedure`.
- `since` means "from that time". Write `because the lock is held`, not `since
  the lock is held`.
- `see` means "see with your eyes". Write `make sure that the job stopped`, not
  `see if the job stopped`.

**Use each word as its approved part of speech** (STE 1.2). In STE, `check` and
`test` are nouns, not verbs. Write `make sure that the flag is on` or `do a
check of the flag`, not `check the flag`. Write `run the tests`, not `test the
endpoint`.

**One item, one name** (STE 1.11, 9.4). Pick one term for a concept and repeat
it. Never use a synonym to avoid repetition. If you call an item a `queue`, do
not call the same item a `channel` or a `list` later.

**Use the verb for the action, not a noun** (STE 3.7). Write `the job removes
the column`, not `the job does the removal of the column`. Write `the log shows
3 retries`, not `the log gives an indication of 3 retries`.

**No phrasal verbs** (STE 9.3). A verb plus a preposition often has a second,
abstract meaning. Write `examine`, not `look into`. Write `start`, not `spin
up`. Write `remove`, not `clean up`.

**No idioms, slang, humor, or metaphor** (STE 1.10). "Kick off the job", "under
the hood", "nuke the cache", and "happy path" all go. Write what occurs.

**Use American English spelling** (STE 1.14): `behavior`, `color`, `analyze`.
Use a different spelling only when the repo style guide tells you to.

**Use gender-neutral words** (STE GR-7). Write `they` or `the engineer`, never
`he` or `she`.

## 2. Noun clusters

**A noun cluster has 3 words at most** (STE 2.1). `user account status event
handler` has 5. Break it with a preposition: `the handler for status events
from user accounts`.

**A long technical name stays whole** (STE 2.2). Write it in full the first
time. Then use a shorter form or an abbreviation that you give on first use.

**Hyphenate words that operate as one unit** (STE 2.2, 8.2): `grace-period
config`, `read-only replica`. A hyphenated group counts as 1 word.

## 3. Verbs

**Use only these verb forms** (STE 3.2):

- The infinitive (`to delete`) and the imperative (`delete the row`)
- The simple present, the simple past, and the simple future (`will delete`)
- The past participle as an adjective (`the archived group`).

No perfect tenses, no continuous tenses. Write `the job started at 03:00`, not
`the job has started` or `the job was starting`.

**No complex verb constructions** (STE 3.4). Write `you can change the limit`,
not `the limit can be changed`. Write `delete the row`, not `the row is to be
deleted`.

**Use the active voice** (STE 3.6). Name the actor. Write `the worker archives
the record`, not `the record is archived by the worker`. In an incident, the
actor is the whole question.

- In a procedure, the passive voice is never correct. Use the imperative.
- In a description, use the passive only when the actor is unknown. Or make
  `something` the actor: `Something deleted the row. We do not know which job.`
- A past participle that shows a condition is not passive (STE 3.3): `If the
  record is archived, skip it.`

**Do not use -ing forms** (STE 3.5). They read as a noun, an adjective, or a
tense. Write `to delete the row`, not `deleting the row`. Write `when the
consumer commits`, not `on committing`.

Exceptions: an -ing word in a technical name (`logging service`, `Troubleshooting`
as a heading), and the approved words `during`,
`something`, `missing`, `remaining`.

**Say obligation exactly.**

| Meaning     | Use                     | Replace only in this sense     |
| ----------- | ----------------------- | ----------------------------- |
| Required    | `must`                  | `shall`, `should`, `needs to`, `has to` |
| Able or permitted | `can`             | `may`, `might`, `could` |
| Forbidden   | `must not`, `do not`    | `should not` |
| Recommended | Name the source and use `recommend that ...` | `should` when it means advice |

First find what `should`, `may`, `might`, or `could` means in context. Do not
replace a possibility with an assertion of ability or permission. When the
degree of certainty is material, report a sentence that has no faithful STE
rewrite. In a procedure, do not put `must` before an instruction unless it is
for safety (STE 5.3).

## 4. Sentences

**One topic per sentence** (STE 4.1). If you write `and` or `which` to add a
second idea, split the sentence. Be specific: `the p99 latency is 800 ms`, not
`the endpoint is slow`.

**Do not omit words** (STE 4.2). Keep the articles, the subject, and the verb.
Write `the consumer reads the offset`, not `consumer reads offset`. Write `if
the flag is on, ...`, not `if on, ...`.

**No contractions** (STE 4.2). Write `do not`, `is not`, `cannot`.

**Use `that` after `make sure`, `show`, and `recommend`** (STE GR-1). Write
`make sure that the lag is 0`.

**Use connecting words** to link sentences (STE 4.4): `then`, `thus`, `but`,
`as a result`, `at the same time`.

**Use a vertical list** for more than 2 items, conditions, or steps (STE 4.3).
Put a colon before the list. Start each item with an uppercase letter. Do not
mix instructions and descriptions in one list.

**If a pronoun can refer to 2 nouns, repeat the noun** (STE GR-3, GR-4).

## 5. Procedures

Procedures are runbooks, migration steps, rollout plans, and numbered
instructions.

- **20 words at most in each sentence** (STE 5.1).
- **Start each step with the verb, in the imperative** (STE 5.3). `Stop the
  consumer.`
- **One instruction in each sentence** (STE 5.2), unless the actions occur at
  the same time: `Hold the lock and run the migration.`
- **Put the condition first**, then a comma (STE 5.4). Write `If the queue is
  empty, stop the worker.` Not `Stop the worker if the queue is empty.`
- **Put the result or the limit directly after the action**, in the same step.
  `Run the query. The count must be 0.`
- **Keep the steps in the order that the reader does them.**

**Notes give information, never instructions** (STE 5.5). A note (`> [!NOTE]`
or `NOTE:`) has no imperative, no requirement, and no limit. It obeys the
description limit of 25 words. Test: the reader must be able to do the
procedure without the notes. If not, move the information into a step.

## 6. Descriptions

Descriptions are ADR context, spec background, and comments that explain a rule.

- **25 words at most in each sentence** (STE 6.3).
- **Give information gradually** (STE 6.1). One subject for each sentence.
- **Start each paragraph with a topic sentence** (STE 6.4). One topic for each
  paragraph (STE 6.5).
- **6 sentences at most in each paragraph** (STE 6.6).
- **Repeat the key words** (STE 6.2). They connect the sentences.
- **No imperative.** A description gives information, not instructions.
- **Say why.** A description carries the reason that the code cannot.

## 7. Warnings and cautions

STE puts safety first (STE section 7). Our equivalent is the operation with
risk: a datafix, a delete, a production write, a migration without rollback.

| Word      | Use it when                                               |
| --------- | --------------------------------------------------------- |
| `WARNING` | Customers, money, or data can be lost, and you cannot restore them |
| `CAUTION` | Damage can occur, but you can repair it (downtime, a failed deployment) |

If the 2 risks occur together, use `WARNING` (STE 7.1). Keep these meanings
constant. GitHub shows `[!CAUTION]` in red and `[!WARNING]` in yellow. Do not
use the color to show the level.

- **Put the warning before the step it applies to.** Never after.
- **Start with a clear command or condition** (STE 7.2). `Take a snapshot of
  the table.`
- **Then give the risk** (STE 7.3). `You cannot restore the deleted rows.`
- **Give the scope of the risk in numbers** when you know it: rows, users,
  money.

## 8. Punctuation and word count

- **Do not use the semicolon** (STE 8.1). Write 2 sentences.
- **Use parentheses only** (STE 8.3) for an abbreviation, a reference, an
  identifier, a short explanation, an alternative, or a plural: `the job(s)`.
- Do not use `/` for "and" or "or". Write the word you mean.
- Write digits for quantities: `3 retries`.
- Give a number its applicable unit and basis when the source gives them. Do
  not invent a unit or a denominator.
- Write dates in full: `2026-09-22` or `22 September 2026`. Never `next
  Tuesday`, never `recently`.
- Expand each abbreviation on first use in each document. Do not use an
  abbreviation for a name of 3 words or fewer.

**Count words like STE** (STE 8.4–8.7). Each of these counts as 1 word:

- A number, or a number with its unit (`10 000 messages`, `15 min`)
- An abbreviation (`RDS`, `IOPS`)
- An identifier or a code span (`Worker42`, `retry_count`)
- Quoted text, a title, a heading, or a proper name
- A hyphenated group (`read-only`)
- Text in parentheses. That text also counts as its own sentence.

In a vertical list, the colon ends a sentence. Each item is a new sentence.

## 9. Technical names and technical verbs

STE lets a company approve technical nouns (STE 1.5, 1.8) and technical verbs
(STE 1.12) for a subject field. A name in code is a candidate, not automatic
approval. Use a project glossary when one exists. Make sure that each term fits
its stated meaning and context.

Common software technical nouns to evaluate: service and entity names from the code, `consumer`,
`topic`, `offset`, `migration`, `webhook`, `endpoint`, `schema`, `query`,
`cron job`, `feature flag`, `pull request`, `deployment`, `rollback`, `backup`.

Common software technical verbs to evaluate: `deploy`, `commit`, `merge`, `publish`, `consume`,
`run` (a script, query, or job), `install`, `upload`, `download`, `enable`,
`disable`, `validate`, `delete`, `restart`, `debug`, `process`.

Rules for them:

1. **Use the approved domain name consistently.** If `RetryPolicy` means
   retry policy in the source, use `retry policy` for that item.
2. **Do not give a second name** to an item that has one (STE 1.11).
3. **Do not use a technical name as a verb** (STE 1.7). Write `send a Slack
   message to the team`, not `Slack the team`.
4. **Do not use a technical verb as a noun** (STE 1.13). Write `the deployment
   failed`, not `the deploy failed`.
5. **Prefer an approved verb** when it gives the same meaning (STE 1.12). Write
   `find the defect`, not `detect the defect`.
6. **A technical name is not a license for a long sentence.** All other rules
   still apply.

## Other rules

An applicable project rule decides if a comment or document is necessary. This
skill decides how to word text that you write. Preserve correctness, security,
and confidentiality requirements when you apply STE.

## Before you finish

Read your text one time against this list. Correct each failure.

1. Can a sentence have two meanings? Write it again.
2. Is each sentence active, with a named actor?
3. Is a sentence longer than 20 words (procedure) or 25 words (description)?
4. Is there an `-ing` word that is not a technical name?
5. Is there a word from the "do not write" column of the word list? Check
   `should`, `may`, `ensure`, `via`, `utilize`, `check (v)`, `about` (for a quantity).
6. Is there a phrasal verb, a contraction, or a semicolon?
7. Is there a noun cluster of 4 or more words?
8. Does one item have two names anywhere in the text?
9. Is each abbreviation expanded on first use?
10. Does each number keep its unit and basis when the source gives them?
11. Does the text say why, not only what?
12. Did the rewrite preserve all facts, uncertainty, and obligations?
13. Did you verify each uncertain word against the Issue 9 dictionary?

## Where this skill goes past STE

These are house rules. STE permits them but does not require them: digits for
all quantities, full dates, no `/`, abbreviations expanded in each document, a
reason for each description, and the `WARNING`/`CAUTION` mapping for software risk.

## References

- [Word choice](references/word-choice.md):
  the substitution list. Words that STE does not approve, and what to write.
- [Examples](references/examples.md):
  before and after for a comment, TSDoc, an ADR, a JIRA task, a runbook step,
  Gherkin, a PR description, and an alert.

## Accuracy note

This skill summarizes [ASD-STE100 Issue 9](https://www.asd-ste100.org/assets/files/ASD-STE100_ISSUE9.pdf)
(2025-01-15). The official dictionary has
875 approved words and 1 274 words that are not approved. It is copyrighted by
ASD and is not reproduced here. The word list in this skill is a selection of
the entries that occur most in software text. Use the official Issue 9
dictionary to verify a word that is not in this skill. If you cannot consult
it, describe the result as STE-guided, not verified STE.
