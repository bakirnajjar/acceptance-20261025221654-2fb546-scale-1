# Requirements analysis — evidence-mapping draft

This is a draft for the requirements contract, not a product output. The complete pinned story was read; there are no attachments, prior-stage outputs, or story end marker. No approved plan file was available in the repository. The two retrieved excerpts of *Account recovery standard (v1)* have equivalent content; retain **both** knowledge citations on every standard-derived requirement.

## Source-reference register

The labels below expand to the **exact** `StageSourceReference` objects; use the objects, not just the labels, in the eventual contract.

| Label | Exact source reference | Evidence |
| --- | --- | --- |
| S | `{"source_type":"user_story","source_id":"user_story","locator":"characters 0-16000"}` | “As a customer I want to reset my password. Apply the curated recovery standard.” |
| K1 | `{"source_type":"knowledge","source_id":"467a71ce-c5f8-4557-8f1e-a8ff1b01ecc3","locator":"v1, chunk 0"}` | Account recovery standard (v1), ACC-STD-01/02/03. |
| K2 | `{"source_type":"knowledge","source_id":"59efeb19-83f2-4ec6-bfd3-b4aeb7142930","locator":"v1, chunk 0"}` | Account recovery standard (v1), ACC-STD-01/02/03; equivalent to K1. |

Use S, K1, and K2 as the contract-level `source_references` set. Each requirement below lists its own source set; expanding each label supplies exact `source_type`, `source_id`, and `locator`.

## Atomic requirements and acceptance criteria

### REQ-001 — Password-reset entry point

**Statement:** The sign-in page offers a “Forgot password?” link that starts a password-reset request when selected.

**Source references:** S, K1, K2. **Standard coverage:** ACC-STD-01, first bullet.

**Acceptance criterion:** Given a customer is on the sign-in page, when the customer selects “Forgot password?”, then a password-reset request can be started.

### REQ-002 — Complete a password reset

**Statement:** A customer can complete a password reset using a valid reset link, replacing that account's password.

**Source references:** S, K1, K2. **Standard coverage:** ACC-STD-02 (“reset links”; “completing a reset”) and the story's password-reset goal. This states only the implied core outcome, not a delivery mechanism or password policy.

**Acceptance criterion:** Given a customer has a valid, unused, unexpired reset link for an account, when the customer completes a password reset with a replacement password, then that account's password is replaced.

### REQ-003 — Account-existence-neutral request response

**Statement:** The response to a password-reset request is identical whether the account exists or does not exist.

**Source references:** K1, K2. **Standard coverage:** ACC-STD-01, second bullet.

**Acceptance criterion:** Given two otherwise equivalent password-reset requests, one for an existing account and one for a nonexistent account, when each request is submitted, then the customer-visible request responses are identical; no account existence is revealed by the response.

### REQ-004 — Per-account request limit

**Statement:** Password-reset requests are limited to five per account per hour.

**Source references:** K1, K2. **Standard coverage:** ACC-STD-01, third bullet.

**Acceptance criterion:** Given five password-reset requests for an account have been accepted within an hour, when an additional request for that account is made within that hour, then it is limited so that no more than five requests for that account are accepted in that hour.

### REQ-005 — Single-use reset links

**Statement:** Each reset link is single-use.

**Source references:** K1, K2. **Standard coverage:** ACC-STD-02, first bullet, single-use clause.

**Acceptance criterion:** Given a reset link has been used to complete a password reset, when anyone attempts to use that same link again, then it cannot complete another password reset.

### REQ-006 — Reset-link expiry

**Statement:** Each reset link expires 30 minutes after it is issued.

**Source references:** K1, K2. **Standard coverage:** ACC-STD-02, first bullet, expiry clause.

**Acceptance criterion:** Given a reset link was issued at a known time, when someone attempts to use it at or after 30 minutes from issuance, then it cannot complete a password reset.

### REQ-007 — Sign out all active sessions

**Statement:** Completing a password reset signs the account out of every active session.

**Source references:** K1, K2. **Standard coverage:** ACC-STD-02, second bullet.

**Acceptance criterion:** Given an account has one or more active sessions, when its password reset is completed, then every previously active session for that account is signed out.

### REQ-008 — Notify the account owner

**Statement:** The account owner is notified after every successful password reset.

**Source references:** K1, K2. **Standard coverage:** ACC-STD-03, sole bullet.

**Acceptance criterion:** Given an account's password reset completes successfully, when the reset succeeds, then the account owner is notified of that reset; each further successful reset causes its own notification.

## Identifier and evidence coverage

| Evidence identifier or story goal | Covered by | Exact reference labels |
| --- | --- | --- |
| Customer can reset their password | REQ-001, REQ-002 | S; K1, K2 for standard context |
| ACC-STD-01 — “Forgot password?” starts a request | REQ-001 | K1, K2 |
| ACC-STD-01 — response identical for existing/nonexistent accounts | REQ-003 | K1, K2 |
| ACC-STD-01 — five requests per account per hour | REQ-004 | K1, K2 |
| ACC-STD-02 — reset links are single-use | REQ-005 | K1, K2 |
| ACC-STD-02 — reset links expire 30 minutes after issue | REQ-006 | K1, K2 |
| ACC-STD-02 — completion signs out every active session | REQ-007 | K1, K2 |
| ACC-STD-03 — owner notified after each successful reset | REQ-008 | K1, K2 |

## Assumptions for the eventual contract

- `acceptance-20261025221654-2fb546-skill-sentinel` — the acceptance requirements skill produced this evidence-mapping output.
- A usable reset link permits completion of a password change, as implied by the story and the standard's references to reset links and completed resets; no link delivery channel or transport is assumed.
- No exact request-response copy, response format, or status code is prescribed; “identical” covers the customer-visible response for existing and nonexistent accounts.
- The five-per-account-per-hour criterion is applied to accounts; the handling of nonexistent account identifiers and the hour-window implementation are not inferred.
- No replacement-password policy, notification channel, or notification template is inferred.
- A reset request is not itself a completed reset: session sign-out and owner notification apply upon successful completion.
- The complete story has no end marker; no attachments were supplied and therefore there is no acceptance attachment marker to reproduce.

## Non-blocking open questions (and assumptions used)

All questions are **non-blocking** (`blocking: false`) and do not add requirements beyond the cited evidence.

| Question | Assumption used while drafting |
| --- | --- |
| Through which channel is a reset link delivered, and by what transport? | No channel or transport is prescribed; REQ-002 asserts only use of a valid reset link to complete a password change. |
| What exact response copy or format should reset requests use? | No copy or format is prescribed; REQ-003 requires an identical customer-visible response across account-existence cases. |
| What replacement-password policy applies? | No policy is prescribed; REQ-002 requires only replacement of the account password. |
| Through which channel and with what content is the owner notified? | No channel or template is prescribed; REQ-008 requires notification after every successful reset. |
| How should the per-account limit apply to a nonexistent account identifier, and how is the hour window defined? | No nonexistent-account or window implementation is prescribed; REQ-004 tests the documented five-per-account-per-hour limit for accounts, without changing REQ-003's identical-response guarantee. |
