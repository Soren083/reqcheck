# Example: Subscription Paywall

## Original Request

```md
Add a paywall for Pro users.
```

## ReqCheck Questions

```md
1. Should users see the paywall before trying the feature, or only after they hit a free limit?
2. What should happen if payment succeeds but the app does not refresh immediately?
3. If subscription expires or payment fails, which Pro features should stop working?
```

## Brief Outline

```md
# Development Brief

## Goal
Introduce Pro access without trapping users in unclear payment states.

## Not Doing
No advanced coupon, refund, or family plan logic in version one.

Risk if skipped:
Support may need to handle edge cases manually.

Guardrail:
Show a support link and clear subscription status.

## User Path
Hit limit -> see paywall -> subscribe -> return to feature with Pro state refreshed.

## Edge Cases
Payment success but stale local state, payment fail, expired subscription, restore purchase.
```
