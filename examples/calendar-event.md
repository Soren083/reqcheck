# Example: Calendar Event

## Original Request

```md
Build a calendar event creation flow.
```

## ReqCheck Questions

```md
1. If users create an event from a selected day, should that day be the default date?
2. If the event time has already passed, should it be allowed, blocked, or marked as past?
3. If users close the page halfway, should the event be saved as draft or discarded?
```

## Brief Outline

```md
# Development Brief

## Goal
Create events without losing the date context users started from.

## Not Doing
No recurring events in version one.

Risk if skipped:
Users may expect weekly or monthly repeats.

Guardrail:
Hide repeat controls and add later if the basic event flow works.

## User Path
Select day -> add event -> confirm details -> return to that day.

## Edge Cases
Past date, timezone, cancel, missing title, notification permission denied.
```
