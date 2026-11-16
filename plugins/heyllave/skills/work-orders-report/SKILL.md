---
name: work-orders-report
description: Build a report from heyllave work orders — totals by state, customer, or period, with line-item detail where it matters.
---

# heyllave work-orders report

You are producing a business report from the tenant's heyllave work orders.

## Workflow

1. **Clarify scope only if genuinely ambiguous.** Default: all work orders, grouped by state, newest first. If the user named a customer, resolve them with `search_customers` first to get their public ID.
2. **Fetch completely.** `list_work_orders` returns `total`, which can exceed the rows returned — paginate (raise `limit`, filter by `state` or `customer_id`) until you have every matching row. Never report on a partial fetch without saying so.
3. **Fetch detail where needed.** Totals and line items live on `get_work_order`. Pull details for the orders the report actually analyzes; skip detail calls for orders that only appear in counts.
4. **Handle money correctly.** All amounts are **integer cents** in the work order's `currency` (typically COP). Divide by 100 and format with the currency code. Never mix currencies in one total — split by currency if more than one appears.
5. **Aggregate.** Typical cuts: count and total value by state (draft / in_progress / completed / invoiced / …), by customer, by assignee, by month. Call out: empty drafts (no lines), stale drafts, and refund/amendment states.

## Output

- Lead with a 2–3 sentence summary of the headline numbers.
- Then compact tables (one per cut). Keep columns to what the reader acts on.
- Note data caveats explicitly: partial periods, draft orders with zero totals, orders excluded and why.
- In hosts that support documents/artifacts, offer the report as a shareable page; otherwise plain markdown is fine.
