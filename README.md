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

## Quick Demo

User:

```md
帮我做一个客户导入功能。
```

Without ReqCheck:

```md
AI starts coding an upload button.
```

With ReqCheck:

```md
ReqCheck 会先帮你想清楚：

1. 用户上传后，是直接加入客户列表，还是先看到一页确认名单？
2. 文件里有重复客户时，是自动跳过，还是提醒用户决定？
3. 只导入成功一部分时，用户要不要看到成功和失败的明细？
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

Or in Chinese:

```md
用 reqcheck 先帮我澄清需求，不要直接写代码。

我的需求是：帮我做一个客户导入功能。
```

ReqCheck should output:

- possible goals
- smallest useful version
- fuller version
- risks
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
