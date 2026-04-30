# Concept 2 — Web App Visual Layout

## Overview

Canvas-dominant, tool-rail design inspired by professional drawing and CAD applications (Figma, Sketch, Affinity Designer). The canvas occupies the full viewport at all times; tools and controls orbit it rather than claiming a fixed slice of space. A narrow vertical icon rail on the left edge selects the active mode. Mode-specific controls appear in a floating panel that hovers over the canvas and can be dismissed. Annotations and Cost Table open as a slide-in drawer from the right, keeping the drawing visible. Data entry (naming rooms, labelling measurements) happens in a bottom sheet rather than a centred modal, so the canvas above it remains visible. A minimal 36 px header bar provides job name, page navigation, scale settings, and the three global actions without the bulk of a traditional top bar.

The fundamental structural difference from Concept 1: nothing is permanently subtracted from the canvas. Every non-canvas element either overlaps it or slides in over it.

---

## Page: Sign In

A **split-screen layout** that fills the viewport.

**Left half (50% of viewport):**
- A large screenshot of the workspace in use (annotated floor plan with count marks and room boundaries visible), with a dark gradient overlay at the bottom
- App name "Take-off" in large white text at the top-left of this panel, with a one-line tagline beneath it: "Quantity take-offs from construction drawings."

**Right half (50% of viewport):**
- White background, vertically centred content
- Small app logomark at the top of the form area
- Heading: "Welcome back"
- Email address input field
- Password input field
- Error message area (inline, below password field; hidden until a failed sign-in attempt)
- "Sign in" primary button (full-width)
- "Forgot password?" link (right-aligned, below the button)
- Divider line
- "Don't have an account? Create one" link

On viewports narrower than 768 px the left panel is hidden and the right form panel fills the full width.

---

## Page: Sign Up

Same split-screen layout as Sign In. The left panel is identical.

**Right half contents:**
- App logomark
- Heading: "Start your free trial"
- Sub-heading: "14 days, no card required."
- Email address input field
- Password input field (minimum 8 characters; hint text beneath the field)
- Error message area (inline; hidden until account creation fails)
- "Create account" primary button (full-width)
- Success state: the form is replaced by a confirmation message — "Check your inbox. A confirmation email has been sent and your 14-day trial has started." — with a "Go to workspace →" link
- Divider line
- "Already have an account? Sign in" link

---

## Page: Access Restricted

Full-viewport layout with a centred floating card over a blurred, dimmed screenshot of the workspace.

**Card contents:**
- A small illustration at the top (a simple icon representing a locked document)
- Heading: "Your trial has ended"
- Short paragraph listing what is included with a full subscription (OCR scanning, counting, annotation, measurement, area, cost table, PDF export, offline access)
- "Upgrade" button — present but visually disabled, with an explanatory note below it: "Payment integration coming soon — contact us to arrange access."
- "Sign out" link below the note

An access status badge in the top-right of the card shows "Trial expired" in amber.

---

## Header Bar

A 36 px strip pinned to the top of the viewport. The smallest possible chrome — no navigation tabs, no section labels.

**Left group:**
- App icon (logomark only, 24 px) — clicking opens a small dropdown menu containing: "About Take-off", a separator, and "Sign out"
- Job name — compact editable inline text. A pencil icon appears on hover to signal editability. A "↺ reset" icon appears beside it when the name has been changed from the filename default.

**Centre group:**
- Page navigation: `‹ Page 1 of 3 ›` — the two arrow buttons and the label. Arrows are disabled on the first/last page respectively. Hidden when no drawing is loaded or the job has only one page.

**Centre-right group:**
- Two compact inline selectors sitting side by side, separated by a dot: paper size (A1/A2/A3/A4) and drawing scale (1:10 through 1:80). Both are disabled until a drawing is loaded. Displayed as plain text with a small chevron indicating they are interactive; clicking opens a small dropdown. Together they read like: `A1 · 1:50`.

**Right group:**
- Offline indicator: a small dot that appears only when offline or when cached auth status is in use. Clicking shows a tooltip explaining the situation.
- Access status badge: compact pill ("11d trial" / "Paid"). Fewer than 3 days remaining renders it in amber with a warning icon.
- "Save" icon button — triggers immediate save. Disabled when no drawing is loaded or a save is already in progress. Tooltip: "Save now".
- "PDF" icon button — downloads the annotated PDF with a progress indicator while generating. Disabled when no drawing is loaded.
- "Annotations" text button — opens the Annotations right drawer. A badge on the button shows the room count when annotations exist.
- "Cost table" text button — opens the Cost Table right drawer.

---

## Tool Rail

A 52 px wide vertical strip pinned to the left edge of the viewport, full height, below the header bar. Contains icons only. Hover tooltips show the mode name.

**Top section:**
- Load drawing icon (folder/upload icon) — opens the file picker to load a PDF. When a drawing is loaded this changes to a "clear workspace" icon (with a distinct destructive style — red tint on hover). Either action is always at the top of the rail for discoverability.

**Mode icons (stacked vertically below a separator):**
- Legend (grid/table icon)
- Count (target/dot icon)
- Annotate (polygon/shape icon)
- Measure (ruler icon)
- Area (filled square icon)
- Erase (eraser icon)

Only one mode can be active at a time. The active icon is highlighted with a primary-colour background. Modes that require a drawing are dimmed and non-interactive until a drawing is loaded. Count mode is additionally dimmed until at least one legend item exists.

**Below the mode icons, a thin separator, then:**
- A "close floating panel" toggle icon — collapses or expands the floating mode panel when a mode is active. This allows the user to temporarily dismiss the panel without changing mode.

---

## Canvas

Occupies the entire viewport below the header bar and to the right of the tool rail. The full canvas area is available to the drawing.

When no drawing is loaded: a full-canvas drop target with a centred prompt — a large upload icon, the text "Drop a PDF here", and a secondary "or browse files" link. The prompt responds visually when a file is dragged over it.

When a drawing is loaded:
- The PDF drawing fills the canvas, centred. The user can pan by dragging.
- Markup overlays (count marks, room boundaries, measurement lines, area polygons, legend region rectangles) are composited over the drawing in real-time.
- Markup for the currently active mode is drawn interactively as the user places points.

**Canvas overlays (always visible when a drawing is loaded):**

- **Floating mode panel** — see section below. Appears on the canvas just to the right of the tool rail.

- **Minimap** — top-right corner of the canvas, always visible. 180 × 140 px thumbnail of the current page. A semi-transparent rectangle indicates the visible portion of the drawing. The user can click or drag within the minimap to pan the main view. A thin border and subtle shadow distinguish it from the canvas.

- **Zoom controls** — bottom-left of the canvas, close to but not overlapping the tool rail. A "−" button, a zoom percentage display, a "+" button, and a "Reset" link, arranged in a compact horizontal pill. Displayed with a frosted-glass background.

- **Mode label pill** — when the floating panel is collapsed: a small pill at the left edge of the canvas showing the active mode name (e.g. "Count mode"). Clicking it re-expands the panel.

---

## Floating Mode Panel

When a mode is activated, a panel appears floating over the canvas, anchored near the top of the canvas just to the right of the tool rail (at approximately x = 68 px from the left edge). It has a width of approximately 280 px and a maximum height that keeps it within the canvas bounds, with internal scrolling if content overflows.

**Panel anatomy:**
- A narrow drag handle bar at the top, allowing the panel to be repositioned by dragging
- Panel header: mode name ("Count mode") on the left, a "×" collapse button on the right
- Below the header: mode-specific content (identical in content to the sidebar panels in Concept 1, described below)
- The panel is rendered with a solid surface background, a subtle border, and a drop shadow to communicate that it floats above the canvas

Collapsing the panel with "×" hides it and shows the mode label pill at the canvas edge instead. The mode remains active.

---

### Floating Panel — Legend Mode

- Instruction text: "Draw rectangles around the legend regions on the drawing."
- "Remove last region" button — disabled when no regions have been drawn
- "Scan legend" primary button — disabled when no regions exist. Replaced by a spinner while OCR runs. After completion, an inline result banner within the panel shows: "X items found" (with a success style), or a warning if none were found or labels fell back to placeholders.
- Divider
- **Legend items list** — one row per item, each showing a colour swatch, the item label (inline-editable on click), and a remove icon button
- "Add item" link below the list
- Divider
- "Clear legend" danger button — disabled when no regions exist; clicking shows an inline confirmation within the panel (replaces the button with "Clear legend? [Confirm] [Cancel]") before proceeding

---

### Floating Panel — Count Mode

- **Legend items list** — one row per item, each showing a colour swatch, the label, and a count badge (total across all pages). Clicking a row selects it as the active item; the selected row is highlighted.
- Instruction below the list: "Click on the drawing to place marks."
- Divider
- "Undo last mark" button — disabled when no marks exist
- "Clear marks on this page" button — disabled when no marks exist on this page
- Divider
- "Export counts CSV" button

---

### Floating Panel — Annotate Mode

- Instruction: "Click to place boundary points. Click the first point to close the boundary."
- "Cancel boundary" button — enabled only while a boundary is in progress
- Divider
- Saved rooms list — room name and page number per row (no actions on individual rows in this panel)
- "View annotations report" link — opens the Annotations drawer
- Divider
- "Clear all annotations" danger button — disabled when no annotations exist; inline confirmation before proceeding

---

### Floating Panel — Measure Mode

- Scale selectors are not in the panel; they live in the header bar.
- Instruction: "Click to place a start point, then click again for the end point."
- "Cancel measurement" button — enabled only while a measurement is in progress
- Divider
- **Measurement categories list** — colour swatch, label (inline-editable), total distance in metres, remove button per row. Clicking a row sets it as the active category.
- Edit and remove controls disabled when no categories exist

---

### Floating Panel — Area Mode

- Instruction: "Click to place polygon points. Click the first point to close the polygon."
- "Cancel polygon" button — enabled only while a polygon is in progress
- Divider
- **Area categories list** — colour swatch, label (inline-editable), total area in square metres, remove button per row. Clicking a row sets it as the active category.
- Edit and remove controls disabled when no categories exist

---

### Floating Panel — Erase Mode

- Instruction: "Click any count mark, measurement line, or area polygon on the drawing to remove it."
- No other controls.

---

## Right Drawer: Annotations

Triggered by the "Annotations" button in the header bar. A panel slides in from the right edge of the viewport, 400 px wide. The canvas remains visible (and interactive) to the left of the drawer. The header bar remains at the top across the full width.

**Drawer header (pinned at top):**
- "Annotations" heading
- "×" close button (right-aligned) — slides the drawer out

**Drawer body (scrollable):**
- "Download annotated PDF" button near the top for convenience
- **Plain-text summary panel**: a bordered box with the plain-text listing of room names and item counts. "Copy" button in the top-right corner of the box.
- **Per-room sections** — one per annotation:
  - Room name as a sub-heading
  - "Page X" tag
  - Table: Item label | Count, with a Total footer row
- If no annotations exist: a centred empty-state message — "No room annotations yet." with a sub-line "Switch to Annotate mode in the workspace to draw boundaries." and a "Close" link.

---

## Right Drawer: Cost Table

Triggered by the "Cost table" button in the header bar. Same 400 px panel mechanism as the Annotations drawer.

**Drawer header (pinned at top):**
- "Cost table" heading with the current scale shown inline (e.g. "at 1:50")
- "Download annotated PDF" icon button (right of heading)
- "×" close button

**Drawer body (scrollable):**
- **Main cost table** — identical structure to Concept 1's cost table (Item, Qty, Labour, Material, Rate, Total columns; grouped rows; grand total row; all labour/material/rate inputs editable with live total recalculation). The table scrolls horizontally within the drawer if needed.
- **Measurements detail table** — shown when measurement categories exist, identical in content to Concept 1.
- If nothing to price: empty-state message with a "Close" link.

---

## Bottom Sheet: Name Room

Appears after the user closes an annotation boundary. Slides up from the bottom of the canvas, 100% canvas width, approximately 220 px tall. The drawn boundary remains visible in the canvas above it.

**Layout (single row, horizontally arranged):**
- Left section: "Room contents" — compact list of legend items and counts found inside the boundary. If none, the text "No items inside this boundary." Displayed as a read-only summary in a muted box.
- Right section: "Room name" label and text input (pre-populated with a default name). Below the input: "Save" primary button and "Cancel" text link side by side.

The bottom sheet has a drag handle at the top edge. It can be dragged down to dismiss (equivalent to Cancel).

---

## Bottom Sheet: Label Measurement

Appears after the user places both endpoints of a measurement. Same bottom-sheet mechanism.

**Layout (horizontally arranged):**
- Left section: the calculated distance in large bold text ("8.450 m"), with "At scale 1:50 on A1" in small muted text below.
- Right section: "Label" input with an existing-label datalist. Hint text: "New label creates a category; existing label adds to its total." Below: "Save" button and "Cancel" link.

---

## Bottom Sheet: Label Area

Same bottom-sheet mechanism, appears after closing an area polygon.

**Layout:**
- Left: calculated area in large bold text ("24.75 m²"), with scale note below.
- Right: "Label" input with datalist. Hint text. "Save" and "Cancel".

---

## Compact Confirmations

Rather than separate dialogs, destructive confirmations appear as **inline replacement panels** within the element that triggered them:

**Clear workspace** — triggered from the top of the tool rail (the load/clear icon). The tool rail's top area expands to show a small card: "Clear workspace? This removes the drawing and all markup." with "Confirm" (danger style) and "Cancel" buttons.

**Clear annotations** — triggered from the "Clear all annotations" button in the Annotate floating panel. The button is replaced in-place by: "Delete all annotations? [Delete] [Cancel]" — the panel does not close or scroll.

---

## Notification System

- **Offline banner**: a 30 px strip that appears above the header bar (pushing the entire interface down slightly) whenever the device is offline. Text: "You are offline — changes are being saved locally." Disappears automatically when connection is restored.
- **Cached auth indicator**: the access status badge in the header bar gains a small cloud-slash icon and a tooltip explaining cached status is in use.
- **PWA install prompt**: a compact dismissible card pinned to the bottom-left of the canvas (above the zoom controls). "Install Take-off as an app" with an "Install" button and "×".
- **Offline-ready notice**: a brief toast in the top-right corner of the canvas — "Available offline" — auto-dismisses after 3 seconds.
- **Save success / error**: a toast in the top-right corner of the canvas (same area as the offline-ready notice). "Saved" on success (with a tick icon), or the error reason on failure. Auto-dismisses after 3 seconds.
- **Scan result**: inline within the floating Legend panel (not a toast), immediately below the "Scan legend" button, in a result banner.
