---
name: reqcheck
description: Use this skill before writing code when the user has a vague feature idea, bug report, screenshot, competitor reference, product change, or asks to build something but is not technical enough to specify fields, APIs, states, edge cases, or release constraints. This skill turns fuzzy product requests into a clear development brief by expanding likely goals, finding hidden risks, clarifying what not to do yet, explaining what could break if skipped, and asking at most 3 important questions before implementation. For permissions, payments, app review, privacy, health data, dates, async AI/upload flows, login/account, or cross-surface data, use deep mode and audit whether every UI success/failure state has a real source of truth. Use it even if the user does not explicitly ask for "requirements" or "PRD".
---

# ReqCheck

Use this skill to clarify a software request before implementation.

The user may not know how to describe technical constraints. Do not wait for them to mention fields, APIs, states, permissions, edge cases, compatibility, or release steps. Think through the common places where AI-generated software breaks, then ask only the questions that affect product direction.

This skill does not write code, edit files, test the finished feature, or deploy. It produces a clear development brief that another coding agent can execute.

## Output Goals

Complete five things:

1. Turn the user's fuzzy request into 2-3 possible directions.
2. Explain what each direction changes, roughly where the cost is, and what it may affect.
3. Make the user choose a clear version that can be handed to a coding agent.
4. Write what is not being done now, why, what could go wrong if skipped, and how to reduce that risk.
5. For high-risk requests, audit UI promises against real evidence before allowing any success, connected, saved, synced, authorized, deleted, paid, or completed state.

Use plain language. Avoid technical questions that a non-code user cannot answer unless the answer is truly a product decision.

## Workflow

Follow this order:

1. Judge how clear the request is.
2. If unclear, expand it into possible goals and versions.
3. Look at project context if available: existing code, docs, screenshots, competitors, current stage.
4. Tag high-risk domains: permissions, payments/subscriptions, app review, privacy/legal, health data, dates/reports, async AI/upload, login/account, deletion/export, or shared data shown in many places.
5. If any high-risk tag applies, run Deep Mode before asking questions.
6. Run the common bug scenario check below.
7. Give a smallest useful version and a fuller version.
8. List risks and not-doing tradeoffs.
9. Ask at most 3 questions that change the direction.
10. After the user confirms, output a development brief.

## Deep Mode

Use Deep Mode whenever the request touches permissions, payments, app review, privacy/legal, health data, dates/reports, async AI/upload flows, login/account, deletion/export, or a UI state that claims something is saved, synced, connected, authorized, paid, deleted, complete, or successful.

Deep Mode is not more brainstorming. It is an evidence check. The goal is to prevent UI that makes users or reviewers believe something happened when the system cannot prove it.

### Source-of-Truth Audit

List the important UI states and promises before implementation:

| UI says | User will believe | Source of truth | Can verify reliably? | If not verified, safer UI |
| --- | --- | --- | --- | --- |
| Connected / complete / saved / paid / synced / authorized | The action really succeeded | API response, backend record, local transaction, OS permission API, receipt, webhook, query result, or explicit user confirmation | yes/no/partial | pending, unverified, attempted, needs confirmation, continue without, retry |

Rules:

- Do not allow a success state unless a specific source of truth proves it.
- If a platform hides or limits the status, say that plainly and design the UI as "unverified" or "will try to sync", not "connected" or "complete".
- A local flag is only proof of local user intent unless it is backed by a real permission, backend, receipt, or data-read result.
- If data absence and permission denial look the same, do not label the state as denied or granted. Use neutral copy and a recovery path.
- If an old request, cache, webhook, OTA, or backend job can overwrite the new action, include the stale-response guardrail.

### State Machine

For high-risk work, list states instead of only the happy path:

- initial / not requested
- requesting / loading
- user cancels or backs out
- system returns but result is unknown
- allowed / verified
- denied / unavailable
- partial success
- no data yet
- retry / reconnect / restore
- offline / timeout
- stale local cache or old async response

Only include states that apply, but do not skip "unknown" when the platform or API cannot prove the result.

### Negative Acceptance

For high-risk work, acceptance checks must include the ways the feature can lie:

- User denies permission, payment fails, AI fails, upload fails, or backend times out.
- User leaves mid-flow and returns.
- The same data is viewed from another page.
- Device, language, screen size, OS version, or review environment differs.
- The system returns no data even though the request technically succeeded.

If any of these cases can show a misleading success state, the brief is not ready.

## Common Bug Scenario Check

Before asking the user questions, map the request to these software scenarios. If a row applies, ask the corresponding product question in plain language.

| Scenario | What breaks when missed | Example in another project | Ask before development |
| --- | --- | --- | --- |
| Temporary result vs final result | User only previewed, edited halfway, or left the page, but the data already became real | E-commerce preview reserves stock; approval draft is sent; import preview creates real customers | When does this data become real? What happens if the user cancels, goes back, or closes the page? |
| New entry reusing old entry | Two entries create the same object, but their input, validation, timeout, and errors differ | Bulk import reuses manual form validation; voice input reuses text input and loses fields | Is this just a variant of an old entry, or a new flow? What can be reused, and what needs separate handling? |
| Same data shown in many places | Home, detail, notification, export, and AI summary show different numbers | Dashboard sales differ from exported Excel; wallet balance differs from checkout | Which place is the source of truth? Where else will this data appear? |
| Page context inheritance | User starts from one page, but the result saves to a default place | Add calendar event from May 1 but it saves to today; add task under Project A but it goes to default project | What context should carry over from the current page: date, project, store, customer, role? |
| Business time vs created time | Reports and lists use the time the row was created, not when the event happened | Today entering last week's expense counts as today's cost; backfilled inventory appears in today's report | What date or period does this record belong to? Are created time and event time different? |
| Completion, expiry, and late confirmation | The system does not know whether something is done, expired, failed, or still waiting | Appointment time passes; course reaches 90%; task is submitted after deadline | When is this done? What if the user does not open the product at that moment? Can they confirm later? |
| Non-default environment | Default device and language work, but real users use other conditions | Long translated text squeezes buttons; small phone hides the main action; dark mode has low contrast; streaming request misses locale/auth headers | Which languages, screen sizes, font sizes, themes, OS versions, and special request paths must work in version one? |
| Old request overwrites new action | A slower old response replaces the user's latest action | Uploaded avatar flashes then returns to default; cart quantity changes back; old profile response overwrites new nickname | After the user acts, can older requests or caches overwrite the new result? Show local result first or wait for server? |
| Failure retry and loading state | Failure causes endless loading, repeated retry, flicker, or duplicate submit | Upload progress jumps forever; payment retries repeatedly; dashboard skeleton flashes offline; AI generation button stays stuck | What should users see on failure, timeout, or offline? Retry automatically? How many times and how often? |
| Permission denial and partial usability | Rejecting one permission blocks the whole product | Deny location and cannot type address manually; deny photos and cannot change avatar; deny notification but settings still show enabled | If permission is denied, what stops working? Is there a manual fallback? Can other features still work? |
| Release path and version | Code changed, but users see another version or frontend/backend do not match | Web cache serves old page; frontend button is live but backend API is not; app review build opens and asks for update | How does this change reach users: frontend release, backend deploy, config switch, native app build, data migration? |
| Unverifiable success state | The UI says connected, saved, synced, paid, or authorized, but the system cannot prove it | HealthKit read permission status is hidden; payment redirect returns before webhook; upload job is queued but not processed | What proves this success state is true? If it cannot be proven, should the UI say pending, unverified, or will try? |
| UI promise vs real behavior | Page, policy, review note, or marketing says one thing; system behavior does another | Delete account button exists but backend keeps data; privacy policy says no location but app requests it; refund copy exists but no refund flow | Does backend, permission behavior, data handling, and support flow actually match what the UI or policy says? |

Do not dump this whole table into the response. Pick the relevant rows.

## What Not To Do Yet

Every brief should include a not-doing section when scope is cut.

Use this structure:

| Not doing now | Why skip it now | What could go wrong | Guardrail |
| --- | --- | --- | --- |

If skipping something can put data in the wrong place, make users think it worked when it did not, expose data, break review/payment/login, or create a hard later migration, call it out.

If skipping source-of-truth work can make the UI claim success without proof, explicitly mark that success state as pending/unverified, or require a verifiable backend/OS/receipt/data-read check before showing success.

## Questions

Ask at most 3 questions at once.

Good questions:

- This feature is for internal testing or real users?
- Should the result save immediately, or only after user confirmation?
- If this starts from a date/project/store/customer page, should the result inherit that context?
- If the request fails, should users retry, save a draft, or just return to the previous page?

Bad questions for non-code users:

- What database fields do you want?
- What should the API schema be?
- What component library should I use?

Only ask technical questions after inspecting the project and only when it changes product direction.

## Development Brief Format

After the user confirms direction, output this structure:

```md
# Development Brief

## Background
Why the user wants this.

## Goal
What this change should solve now.

## Not Doing
What is not included, why, what could go wrong, and the guardrail.

## User Path
Where users enter, what they do, and how the flow ends.

## Pages and Interactions
Pages, buttons, modals, messages, empty states, loading states.

## Data and Rules
What is shown, saved, calculated, limited, or deleted.

## Source of Truth
For any success/failure/connected/saved/synced/authorized/paid/deleted state: what proves it, whether it can be verified reliably, and what the UI says when it cannot be verified.

## Edge Cases
Failure, cancel, back, repeat tap, no data, permission denial, offline/timeout.

## AI Behavior
If AI is involved: input, output, uncertainty, timeout, fallback, safety limits.

## Acceptance
How a non-code user can verify this is done, including negative tests where the feature fails, is denied, is cancelled, has no data, or cannot verify the result.

## Dependencies and Impact
Existing pages, data, external services, manual process, release path.

## Later
What can be added after this version works.
```

## Style

Be direct and concrete.

Avoid vague advice like "improve user experience" unless it is translated into a path, state, rule, or acceptance check.
