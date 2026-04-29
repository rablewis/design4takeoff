# Concept 1 — Web App Visual Layout

## Overview

The app has three distinct authentication/access states (Sign In, Sign Up, Access Restricted) and three main content views (Workspace, Annotations Report, Cost Table). Modals appear on top of the Workspace for data-entry steps mid-task. A persistent top bar is shared by all three main content views and provides app-level navigation and status.

---

## Page: Sign In

A centred card on a plain background, roughly 420 px wide.

**Card contents (top to bottom):**
- App name / logo at the top of the card
- Heading: "Sign in"
- Email address input field
- Password input field
- Error message area (hidden until a sign-in attempt fails; shows the reason inline below the password field)
- "Sign in" primary button (full-width)
- Divider line
- "Don't have an account? Create one" link — navigates to Sign Up

No other chrome. The card is vertically centred in the viewport.

---

## Page: Sign Up

Same centred card layout as Sign In.

**Card contents (top to bottom):**
- App name / logo
- Heading: "Create account"
- Email address input field
- Password input field (minimum 8 characters; hint text beneath the field)
- Error message area (hidden until account creation fails; shows the reason)
- "Create account" primary button (full-width)
- Success state (replaces form after success): message confirming a 14-day free trial has started and that a confirmation email has been sent
- Divider line
- "Already have an account? Sign in" link — navigates to Sign In

---

## Page: Access Restricted

Shown to authenticated users whose trial has expired and who have no paid subscription. Same centred card layout.

**Card contents (top to bottom):**
- App name / logo
- Heading: "Your trial has ended"
- Short paragraph describing what is included with full access
- "Upgrade" button — present but visually disabled (grayed out), with a tooltip or note explaining payment integration is coming soon
- "Sign out" text link below the upgrade button

---

## Page: Main Workspace

The primary working view. Three structural regions: a top bar, a left sidebar, and a canvas area.

### Top Bar

Full-width strip pinned to the top of the viewport. Left-to-right:

**Left group:**
- App logo / name (not a nav link — clicking does nothing)
- Job name — displayed as editable inline text. Single-click activates an edit field. A small "reset to filename" icon appears beside the field when the job name has been changed.

**Centre group:**
- Three navigation tabs: **Workspace** (active), **Annotations**, **Cost Table** — switch between the three main content views

**Right group:**
- Access status badge: "Paid" or "Trial — N days left". Fewer than 3 days remaining renders the badge in amber with stronger weight.
- Offline indicator: a small icon that appears only when offline, or when the live auth check failed and cached status is in use. Tooltip explains the situation.
- "Save" icon button — triggers an immediate save. Disabled when no drawing is loaded or a save is in progress. A brief success or error toast appears after the attempt.
- "Download PDF" icon button — downloads the annotated PDF. Shows a progress indicator while generating. Disabled when no drawing is loaded.
- "Clear workspace" icon button (destructive; triggers the Clear Workspace confirmation dialog). Disabled when no drawing is loaded.

### Left Sidebar

Fixed width (~280 px), full height below the top bar, scrollable if content overflows.

**Mode switcher** — a horizontal row of six compact icon+label toggle buttons at the top of the sidebar. Only one is active at a time. The active mode is highlighted. Modes that require a drawing are visually disabled until a drawing is loaded. Count mode is additionally disabled until at least one legend item exists.

Modes: **Legend** · **Count** · **Annotate** · **Measure** · **Area** · **Erase**

Below the mode switcher, the panel content changes to match the active mode.

---

#### Sidebar — Legend Mode

- Instruction text: "Draw rectangles around the legend regions on the drawing."
- "Remove last region" button — disabled when no regions have been drawn
- "Scan legend" primary button — disabled when no regions exist. A spinner replaces it while OCR runs. On completion shows a brief inline result: items found, or a warning if none were found or fallback labels were used.
- Divider
- **Legend items list** — one row per item. Each row shows:
  - Colour swatch (used for count marks)
  - Item label (inline-editable on click)
  - "Remove" icon button — removes the item and all associated marks
- "Add item" text button below the list — adds a new item with a default label ready for renaming
- Divider
- "Clear legend" danger button at the bottom — disabled when no regions exist; opens a brief inline confirmation before proceeding

---

#### Sidebar — Count Mode

- **Legend items list** — one row per item. Each row shows:
  - Colour swatch
  - Item label
  - Count badge showing the total across all pages
  - Clicking a row selects it as the active item for placing marks; the active row is highlighted
- "Undo last mark" button — disabled when no marks exist anywhere
- "Clear marks on this page" button — disabled when no marks exist on the current page
- Divider
- "Export counts CSV" button — downloads a CSV of item labels and totals

---

#### Sidebar — Annotate Mode

- Instruction text: "Click on the drawing to place boundary points. Click the first point to close the boundary."
- "Cancel boundary" button — visible and enabled only while a boundary is in progress; discards placed points
- Divider
- Saved rooms list — one row per annotation showing the room name and the page it came from
- "View annotations report" button — navigates to the Annotations view
- "Clear all annotations" danger button — disabled when no annotations exist; opens the Clear Annotations confirmation dialog

---

#### Sidebar — Measure Mode

- **Scale settings** (two compact selectors side by side):
  - Paper size: A1 / A2 / A3 / A4 (default A4)
  - Drawing scale: 1:10 through 1:80 (default 1:20)
  - Both disabled until a drawing is loaded. Changing either value immediately recalculates all displayed measurements.
- Instruction text: "Click to place a start point, then click again to place the end point."
- "Cancel measurement" button — visible and enabled only while a measurement is in progress
- Divider
- **Measurement categories list** — one row per category. Each row shows:
  - Colour swatch
  - Category label (inline-editable on click)
  - Total distance in metres (3 d.p.)
  - "Remove" icon button — removes the category and all its measurements
  - Clicking the row sets it as the active category and activates Measure mode
- Edit and remove controls on rows are disabled when no categories exist

---

#### Sidebar — Area Mode

- Instruction text: "Click to place polygon points. Click the first point to close the area."
- "Cancel polygon" button — visible and enabled only while a polygon is in progress
- Divider
- **Area categories list** — one row per category. Each row shows:
  - Colour swatch
  - Category label (inline-editable on click)
  - Total area in square metres (2 d.p.)
  - "Remove" icon button — removes the category and all its area polygons
  - Clicking the row sets it as the active category and activates Area mode
- Edit and remove controls are disabled when no categories exist

---

#### Sidebar — Erase Mode

- Instruction text: "Click any mark or measurement line on the drawing to remove it."
- No other controls; the sidebar is otherwise empty in this mode.

---

### Canvas Area

Occupies the full remaining space to the right of the sidebar. The PDF drawing is rendered inside this region. When no drawing is loaded a centred "drop a PDF here or click to browse" upload prompt fills the canvas.

When a drawing is loaded:
- The drawing fills the canvas, clipped to the visible area; the user can pan by dragging
- Markup (count marks, room boundaries, measurement lines, area polygons) is composited over the drawing in real-time

**Canvas overlays — always visible when a drawing is loaded:**

- **Page navigation bar** — centred at the bottom of the canvas, pinned above the canvas edge. Contains a left arrow (previous page), a right arrow (next page), and a "Page X of Y" label between them. Arrows are disabled on the first/last page respectively. Hidden for single-page PDFs.

- **Zoom controls** — bottom-right corner of the canvas. A "–" button, a zoom percentage display (e.g. "150%"), a "+" button, and a "Reset" label/button that returns zoom to 100%.

- **Map overview** — bottom-right corner above the zoom controls. A thumbnail of the current page at reduced scale. A semi-transparent rectangle indicates the currently visible portion. The user can click or drag within the thumbnail to pan the main view.

---

## Dialog: Name Room

Appears as a modal overlay after the user closes an annotation boundary (three or more points placed and polygon closed).

**Contents:**
- Heading: "Name this room"
- Room contents summary: a read-only list of legend item labels and their counts found inside the boundary. If no marks fall inside, a message: "No items found inside this boundary."
- Text input labelled "Room name" — pre-populated with a default name if left empty
- "Save" primary button
- "Cancel" text button — discards the annotation without saving

---

## Dialog: Label Measurement

Appears as a modal overlay after the user places both end points of a measurement.

**Contents:**
- Heading: "Record measurement"
- Calculated distance displayed prominently: "Distance: X.XXX m"
- Text input labelled "Label" with a datalist dropdown suggesting existing category labels
- Hint text: "Enter an existing label to add to its total, or a new label to create a category."
- "Save" primary button
- "Cancel" text button — discards the measurement

---

## Dialog: Label Area

Appears as a modal overlay after the user closes an area polygon.

**Contents:**
- Heading: "Record area"
- Calculated area displayed prominently: "Area: X.XX m²"
- Text input labelled "Label" with a datalist dropdown suggesting existing category labels
- Hint text: "Enter an existing label to add to its total, or a new label to create a category."
- "Save" primary button
- "Cancel" text button — discards the area

---

## Dialog: Confirm Clear Workspace

Triggered by the "Clear workspace" button in the top bar.

**Contents:**
- Heading: "Clear workspace?"
- Body: "This will unload the drawing and remove all markup. This cannot be undone."
- "Clear" danger button
- "Cancel" button

---

## Dialog: Confirm Clear Annotations

Triggered by "Clear all annotations" in the Annotate sidebar panel.

**Contents:**
- Heading: "Delete all annotations?"
- Body: "All saved room annotations will be permanently removed."
- "Delete" danger button
- "Cancel" button

---

## Page: Annotations Report

Shares the top bar with the Workspace (the **Annotations** tab is active). No sidebar. The main area is a scrollable single-column content region centred at a readable max-width (~800 px).

**Content (top to bottom):**

- Page heading: "Annotations report"
- "Download PDF" icon button — same behaviour as in the top bar; reproduced here for convenience
- **Plain-text summary panel**: a bordered box containing a plain-text listing of all room names and their item counts. A "Copy to clipboard" button sits in the top-right corner of the panel.
- **Per-room sections** — one section per annotation, each containing:
  - Room name as a sub-heading
  - "Page X" label indicating which drawing page the room came from
  - A small table: Item label | Count, with a "Total" footer row
- If no annotations exist, centred message: "No annotations yet. Draw room boundaries in the Workspace to create annotations." with a "Go to Workspace" link.

---

## Page: Cost Table

Shares the top bar (the **Cost Table** tab is active). No sidebar. Scrollable single-column region centred at a readable max-width (~960 px).

**Content (top to bottom):**

- Page heading: "Cost table"
- "Download PDF" icon button — reproduced here for convenience
- **Main cost table** — a data table with columns:
  - **Item** (read-only) — legend item label, measurement category label, or area category label
  - **Qty** (read-only) — count (legend items), distance in whole metres rounded up (measurement categories), area in m² to 2 d.p. (area categories)
  - **Labour** — numeric text input, per-unit cost
  - **Material** — numeric text input, per-unit cost
  - **Rate** — numeric text input; tooltip or label explaining it overrides Labour + Material when filled
  - **Total** — read-only, calculated as Qty × Rate, shown to 2 d.p., updates on every keystroke in cost inputs
  - A **Grand total** row at the bottom spanning the Item–Rate columns with the summed total in the Total column
  - Rows are grouped by type (legend items first, then measurement categories, then area categories) with a subtle group header row for each group
- **Measurements detail table** — shown below the main table only when measurement categories exist
  - Heading: "Measurement detail" with the current scale displayed inline (e.g. "at 1:20")
  - Columns: Category label | Total distance (m, 3 d.p.)
  - Footer row: sum of all distances
- If no items, measurement categories, or area categories exist, centred message: "Nothing to price yet. Add legend items, measurements, or areas in the Workspace." with a "Go to Workspace" link.

---

## Notification Toasts and Status Banners

These appear above all page content and do not block interaction.

- **Offline banner** — a slim bar below the top bar, visible whenever the device has no network connection. Text: "You are offline — changes are being saved locally."
- **Offline auth notice** — added to the access status badge in the top bar when cached auth is in use rather than a live check. The badge gains a small icon and tooltip.
- **PWA install prompt** — a dismissible banner below the top bar on browsers that offer app installation: "Install Take-off as an app for faster access." with an "Install" button and a dismiss "×" button.
- **Offline-ready notice** — a brief, subtle toast shown once after all application assets have been successfully cached: "Available offline."
- **Save success / error toast** — bottom-centre, auto-dismisses after 3 seconds. Shows "Saved" on success or the error reason on failure.
- **Scan result toast** — brief inline feedback inside the sidebar after an OCR scan, not a floating toast, to keep context close to the action.
