# Before and after

These fictional examples show style changes. Each "before" version contains
the facts in its "after" version. In real work, do not add facts, thresholds,
actors, or steps that the source does not support. Verify technical terms and
uncertain words against the Issue 9 dictionary and any applicable glossary.

## A code comment

**Before**

```ts
// Since the worker has already processed these records in an earlier step,
// we're skipping them here in order to avoid processing them twice.
```

Breaks: `since` for because, contraction, present perfect, -ing form, and a
long sentence.

**After**

```ts
// The worker processed these records in an earlier step. This code skips the
// records so that the worker does not process them again.
```

## A TSDoc block

**Before**

```ts
/**
 * This method is responsible for handling the processing of the user account
 * status change events which are being emitted by the account service.
 */
```

Breaks: noun for the action (STE 3.7), `handle`, passive, continuous tense,
5-word noun cluster.

**After**

```ts
/**
 * This method processes the status change events that the account service
 * sends for user accounts.
 */
```

## An ADR context paragraph

**Before**

> Currently, the API service and the worker are handling retries, and it has
> been observed that the worker sometimes does not receive the retry event.
> When this happens, the job stays PENDING and the request stays open. We
> measured this on 14 of 210 requests in August 2026.

Breaks: `currently`, present perfect, passive voice, -ing form, and a long
sentence.

**After**

> The API service and the worker process retries. The worker does not always
> receive the retry event. When the event does not arrive, the job keeps the
> PENDING status. Thus, the request stays open. We measured this on 14 of 210
> requests in August 2026.

## A JIRA task description

**Before**

> The metrics job runs `collect_metrics` every 60 seconds, and the query is
> doing a sequential scan. The disk read rate hit the alert threshold at 04:00
> on 3 nights in September 2026. This task is to add the 2 missing
> indexes. Acceptance means the query plan uses an index scan and the 04:00
> disk read rate stays under the alert threshold for 7 nights.

Breaks: continuous tense, a long sentence, and unclear separation of the
problem, change, and acceptance conditions.

**After**

> ## Problem
>
> The metrics job runs `collect_metrics` every 60 seconds. The query does a
> sequential scan. The disk read rate reached the alert threshold at 04:00 on
> 3 nights in September 2026.
>
> ## Change
>
> Add the 2 missing indexes for the query.
>
> ## Acceptance
>
> The query plan uses an index scan. For 7 nights, the 04:00 disk read rate
> stays less than the alert threshold.

## A runbook step

**Before**

> Next you'll want to take a snapshot of `expired_sessions`, then run
> `sessions/find.sql` and record the count. After that, run
> `sessions/delete.sql` and check that it deletes the same number of rows.
> Without the snapshot, you can't restore the deleted rows.

Breaks: the risk appears after the action, contractions, and several
instructions in one sentence.

**After**

> **WARNING:** Take a snapshot of the `expired_sessions` table before step 2.
> Without the snapshot, you cannot restore the deleted rows.
>
> 1. Run `sessions/find.sql`. Record the row count.
> 2. Run `sessions/delete.sql`. The number of deleted rows
>    must be equal to the row count from step 1.

## A Gherkin scenario

**Before**

```gherkin
Scenario: Testing what happens when the scheduler sends a timeout event for
          an open request with an active job, which must set the request to
          EXPIRED and the job to STOPPED
  Given there is an open request that has got an active job
  When a timeout event is received from the scheduler for the request
  Then the request should have the EXPIRED status
  And the job should have the STOPPED status
```

Breaks: -ing form, present perfect, passive voice, `should`, and a long title.

**After**

```gherkin
Scenario: A timeout event sets an open request to EXPIRED
  Given an open request with an active job
  When the scheduler sends a timeout event for the request
  Then the request status is EXPIRED
  And the job status is STOPPED
```

## A PR description summary

**Before**

> This PR basically changes the retry job so it reads the retry limit from the
> configuration instead of the default limit. It also fixes two bugs: the
> worker did not retry FAILED jobs, and a completed job lost its retry count.

Breaks: `basically`, `fix`, and a long sentence.

**After**

> The retry job reads the retry limit from the configuration, not from the
> default limit. This change corrects 2 bugs. The worker did not retry FAILED
> jobs. A completed job lost its retry count.

## A Datadog monitor message

**Before**

> The `events` consumer lag went over 10 000 messages for 15 minutes. The
> topic has no dead-letter queue, so one bad offset stops the
> consumer. Please investigate the skip list and the offset in the error log.

Breaks: `went over`, `investigate`, and two instructions in one sentence.

**After**

> The `events` consumer lag is more than 10 000 messages for 15 minutes. This
> topic has no dead-letter queue. Thus, one bad offset stops
> the consumer.
>
> 1. Examine the skip list.
> 2. Examine the offset in the error log.
