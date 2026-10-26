# SabiStock - Product Requirements Document


## Who It's For
Small business owner (provision store, 50-100 SKUs) in Nigeria, uses Android phone, wants to run business more efficiently. Not tech-savvy, wants 2-tap actions.


## What It Does
Mobile web app that answers: "What should I restock TODAY so I don't lose customers?"


## Screens & User Stories


### Screen 1: Dashboard (Home) - Owner sees FIRST
- Top: Search bar "Find item..."
- 3 Buckets auto-sorted by urgency:
    - 🔴 Low Stock (<5 left) - Red
    - 🟡 Out Soon (<10 left) - Yellow  
    - 🟢 Safe (10+ left) - Green
- Each card shows: photo, name, quantity left, last sold
- Quick actions: [-1 Sold] [+1 Add] buttons
- Footer: "Items to restock: 8 | Lost sales today: 2"
- User Story: "As owner opening shop, in 5 seconds I know what to buy from market today."


### Screen 2: Add Item
- Inputs: Photo (camera), Name (e.g., "Indomie Chicken"), Quantity (number)
- Optional: Low Stock Alert at (default 5)
- CTA: Save Item
- User Story: "As owner, I can add new stock in <20 secs while customers are in shop."


### Screen 3: Customer Asked Log (Simple)
- Button on each Low Stock item: "Customer Asked"
- Logs: item name + date + count of asks
- User Story: "As owner, when customer asks for what I don't have, I tap one button so I remember to buy it."


## Requirements


### Functional
- FR1: Add item with image + name + qty
- FR2: Persist all items in localStorage `sabistock_items`
- FR3: Auto-categorize: Low <5, Soon <10, Safe >=10
- FR4: Sort Low bucket by qty ascending (lowest first)
- FR5: Search by name instantly
- FR6: Sold -1 reduces qty, +1 increases qty, if 0 moves to Out of Stock
- FR7: Log customer asked taps


### Non-Functional
- NFR1: Works offline, single HTML file, no backend
- NFR2: Loads <2s on 3G
- NFR3: Mobile-first, big buttons for thumb


### AI Best Practice
- Reusable Skill: `restock-prioritizer` - sorts by qty + customer demand
- Plan-before-build: scope -> prd -> spec approved before code


