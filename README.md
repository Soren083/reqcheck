# ReqCheck

Preflight checks for AI coding.

ReqCheck is a Codex / Claude skill that turns vague product ideas into development-ready briefs before an AI agent writes code.

It is built for people who are not code-first. The user can say something rough like "build a customer import feature" or "make this page work like the competitor", and ReqCheck helps the AI ask the product questions that usually become bugs later.

## Why

Most AI coding failures do not start in code.

They start earlier, when the request skips questions like:

- When does this data become real?
- What happens if the user cancels?
- Does this new entry behave like the old entry?
- Can an older request overwrite the user's latest action?
- How will users actually get this version?

ReqCheck makes the AI slow down before implementation and turn those hidden decisions into plain questions.

For high-risk product work, ReqCheck also checks whether the UI has evidence for what it claims. A page should not say "connected", "saved", "synced", "paid", "authorized", "deleted", or "complete" unless there is a real source of truth behind that state. If an API, platform, or backend cannot prove the result, ReqCheck should push the design toward "pending", "unverified", "will try", or another honest fallback.

## Quick Demo

User:

```md
Build a customer import feature.
```

Without ReqCheck:

```md
AI starts coding an upload button.
```

With ReqCheck:

```md
ReqCheck first asks:

1. After users upload a file, should customers be added immediately, or should users review the list first?
2. If the file contains customers that already exist, should the system skip them automatically or ask users what to do?
3. If only part of the file imports successfully, should users see which rows succeeded and which failed?
```

That difference matters. The second version prevents real bugs: accidental imports, duplicate customers, unclear partial failures, and confusing recovery flows.

## What ReqCheck Catches

ReqCheck uses a pattern library extracted from real AI-assisted product development bugs.

| Pattern | What it prevents |
| --- | --- |
| Temporary result vs final result | Previewed or half-edited data becoming real too early |
| New entry reusing old entry | A new input path incorrectly sharing old validation, timeout, or error handling |
| Same data shown in many places | Home, detail, export, notification, and AI summary disagreeing |
| Page context inheritance | New records saving to the wrong date, project, store, customer, or role |
| Business time vs created time | Reports using when data was entered instead of when the event happened |
| Completion / expiry / late confirmation | The system not knowing whether something is done, expired, or waiting |
| Non-default environment | Long text, small screens, denied permissions, dark mode, or locale-specific paths breaking |
| Old request overwrites new action | Slow old responses replacing the user's latest change |
| Failure retry and loading state | Infinite loading, repeated retries, flicker, or duplicate submit |
| Permission denial and partial usability | One rejected permission blocking the whole product |
| Release path and version | Users, reviewers, frontend, and backend seeing different versions |
| Unverifiable success state | UI claiming success when the system cannot prove the permission, payment, sync, or job actually succeeded |
| UI promise vs real behavior | The UI or policy promising something the backend does not actually do |

## Install

### Codex

Copy the skill directory into your Codex skills folder:

```bash
mkdir -p ~/.codex/skills
cp -R skills/codex/reqcheck ~/.codex/skills/reqcheck
```

Restart Codex or open a new session if the skill list does not refresh immediately.

### Claude Code

Copy the skill directory into your Claude skills folder:

```bash
mkdir -p ~/.claude/skills
cp -R skills/claude/reqcheck ~/.claude/skills/reqcheck
```

You can also import the packaged file:

```text
packages/reqcheck.skill
```

## Use

```md
Use reqcheck before coding.

My request:
Build a customer import feature.
```

ReqCheck should output:

- possible goals
- smallest useful version
- fuller version
- risks
- source-of-truth audit for high-risk states
- what not to do yet
- what could break if skipped
- at most 3 important questions
- after confirmation, a development brief

## Examples

See:

- [Customer import](examples/customer-import.md)
- [Subscription paywall](examples/subscription-paywall.md)
- [Calendar event](examples/calendar-event.md)
- [Admin dashboard](examples/admin-dashboard.md)
- [AI summary feature](examples/ai-summary-feature.md)

Each example includes:

1. Original fuzzy request
2. ReqCheck questions
3. Development brief outline

## Evaluation Ideas

ReqCheck is not evaluated by whether it writes code. It is evaluated by whether it catches hidden product decisions before code is written.

A good ReqCheck response should:

- ask no more than 3 questions at a time
- avoid database/API/schema questions before product rules are clear
- include a not-doing section when scope is cut
- explain what could break if something is skipped
- catch at least one hidden state, data, context, failure, permission, or release problem
- reject or downgrade success states that do not have a verifiable source of truth
- include negative acceptance checks for high-risk requests
- produce a brief a coding agent can implement after the user confirms direction

See [eval prompts](evals/prompts.json).

## Project Structure

```text
reqcheck/
  README.md
  skills/
    codex/reqcheck/SKILL.md
    claude/reqcheck/SKILL.md
  packages/
    reqcheck.skill
  docs/
    patterns.md
  examples/
  evals/
```

## License

MIT
