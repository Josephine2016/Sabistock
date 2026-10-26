---
doc: spec
status: approved
---

# SabiStock — Technical Spec

## How This Works, In Plain Language

SabiStock is one web page in `index.html`. HTML provides the dashboard and add-item form, CSS makes them clean and easy to use on a phone, and JavaScript responds to taps, sorts items into stock buckets, checks form entries, and saves changes.

The browser keeps the inventory on this device in `localStorage`—like a small sticky note reserved for this app. When the owner adds an item, sells one, or adds stock, the page updates that note. Photos are shrunk to small thumbnails before they are saved with the inventory. There is no account, server database, or online service for the app itself.

We will open the page through a small local web server at `localhost`. That server serves the file from this computer; it does not need the internet while running. Keeping the app in one file with no online libraries keeps the proof of concept small and usable offline. The tradeoff is that data stays in this browser on this device and does not sync to another phone.

## The Core Journey Through the System

PRD ref: `prd.md > The Core Journey`.

1. The owner opens the page served from `localhost`. JavaScript reads the saved `sabistock_items` list from the browser and draws the red, yellow, and green stock buckets. If there is no saved list, the page shows the first-use message and Add Item action.
2. As the owner searches, JavaScript filters the in-memory inventory by item name and redraws the matching cards or the no-results message.
3. When the owner taps Sold -1 or Add +1, JavaScript changes that item's quantity, saves the updated list to `localStorage`, and redraws its card in the correct bucket and order.
4. When the owner adds an item, the page checks its name and quantity, optionally reads and compresses a photo using the browser's built-in canvas, adds the valid item to the list, saves the list, and returns to the dashboard.
5. On a later visit from the same browser and local address, the page reads the saved list again. The owner can see the prioritized restock list without an internet connection while the local server is running.

## Stack

- **HTML, CSS, and JavaScript in one `index.html`** — selected by the learner to keep the app simple, single-file, and free of network dependencies. The browser runs it directly without a build step.
  - [HTML](https://developer.mozilla.org/en-US/docs/Web/HTML)
  - [CSS](https://developer.mozilla.org/en-US/docs/Web/CSS)
  - [JavaScript](https://developer.mozilla.org/en-US/docs/Web/JavaScript)
- **Browser `localStorage`** — selected by the learner for a single-device inventory saved as a JSON array under `sabistock_items`. It is simple, but browser storage is limited and does not sync across devices.
  - [Web Storage API](https://developer.mozilla.org/en-US/docs/Web/API/Web_Storage_API)
- **Canvas API** — selected by the learner for resizing photos to 300 × 300 JPEG thumbnails, starting at quality 0.6 and reducing quality if needed to keep the JPEG image data at or below 20 KB per saved photo. Base64 storage adds some extra text, but the learner accepted this for the small thumbnails.
  - [Canvas API](https://developer.mozilla.org/en-US/docs/Web/API/Canvas_API)
- **No framework, external libraries, CDN, or build tooling.** In particular, do not load Tailwind or other assets from a CDN; the app must not rely on the internet.
- **Local static server for the demo:** Node.js/npm and the `serve` command via `npx` are needed to serve the file at a stable local address. Node.js v22.21.0 and npm v10.9.4 are available in the current environment. `npx` may need internet access the first time it downloads `serve`; the running app does not need internet.
  - [Node.js](https://nodejs.org/)
  - [npm `npx` command](https://docs.npmjs.com/cli/v11/commands/npx)
  - [`serve` package](https://www.npmjs.com/package/serve)

## Where It Runs and How Someone Tries It

- **Runtime:** A modern desktop or mobile browser. The app is a single static page; there are no API keys or accounts.
- **Recommended local demo:** From the project root, run `npx --yes serve .` and open the local URL printed by the command, typically `http://localhost:3000`. Keep the local server running while using the app. Internet access may be needed for the first `npx` download of `serve`; after that, the app and its features use no external network resources. Restart the server when returning to the demo.
- **Direct file opening:** `index.html` has no online dependencies and can be opened as a local file, but browser storage behavior for `file://` addresses can vary. Use the local server for a reliable saved-inventory demo.
- **Offline boundary:** This is not an installable hosted PWA. It works without internet when served locally, but it cannot be reopened from `localhost` while the local server is stopped. No service worker or hosted offline cache is included.
- **Submission:** Record a short demo video and publish the repository on GitHub. Deployment is optional and is not part of this spec; a local browser demo is sufficient.

## Look and Feel

Implement `prd.md > Look and Feel` as a clean, fast, mobile-first page: white background, dark text, large readable type, rounded item cards, and large thumb-friendly controls. Use red for Low Stock, yellow for Out Soon, and green for Safe. Make the zero-stock card darker red and keep “OUT OF STOCK” and “0 left” easy to notice. Avoid decorative elements that compete with the prioritized list. Keep empty-state and validation copy readable and close to the action it explains.

## Components

### `index.html` — Dashboard and bucket rendering

Render the search bar, Low Stock / Out Soon / Safe buckets, restock count, item cards, empty state, and no-results state. Sort Low Stock items by quantity ascending, including zero first. Apply the agreed urgency colors and mobile-friendly layout.

Implements `prd.md > Screens and Layout`, `prd.md > Look and Feel`, `prd.md > Features and Behavior > Stock dashboard and item updates`, and `prd.md > States and Boundaries`.

### `index.html` — Inventory state and browser storage

Read the saved item array when the page opens. Add items and update quantities in memory, then save after each successful change. Re-render the dashboard so bucket membership and ordering reflect the latest quantity. If storage cannot be read or written, show a clear error rather than claiming the change was saved.

Implements `prd.md > The Core Journey`, `prd.md > Features and Behavior > Stock dashboard and item updates`, and `prd.md > States and Boundaries` (Return and offline use).

### `index.html` — Search and item actions

Filter items by name as the owner types. Clear Search resets the filter. Sold -1 cannot reduce a zero quantity; Add +1 increases stock. Keep actions tied to the correct item even when the displayed list is filtered or reordered.

Implements `prd.md > Features and Behavior > Search` and `prd.md > Features and Behavior > Stock dashboard and item updates`.

### `index.html` — Add-item form and validation

Collect the required name, whole-number quantity, and optional photo. Treat a blank quantity as zero. Keep the form open with the agreed message when the name or quantity is invalid. On a valid save, show the new item in the correct dashboard bucket, using a placeholder if there is no photo.

Implements `prd.md > Screens and Layout`, `prd.md > Features and Behavior > Add an item`, and `prd.md > States and Boundaries` (Invalid item).

### `index.html` — Photo selection and compression

Allow taking a photo or choosing one from the gallery, then show a preview. Use the browser's canvas to resize the photo within 300 × 300 pixels and encode it as JPEG. Start at quality 0.6 and reduce quality only as needed to keep the JPEG image data at or below 20 KB before converting it to a base64 data URL for storage. A missing photo is valid and uses the placeholder.

Implements `prd.md > Features and Behavior > Add an item`.

## Data Model

Store a JSON array at the `sabistock_items` key in `localStorage`. Each item contains:

- `id`: stable unique identifier for targeting updates after sorting or filtering.
- `name`: required item name.
- `quantity`: non-negative whole number.
- `photo`: compressed JPEG data URL, or `null` when no photo is selected.

Adding an item appends it to the array. Sold -1 and Add +1 update only the matching item's quantity and save the array immediately. The dashboard derives bucket membership and order from each quantity; bucket labels are not separately stored. On return, parse the array and render it again. No account, cloud sync, or in-app item deletion is included in this proof of concept; the owner can clear browser data manually.

## File Structure

```text
sabistock/
├── index.html                  # Complete mobile-friendly app; embedded CSS and JavaScript
├── .gitignore                  # Keeps personal learner profile and local credentials untracked
└── devpost/
    ├── learner-profile.md      # Personal learning context; ignored by Git
    ├── scope.md                # Approved project boundary
    ├── prd.md                  # Approved product behavior
    └── spec.md                 # This technical blueprint
```

The build skill will add `devpost/checklist.md` before implementation slices begin. No package manifest or generated build output is needed for the app.

## External Services and Dependencies

- **App runtime:** No external API, database, or hosted service. HTML, CSS, JavaScript, Web Storage, and Canvas are built into the browser.
- **Local demo server:** `npx --yes serve .` uses Node.js/npm and the `serve` package to serve the project folder locally. It is a development/demo helper, not an app service or runtime dependency after the page is served.
- **Internet:** Not required by the app. It may be required once to download the local `serve` command. No external fonts, images, scripts, or stylesheets are loaded.

## Important Failure Modes

- **Browser storage is full or blocked** → Show an explicit message that the change could not be saved; do not show a success state or discard the only saved copy silently. The small photo cap reduces but cannot eliminate this risk.
- **Photo cannot be read or compressed below 20 KB** → Keep the form open, explain that the photo could not be prepared, and let the owner choose another photo or save without one.
- **Saved inventory is unreadable** → Show a clear recovery message and do not silently replace the saved data with an empty inventory.
- **Local server is unavailable** → The page will not open at its local address. Start `npx --yes serve .` from the project root and use the URL it prints.

## What Was Simplified and Why

- **One local inventory on one device** instead of accounts, cloud storage, or shared updates — preserves the restock workflow without introducing an online service.
- **Small compressed photo thumbnails** instead of full-resolution images — keeps photos useful on item cards while reducing the chance of filling the browser's limited storage.
- **Local server instead of hosting and a service worker** — enough to record and try the demo without building a deployment or offline-installation system.
- **No external styling framework or CDN** — avoids a network dependency and keeps the app self-contained.

## Decisions and Open Issues

- **Learner choice:** One `index.html` with plain HTML, CSS, and JavaScript, no external CDN or framework — keeps the demo simple and independent of the internet.
- **Learner choice:** Store a JSON array under `sabistock_items` in `localStorage` — keeps one owner's inventory on the same browser and device without a backend.
- **Learner choice:** Resize photos to 300 × 300 and save JPEG image data at or below 20 KB before base64 encoding, starting at quality 0.6 and compressing further only as needed — reduces storage use for an expected 50-item demo; lower photo quality is acceptable for thumbnails.
- **Learner choice:** Run the demo locally with `localhost`; no hosted PWA or service worker — a local server can serve the single file without internet while it is running.
- **Clarified uncertainty:** A hosted page that must reopen offline after closing requires browser caching support and extra files. The learner agreed that a local server demo is enough, so hosted offline caching is out of scope. `file://` storage can vary by browser, so `localhost` is the reliable demo path.
- **Implementation detail:** Generate a stable ID for each item so quantity actions still target the right item after filtering or sorting.
- **Environment checked:** Node.js v22.21.0 and npm v10.9.4 are available for the local demo server.
- **Open issue to verify during build:** Browser storage quotas differ. Test adding representative items and compressed photos, and show an explicit error if a save exceeds the available quota.
