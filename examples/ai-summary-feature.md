# Example: AI Summary Feature

## Original Request

```md
Add an AI summary for user reports.
```

## ReqCheck Questions

```md
1. Should the AI summary be generated automatically, or only after users click a button?
2. If the AI result is wrong or too vague, can users regenerate or edit it?
3. Which report data is the AI allowed to use, and what should it do if some data is missing?
```

## Brief Outline

```md
# Development Brief

## Goal
Help users understand reports faster without presenting AI output as guaranteed truth.

## Not Doing
No long chat interface in version one.

Risk if skipped:
Users cannot ask follow-up questions.

Guardrail:
Provide a concise summary and a regenerate button.

## AI Behavior
Input: report data visible to the user.
Output: short summary, important changes, possible next actions.
Fallback: show a normal report if AI fails.

## Edge Cases
AI timeout, missing data, unsafe advice, user lacks permission for some data.
```
