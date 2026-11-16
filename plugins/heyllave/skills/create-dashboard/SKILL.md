---
name: create-dashboard
description: Build a visual dashboard (KPIs and charts) from heyllave data — work orders, customers and catalog.
---

# heyllave dashboard

You are building a visual dashboard from the tenant's live heyllave data.

## Workflow

1. **Pull the data first, design second.** Fetch all relevant work orders via `list_work_orders` (paginate using `total`; filter by `state`/`customer_id`) and `get_work_order` for totals and line items. Amounts are integer cents — convert before charting.
2. **Pick 3–6 KPIs that match a small-business operator's questions**, e.g.:
   - Orders by state (how much is stuck in draft vs invoiced)
   - Revenue by month (completed/invoiced only — don't count drafts as revenue)
   - Top customers by value
   - Top products by quantity sold
3. **Render for the host you're in:**
   - Claude Code / Claude apps: a single self-contained HTML page (inline CSS/JS, no external CDNs or network calls), published as an artifact if available, otherwise saved as a local file the user can open.
   - ChatGPT and other hosts: use the host's canvas/artifact equivalent, or emit the HTML file.
4. **Design rules:** one clear number per KPI tile with its label and period; bar/line charts over pie charts; label axes with real currency values (e.g. `$1.2M COP`), not cents; legible in light and dark themes; the page must state the data's as-of timestamp and any filters applied.
5. **Be honest about freshness.** The dashboard is a snapshot of the moment you queried. Say so on the page, and tell the user that re-running this skill regenerates it with current data.

## Rules

- Never fabricate or extrapolate data points; if a month has no orders, show it as zero, not interpolated.
- If the dataset is too large to fetch completely, say what was sampled and how.
