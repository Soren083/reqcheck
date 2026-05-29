# Example: Admin Dashboard

## Original Request

```md
Make an admin dashboard for orders.
```

## ReqCheck Questions

```md
1. Which number should be treated as the source of truth: paid orders, created orders, or shipped orders?
2. Who can see this dashboard: all staff, managers, or admins only?
3. If the data fails to load, should users see cached data, an empty state, or a clear error?
```

## Brief Outline

```md
# Development Brief

## Goal
Show order status and revenue using a clear metric definition.

## Not Doing
No custom report builder in version one.

Risk if skipped:
Some users may ask for filters that are not available.

Guardrail:
Provide fixed filters first: date range, status, channel.

## Data and Rules
Define every metric before building charts.

## Edge Cases
No orders, delayed data, permission denied, export mismatch.
```
