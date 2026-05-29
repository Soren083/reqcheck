# ReqCheck Pattern Library

These patterns are the core of ReqCheck. They are written for product thinking before code starts.

## 1. Temporary Result vs Final Result

Question: when does this become real?

Examples:

- An import preview should not create real customers until confirmed.
- A checkout preview should not reserve stock unless the product intends that.
- A draft approval should not notify reviewers before submit.

## 2. New Entry Reusing Old Entry

Question: is this actually the same flow?

Examples:

- Manual create and CSV import both create customers, but validation and error recovery differ.
- Text input and voice input both create notes, but transcription can fail.
- Photo upload and file URL import both create media, but timeout and permissions differ.

## 3. Same Data Shown In Many Places

Question: which number is the source of truth?

Examples:

- Dashboard revenue and exported revenue must use the same definition.
- Wallet balance and checkout balance must not disagree.
- Progress shown on home and detail page should follow the same rule.

## 4. Page Context Inheritance

Question: what should carry over from where the user started?

Examples:

- Creating an event from May 1 should default to May 1.
- Creating a task inside Project A should attach to Project A.
- Adding a product from Store B should not save under the default store.

## 5. Business Time vs Created Time

Question: when did it happen, not when was it entered?

Examples:

- Backfilled expenses belong to the purchase date.
- A delayed sales note belongs to the visit date.
- Inventory backfill belongs to the stock movement date.

## 6. Completion, Expiry, And Late Confirmation

Question: what counts as done?

Examples:

- A course watched to 90% may still need a finish rule.
- A task after deadline may be late, failed, or accepted.
- An appointment may need manual confirmation after the scheduled time passes.

## 7. Non-default Environment

Question: what real environments must work?

Examples:

- Translated copy may be longer.
- Small screens may hide primary buttons.
- Dark mode may break contrast.
- A streaming request may miss headers that normal requests include.

## 8. Old Request Overwrites New Action

Question: can stale data replace fresh user action?

Examples:

- Old profile response overwrites a newly uploaded avatar.
- Old cart response resets a changed quantity.
- Old cache restores a deleted item.

## 9. Failure Retry And Loading State

Question: what happens when it fails?

Examples:

- Upload progress should not loop forever.
- Payment retry should not duplicate charges.
- AI generation failure should unlock the button and explain what happened.

## 10. Permission Denial And Partial Usability

Question: can the user keep going without this permission?

Examples:

- Deny location, but allow manual address input.
- Deny photos, but allow taking a new photo if camera is allowed.
- Deny notifications, but keep in-app reminders visible.

## 11. Release Path And Version

Question: how will users get the change?

Examples:

- A frontend change may need cache invalidation.
- A backend API change may require old frontend compatibility.
- A mobile change may require a native build, not just an OTA update.

## 12. UI Promise Vs Real Behavior

Question: does the system really do what it says?

Examples:

- Delete account should delete or anonymize the right data.
- Privacy copy should match actual permission requests.
- Refund text should match support and payment behavior.

## 13. Unverifiable Success State

Question: what proves this success state is true?

Examples:

- A HealthKit read permission flow can return without proving whether the user allowed read access, so the UI should not say "connected" unless another reliable check exists.
- A payment redirect can return before a webhook confirms the subscription, so the UI should show pending or verifying instead of instantly unlocking paid features.
- An upload job can be queued but not processed, so the UI should say uploaded or processing only according to the backend state it can verify.
- A local flag can prove user intent, but it cannot prove that a permission, sync, deletion, or payment actually succeeded.
