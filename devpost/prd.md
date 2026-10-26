---
doc: prd
status: approved
---

# SabiStock — Product Requirements

A quick stock-restocking app for small phone and computer-accessory shops with no more than 10 people.
Source: `scope.md > Who It's For`, `scope.md > The Unique Kernel`.

## The Core Journey

1. The owner opens SabiStock and sees a dashboard organized by stock urgency. Items below five are in the red Low Stock bucket at the top, with the lowest quantities first.
2. The owner searches for an item or finds its card, which shows its photo (or a placeholder), name, and quantity left.
3. When an item sells, the owner taps Sold -1. Its quantity updates immediately, and its position or bucket changes if its urgency changes. Stock never falls below zero.
4. When stock arrives, the owner taps Add +1 on its card. The quantity updates immediately and the item moves to the appropriate bucket if needed.
5. The owner can add another item using its name and quantity, with an optional photo. After saving, the item appears in the correct dashboard bucket.
6. The owner uses the prioritized list to identify what to restock in less than a minute, instead of spending about 15 minutes checking shelves.

Source: `scope.md > The Core Loop`, `scope.md > What "Working" Looks Like`, `scope.md > The POC Boundary`.

## Screens and Layout

- **Dashboard:** A search bar at the top, followed by three vertically arranged buckets: red Low Stock, yellow Out Soon, and green Safe. Show the Low Stock bucket first and the lowest quantities first within it. Show a count such as “Need to restock: 5 items.” Each item card has a photo or placeholder, item name, quantity left, and large Sold -1 and Add +1 buttons. A prominent Add Item action opens the add-item form.
- **Add Item form:** Three fields: optional photo (take a photo or choose from the gallery, with a preview), required name, and quantity. Include a Save button. After a valid save, return to the dashboard with the item visible in its bucket.

The exact transition style between the dashboard and add-item form is not specified.

## Look and Feel

Clean, simple, and fast, with a familiar small-business utility feel. Use a white background, dark text, large fonts, rounded cards, and large thumb-friendly buttons. Red signals urgent low stock, yellow signals a warning, and green signals safe stock. Avoid fancy decoration that slows the quick stock check.

## Features and Behavior

### Stock dashboard and item updates

Develops `scope.md > The Unique Kernel`, `scope.md > The Core Loop`, and `scope.md > What "Working" Looks Like`.

- Put items with quantities from 0 through 4 in the red Low Stock bucket; sort them by quantity ascending, including zero-stock items at the very top.
- Put items with quantities from 5 through 9 in the yellow Out Soon bucket.
- Put items with quantities of 10 or more in the green Safe bucket.
- Show each card's item photo when available, otherwise a placeholder icon; show the item name and quantity left.
- Tapping Sold -1 decreases a positive quantity by one and immediately updates the displayed quantity and bucket/order as appropriate.
- At quantity 0, keep the item visible at the top of Low Stock. Show “OUT OF STOCK” and “0 left” in bold red, style the card darker red, disable the Sold -1 button in gray, and show “Cannot sell at 0 - restock first”. Do not allow the quantity to go negative.
- Tapping Add +1 increases the quantity by one and immediately updates the displayed quantity and bucket/order as appropriate.
- Show the Low Stock item count in a message such as “Need to restock: 5 items.”
- **Acceptance criteria**
  - [ ] An item with quantity 2 appears in Low Stock ahead of an item with quantity 3; an item with quantity 5 appears in Out Soon; an item with quantity 10 appears in Safe.
  - [ ] Tapping Sold -1 on an item with quantity 2 changes it to 1 immediately. When it reaches 0, the specified out-of-stock treatment is visible, and another sale cannot reduce it below 0.
  - [ ] Tapping Add +1 on an item with quantity 0 changes it to 1 immediately and removes the out-of-stock treatment.

### Search

Develops `scope.md > The POC Boundary`.

- The dashboard search bar finds items by name and filters the visible inventory.
- If nothing matches, show “No results for [search text]” and a Clear Search button. Clearing the search restores the dashboard list.
- **Acceptance criteria**
  - [ ] Searching for an existing item shows the matching item.
  - [ ] Searching for a name with no match shows the no-results message; Clear Search restores the items.

### Add an item

Develops `scope.md > The Core Loop` and `scope.md > The POC Boundary`.

- Let the owner take or choose an optional photo and preview it. An item without a photo uses a placeholder icon.
- Require a non-empty item name. If it is blank, block saving and show “Enter item name”.
- Accept a quantity as a whole number zero or greater. A blank quantity is treated as 0. If the owner enters letters, a negative number, or a decimal, block saving and show “Enter whole number 0 or more”.
- Save a valid item and show it immediately in the appropriate dashboard bucket.
- **Acceptance criteria**
  - [ ] An item with a name and blank quantity saves as quantity 0 and appears in Low Stock.
  - [ ] An item can be saved without a photo and displays the placeholder icon.
  - [ ] A blank name blocks saving and shows “Enter item name”.
  - [ ] Letters, negative values, and decimal quantities block saving and show “Enter whole number 0 or more”.
  - [ ] A valid item with a photo can be saved and appears with its photo in the appropriate bucket.

## States and Boundaries

- **First use:** With no items, show a friendly message: “No items yet. Tap + Add Item to start tracking your stock” with an illustration and a clear next action. Do not show a blank screen.
- **Search with no match:** Show “No results for [search text]” and a Clear Search button.
- **Invalid item:** Keep the add form open, show the relevant validation message, and do not save until the required name and valid quantity are supplied.
- **Zero stock:** Keep the item visible as the most urgent item in Low Stock and disable selling below zero, as described under `Features and Behavior > Stock dashboard and item updates`.
- **Return and offline use:** Items, photos, names, and current quantities remain available as left after closing and reopening the app. After the first load, the owner can use the app offline. No login is required.

## Product Decisions

- The first audience is small phone-accessories, mobile-phone, and computer-accessories shops with no more than 10 people.
- The owner needs a quick, prioritized stock check; the visible proof is sorted Low Stock items and live quantity changes.
- Stock buckets are red for 0–4, yellow for 5–9, and green for 10 or more; zero-stock items stay visible and are most urgent.
- Photos are optional to make adding items quick; without one, show a placeholder.
- Blank quantity means zero; names are required; quantities must be whole numbers zero or greater.
- Saved items remain available after reopening and offline after the first load, without an account.
- Visual direction is clean, simple, and fast, with a white background, dark text, large fonts and buttons, rounded cards, and red/yellow/green urgency colors.

## What We're Building

A single-owner stock dashboard and add-item form for small phone and computer-accessory shops: add named items with optional photos and non-negative whole-number quantities, find items by name, record sales and stock additions, and use a sorted, color-coded dashboard to identify restock needs. Items and photos remain available after reopening and during offline use after the first load.

## Deferred From the POC

- Multiple staff, shared inventory, or simultaneous updates — the first version is for one owner and one device.
- Accounts or cloud sync — not needed for the single-owner demo.

## Possible Later Enhancements

Other stock-management or restocking capabilities can be considered after the core prioritized restock flow is demonstrated. No specific later features have been selected.

## Non-Goals

- The first version is focused on phone and computer accessories; other business types and capabilities are outside this proof of concept.

## Open Questions

- No product-defining questions remain before `4-spec`. The exact dashboard-to-add-form transition is intentionally left open for the technical plan, provided the described user journey remains clear.
