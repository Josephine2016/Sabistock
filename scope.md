# SabiStock - Scope


## Dream
Help every small business owner in Nigeria run their shop efficiently and never say "We don't have" to a customer again.


## Empathy Discovery
Watched provision shop owners in Port Harcourt/Lagos. Real pain is not big accounting, it's small daily losses.
Quotes:
- "Customer asked for Dano Milk yesterday, I forgot to buy it"
- "I only know Peak Milk finished when shelf is empty"
- "I write restock list on paper, I lose the paper"


Root Problem vs Symptom:
- Symptom: "Sales are low, business not efficient"
- Root Problem: No simple, low-effort way to know what is low and what customers asked for but wasn't available. Existing inventory apps need barcode, internet, money.


## POC Boundary (Must finish in one afternoon)
**Who:** 1 small provision store owner, 50-100 SKUs, Android phone


**Core Flow - One Thing End-to-End:**
1. Add item with photo + quantity (e.g., Indomie 12 pieces)
2. When you sell, tap Sold -1 (quantity reduces)
3. Dashboard instantly shows 3 buckets: Low Stock (<5), Out Soon (<10), Safe
4. "Customer Asked" button to log missed sales


**In Scope for POC:**
- Add item with photo, name, quantity
- Dashboard sorted by urgency: Low Stock first
- Tap Sold -1 / +1 to update stock
- Search to find item fast
- Offline localStorage (works without internet)


**Out of Scope (Intentionally Cut):**
- Barcode scanning, expiry AI, supplier ordering, payments, login, cloud


**Success Metric (Testable Problem Statement):**
For shop owners managing 50-100 SKUs, can we reduce time to know what to restock from ~15 mins (checking shelves + memory) to <1 minute, and reduce "out of stock" lost sales? Measured by: adding 10 items <3 mins, and dashboard shows Low Stock in <5 seconds.


## Why This Wins
Design: Solves daily workflow, not just tech.
Innovation: Simple photo + qty, no barcode needed for low-resource shops.
Purpose: Stops lost sales, helps small businesses be efficient.






