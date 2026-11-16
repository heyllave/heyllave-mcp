---
name: setup-catalog
description: Migrate customer and product data into heyllave and set up the catalog — from spreadsheets, CSVs, invoices, or an export of another system.
---

# Set up the heyllave catalog / migrate data

You are helping a heyllave tenant load their real-world data (customers, products, suppliers, prices) into heyllave. Treat this as a careful migration, not a bulk dump.

## Workflow

1. **Discover what you can write.** List the heyllave MCP tools available in this session. If only read tools are exposed (e.g. `list_work_orders`, `get_work_order`, `search_customers`), say so plainly and fall back to preparing clean import-ready files plus instructions for the heyllave app, instead of pretending to write.
2. **Get the source data.** Ask the user for whatever they have: a spreadsheet, CSV, photos of price lists, supplier invoices, or an export from their previous system. Read it fully before proposing a mapping.
3. **Propose a mapping and get sign-off.** Show a short table: source column → heyllave field. Flag anything ambiguous (units, taxes included or not, duplicate names) as questions rather than guessing.
4. **Normalize before loading:**
   - Monetary amounts in heyllave are **integer cents** (e.g. $12.500 COP → `1250000` cents is WRONG; $12.500,00 COP → `1250000` cents — be careful with Latin-American thousand separators; confirm with the user when a price looks 100× off).
   - Deduplicate customers/products by normalized name; prefer updating an existing record over creating a near-duplicate.
   - Keep **one supplier per catalog item** unless the tenant explicitly manages multiple suppliers per product.
5. **Load a small batch first** (3–5 records), verify them with the read tools, show the user, and only then run the full load.
6. **Verify and report.** After loading, re-query to confirm counts match the source. Report: created, updated, skipped (with reasons), and anything needing manual review.

## Rules

- Never invent prices, tax rates, or customer identifiers — ask.
- Never delete or overwrite existing records without showing the user what would change.
- If the source has invoice-level data from multiple suppliers, keep one fiscal line per invoice and do the per-supplier split in inventory notes/references, not as separate fiscal documents.
