---
doc: checklist
status: approved
---

# Build Checklist

Build mode: fast

## Slices

- [ ] **1. You can see and update a saved restock dashboard**
  Becomes usable: SabiStock runs locally; the owner can add an item with a name and whole-number quantity, see it in the correct urgency bucket, and tap Sold -1 or Add +1 to update stock. Items remain after a reload.
  Why now: This puts the unique kernel—the prioritized Low Stock list and live quantity updates—into the first working slice. Adding and changing an item exercise the full page-to-storage path while keeping the project usable.
  PRD ref: `prd.md > The Core Journey`, `prd.md > Screens and Layout`, `prd.md > Features and Behavior > Stock dashboard and item updates`
  Spec ref: `spec.md > How This Works, In Plain Language`, `spec.md > Where It Runs and How Someone Tries It`, `spec.md > Components > index.html — Dashboard and bucket rendering`, `spec.md > Components > index.html — Inventory state and browser storage`, `spec.md > Data Model`, `spec.md > File Structure`
  Build: Create `index.html` with embedded CSS and JavaScript; implement the mobile dashboard, add form with name and quantity, urgency buckets, sorting, quantity actions including the zero-stock guard, and persistence under `sabistock_items`. Start a local static server as described in the spec.
  Verify (mechanical): Start the local server and open the app. In browser storage, set up items at quantities 0, 2, 3, 5, and 10; confirm bucket placement and ordering, restock count, and zero-stock disabled sale. Use the UI to update quantities, reload, and confirm the changes persist. Check the browser console for runtime errors.
  Learner check: Open the dashboard, add a named item, use Sold -1 and Add +1, and reload the page. Confirm the item appears in the expected bucket with its updated quantity still saved.
  Commit: `Build saved stock dashboard`

- [ ] **2. You can find items and recover from empty or invalid input**
  Becomes usable: Search filters inventory by item name; the no-items, no-search-results, and invalid-input states explain what to do next.
  Why now: Once the core dashboard and saved items work, search and clear recovery make it practical to use the inventory. Validation protects saved data while preserving the learner-approved blank-quantity-as-zero behavior.
  PRD ref: `prd.md > Features and Behavior > Search`, `prd.md > Features and Behavior > Add an item`, `prd.md > States and Boundaries`
  Spec ref: `spec.md > Components > index.html — Search and item actions`, `spec.md > Components > index.html — Add-item form and validation`, `spec.md > Important Failure Modes`
  Build: Add instant name search and Clear Search, the first-use empty message and action, the no-results message, and exact name/quantity validation messages. Keep the form open on invalid input and save blank quantity as zero.
  Verify (mechanical): Search for a present item and a missing name; confirm filtering and Clear Search. Test blank name, blank quantity, letters, negative and decimal quantities, and valid zero; confirm exact messages and that invalid items are not saved. Confirm empty inventory shows the first-use action. Check the browser console.
  Learner check: Search for one item you added, search for a name that is not there, clear the search, then try saving with an empty name and with a blank quantity. Confirm the messages and that blank quantity creates a zero-stock item.
  Commit: `Add inventory search and validation`

- [ ] **3. You can add and keep a small item photo**
  Becomes usable: Owners can take or choose an optional photo, preview it, and save it with a compressed item thumbnail; items without photos retain the placeholder.
  Why now: Photos complete the specified add-item journey. Doing this after inventory and validation exist isolates image/storage risks and makes them easier to verify without delaying the restock kernel.
  PRD ref: `prd.md > Screens and Layout`, `prd.md > Features and Behavior > Add an item`, `prd.md > States and Boundaries > Return and offline use`
  Spec ref: `spec.md > Components > index.html — Photo selection and compression`, `spec.md > Data Model`, `spec.md > Important Failure Modes`, `spec.md > Decisions and Open Issues`
  Build: Add camera/gallery file input, preview, canvas resize within 300 × 300, JPEG compression starting at quality 0.6 and reducing as needed to meet the 20 KB image-data cap, and persistence of the resulting data URL. Surface photo and storage errors without pretending the item was saved.
  Verify (mechanical): Select a photo and confirm the preview, dimensions, JPEG encoding, image-data size at or below 20 KB, persistence after reload, and rendered thumbnail. Add an item without a photo and confirm the placeholder. Exercise an unreadable or unsupported image and a storage-write failure where feasible; confirm a clear error and no false success. Check the browser console.
  Learner check: Add an item with a photo, reload, and confirm its thumbnail remains. Add another without a photo and confirm the placeholder appears.
  Commit: `Add compressed item photos`

## Hands-on Checkpoints

- [x] Early usable behavior explored — after slice 1; learner added an item, tried Sold -1 and Add +1, reloaded to confirm persistence, and reported no confusing parts or requested changes.
- [ ] Final kick-the-tires exploration and feedback completed — after all slices, including search, validation, photos, and persistence.

## Final Review

- [ ] Final review complete — feedback resolved and learner confirms ready to ship

## Code Tour and App Map

- [ ] Learning activity complete — guided route, focused alternative, prior practice connected, or brief recap
- [ ] Optional edit and transfer reflection addressed — offered/declined/already covered/not applicable as appropriate
- [ ] `devpost/app-map.html` generated from finished code, checked, and shown, including a project-grounded practice to reuse

Activity and evidence: [what actually happened; real document/test/code references; unfinished work if interrupted]
Route and stops: [actual paths and symbols; guided stops completed, or reference-only route]
Edit outcome: [tried/kept/reverted/declined/not applicable; verification if changed]
Reflection: [offered/answered/declined/already covered — personal answer belongs only in the ignored profile]
Activity mode: [live app and editor, explicit static fallback, focused alternative, prior practice, or recap]

## Revisions
