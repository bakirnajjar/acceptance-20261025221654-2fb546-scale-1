# Requirements analysis — password reset

Stage: `requirements_analysis` · Schema version: `1.0`

## Sources

The complete pinned story says: “As a customer I want to reset my password. Apply the curated recovery standard.” It has no end marker. No attachments or approved prior-stage outputs were supplied. The two knowledge excerpts are equivalent versions of the Account recovery standard (v1). The following labels expand to the exact source references used in the JSON and below:

| Label | Exact source reference |
| --- | --- |
| S | `{"source_type":"user_story","source_id":"user_story","locator":"characters 0-16000"}` |
| K1 | `{"source_type":"knowledge","source_id":"467a71ce-c5f8-4557-8f1e-a8ff1b01ecc3","locator":"v1, chunk 0"}` |
| K2 | `{"source_type":"knowledge","source_id":"59efeb19-83f2-4ec6-bfd3-b4aeb7142930","locator":"v1, chunk 0"}` |

The contract-level source references are **S, K1, K2**. Both K1 and K2 are cited for every standard-derived requirement.

## Requirements

### REQ-001 — Password-reset entry point

**Statement:** The sign-in page offers a "Forgot password?" link that starts a password-reset request when selected.

**Acceptance criterion:** Given a customer is on the sign-in page, when the customer selects "Forgot password?", then a password-reset request can be started.

**Source references:** S, K1, K2. **Standard:** ACC-STD-01.

### REQ-002 — Complete a password reset

**Statement:** A customer can complete a password reset using a valid reset link, replacing that account's password.

**Acceptance criterion:** Given a customer has a valid, unused, unexpired reset link for an account, when the customer completes a password reset with a replacement password, then that account's password is replaced.

**Source references:** S, K1, K2. **Standard:** ACC-STD-02 (reset links and completed resets); the story establishes the password-reset goal.

### REQ-003 — Account-existence-neutral response

**Statement:** The response to a password-reset request is identical whether the account exists or does not exist.

**Acceptance criterion:** Given two otherwise equivalent password-reset requests, one for an existing account and one for a nonexistent account, when each request is submitted, then the customer-visible request responses are identical and do not reveal account existence.

**Source references:** K1, K2. **Standard:** ACC-STD-01.

### REQ-004 — Per-account request limit

**Statement:** Password-reset requests are limited to five per account per hour.

**Acceptance criterion:** Given five password-reset requests for an account have been accepted within an hour, when an additional request for that account is made within that hour, then it is limited so that no more than five requests for that account are accepted in that hour.

**Source references:** K1, K2. **Standard:** ACC-STD-01.

### REQ-005 — Single-use reset links

**Statement:** Each reset link is single-use.

**Acceptance criterion:** Given a reset link has been used to complete a password reset, when anyone attempts to use that same link again, then it cannot complete another password reset.

**Source references:** K1, K2. **Standard:** ACC-STD-02.

### REQ-006 — Reset-link expiry

**Statement:** Each reset link expires 30 minutes after it is issued.

**Acceptance criterion:** Given a reset link was issued at a known time, when someone attempts to use it at or after 30 minutes from issuance, then it cannot complete a password reset.

**Source references:** K1, K2. **Standard:** ACC-STD-02.

### REQ-007 — Sign out all active sessions

**Statement:** Completing a password reset signs the account out of every active session.

**Acceptance criterion:** Given an account has one or more active sessions, when its password reset is completed, then every previously active session for that account is signed out.

**Source references:** K1, K2. **Standard:** ACC-STD-02.

### REQ-008 — Notify the account owner

**Statement:** The account owner is notified after every successful password reset.

**Acceptance criterion:** Given an account's password reset completes successfully, when the reset succeeds, then the account owner is notified of that reset; each further successful reset causes its own notification.

**Source references:** K1, K2. **Standard:** ACC-STD-03.

## Evidence and identifier coverage

| Evidence | Requirements | References |
| --- | --- | --- |
| Customer password-reset goal | REQ-001, REQ-002 | S, K1, K2 |
| ACC-STD-01: entry point | REQ-001 | K1, K2 |
| ACC-STD-01: identical response regardless of account existence | REQ-003 | K1, K2 |
| ACC-STD-01: five requests per account per hour | REQ-004 | K1, K2 |
| ACC-STD-02: single-use links | REQ-005 | K1, K2 |
| ACC-STD-02: 30-minute expiry after issue | REQ-006 | K1, K2 |
| ACC-STD-02: all active sessions signed out after completion | REQ-007 | K1, K2 |
| ACC-STD-03: owner notified after every successful reset | REQ-008 | K1, K2 |

## Assumptions

1. acceptance-20261025221654-2fb546-skill-sentinel — the acceptance requirements skill produced this output.
2. A usable reset link permits completion of a password change, as implied by the story and the standard's references to reset links and completed resets; no link delivery channel or transport is assumed.
3. No reset-link delivery channel or transport is prescribed; REQ-002 asserts only use of a valid reset link to complete a password change.
4. No exact reset-request response copy, format, or status code is prescribed; REQ-003 requires an identical customer-visible response whether the account exists or not.
5. No replacement-password policy is prescribed; REQ-002 requires only replacement of the account password.
6. No owner-notification channel or template is prescribed; REQ-008 requires notification after every successful reset.
7. No handling for a nonexistent account identifier under the per-account limit or implementation of the hour window is prescribed; REQ-004 tests the documented limit for accounts without changing REQ-003's identical-response guarantee.
8. A reset request is not itself a completed reset: session sign-out and owner notification apply upon successful completion.

## Non-blocking open questions

All questions have `blocking: false`; the context reproduces the paired assumption used.

| Question | Context / paired assumption |
| --- | --- |
| Through which channel is a reset link delivered, and by what transport? | Assumption: No reset-link delivery channel or transport is prescribed; REQ-002 asserts only use of a valid reset link to complete a password change. |
| What exact response copy or format should reset requests use? | Assumption: No exact reset-request response copy, format, or status code is prescribed; REQ-003 requires an identical customer-visible response whether the account exists or not. |
| What replacement-password policy applies? | Assumption: No replacement-password policy is prescribed; REQ-002 requires only replacement of the account password. |
| Through which channel and with what content is the owner notified? | Assumption: No owner-notification channel or template is prescribed; REQ-008 requires notification after every successful reset. |
| How should the per-account limit apply to a nonexistent account identifier, and how is the hour window defined? | Assumption: No handling for a nonexistent account identifier under the per-account limit or implementation of the hour window is prescribed; REQ-004 tests the documented limit for accounts without changing REQ-003's identical-response guarantee. |
