# Concept 3 — Section Nav + Context Panel

## Overview

A three-column structure inspired by VS Code and Notion. The leftmost column is a narrow 60 px section nav containing only icon buttons; it selects the active section. The second column is a 260 px context panel whose content changes entirely based on the active section — it acts as both a navigation aid and a control surface. The third column is the main content area: the drawing canvas in Workspace, or a scrollable report in Annotations and Cost Table.

There is no top bar. The application name, job details, page navigation, tool controls, and status indicators all live inside the left structure. The main content area is therefore a clean, uninterrupted rectangle with no chrome above or below it — the only thing that overlaps the canvas is the minimap, which is embedded in the context panel rather than floating on the canvas.

Data entry (naming rooms, labelling measurements, labelling area polygons) happens inline inside the context panel rather than in a separate dialog or modal — the panel slides a compact form in from the top, replacing the normal panel content, and slides it back out when the action is saved or cancelled. Destructive confirmations use an inline replacement pattern: the triggering button is replaced in-place by a compact confirm/cancel pair.

Auth pages show the same three-column shell in ghost state (section nav icons present but disabled, context panel empty with a centred card), so users can orient themselves immediately on first visit.

---

## Page: Sign In

The three-column shell is rendered with reduced opacity and no interactivity on the section nav and context panel. The main content area holds the sign-in form as a centred card on a plain light surface (no gradient, no background illustration).

**Section nav (left, ghost state):**
- App logomark in the top slot — not interactive
- Workspace, Annotations, Cost Table icon slots are greyed and non-interactive
- No status or settings icons shown

**Context panel (middle, ghost state):**
- Completely empty — background surface only

**Main content area:**
- Plain white (light) or off-white (dark/brand) background
- A card is centred both horizontally and vertically:
  - App logomark (larger than the section nav version) at top of card
  - App name "Take-off" as a heading
  - Tagline: "Quantity take-offs from construction drawings."
  - Email address input
  - Password input
  - Error message area (inline, below password; hidden until sign-in fails)
  - "Sign in" primary button (full-width within card)
  - "Forgot password?" link (right-aligned)
  - Divider
  - "Don't have an account? Create one" link

On viewports narrower than 768 px the section nav and context panel are hidden; the main content area fills the full width.

---

## Page: Sign Up

Same ghost-shell layout as Sign In.

**Main content card:**
- App logomark
- Heading: "Create your account"
- Sub-heading: "14-day free trial · No card required"
- Email address input
- Password input (minimum 8 characters; hint text below field)
- Error message area (inline; hidden until account creation fails)
- "Create account" primary button (full-width)
- Success state: form replaced by — "You're in. Check your inbox for a confirmation email. Your 14-day trial starts now." with a "Open workspace →" link
- Divider
- "Already have an account? Sign in" link

---

## Page: Access Restricted

Ghost shell, main content holds the restriction card.

**Main content card:**
- Small locked-document illustration at top
- Heading: "Your trial has ended"
- Short body paragraph: lists what is included with a subscription
- "Upgrade" button — visually disabled, with note beneath: "Payment integration coming soon — contact us to arrange access."
- "Sign out" link
- A status badge at the top-right of the card: "Trial expired" in amber

---

## Section Nav

A 60 px wide vertical strip that spans the full height of the viewport. Always visible across all authenticated pages.

**Slots (top to bottom):**

1. **App logomark** — 28 px logomark centred in the strip. Clicking opens a small floating menu anchored to the right of the logo: "About Take-off", separator, "Sign out".

2. **Workspace** icon button (canvas/drawing icon) — activates Workspace section. Tooltip: "Workspace".

3. **Annotations** icon button (tag/label icon) — activates Annotations section. A badge on the icon shows the total room count when annotations exist. Tooltip: "Annotations".

4. **Cost Table** icon button (table/grid icon) — activates Cost Table section. Tooltip: "Cost table".

5. **Spacer** (flex-grow)

6. **Status** icon button (circle indicator) — the circle is green (online, paid), amber (warning: trial nearly expired or offline), or grey (trial active, online). Clicking opens a floating popover anchored to the right of the button with full status detail: trial days remaining or subscription state, online/offline status, cached auth note if applicable. No label; the colour communicates state at a glance. Tooltip: "Account status".

7. **Settings** icon button (gear icon) — reserved; clicking shows a popover: "Settings coming soon." Tooltip: "Settings".

The active section button is indicated by a coloured left-edge accent bar (3 px, primary colour) and a slightly elevated background on the icon itself. Hover state on all buttons: a small label appears to the right of the icon (equivalent to a tooltip but rendered as an adjacent label that slides in).

---

## Context Panel

A 260 px wide panel immediately to the right of the section nav, full height. Its content is entirely replaced when the active section changes. The panel header is pinned at the top; below it is a scrollable body.

---

### Context Panel — Workspace Section

**Panel header (pinned):**
- Job name as editable inline text. Click to edit; pressing Enter or blurring commits. A pencil icon appears on hover. A "↺" reset icon appears when the name differs from the filename default.
- Below the job name: page navigation controls — `‹ 1 / 3 ›` with disabled left/right arrows on first/last page. Hidden when no drawing is loaded or the job has only one page.

**Panel body (scrollable):**

*Load area (always at top):*
- When no drawing is loaded: a compact bordered drop zone with a cloud-upload icon, text "Drop a PDF or click to browse", and a browse files link. Full-width, approximately 80 px tall.
- When a drawing is loaded: a compact row showing the filename (truncated) and a "Clear workspace" icon button (dustbin icon with destructive hover style). Clicking shows the inline confirmation (see Compact Confirmations).

*Paper size and scale selectors (below load area):*
- Two compact select controls side by side: paper size (A1/A2/A3/A4) and scale (1:10 → 1:80). Displayed as plain text with a small chevron. Together they read like `A1 · 1:50`. Disabled until a drawing is loaded.

*Separator*

*Tool palette (always visible when a drawing is loaded):*
- Six tool rows stacked vertically. Each row: icon + label (e.g. "Legend", "Count", "Annotate", "Measure", "Area", "Erase"). The active tool row has a coloured left-edge accent and highlighted background. Clicking a row makes it the active tool. Tools that have a prerequisite (Count requires at least one legend item; all modes require a drawing) are dimmed and show a brief hint on hover.
- Below the palette a thin separator, then the active tool's controls (see Tool Control Sections below).

*Separator (below tool controls)*

*Minimap:*
- A 220 × 170 px thumbnail of the current page, centred in the panel. A semi-transparent viewport rectangle shows the visible portion. Clicking or dragging in the minimap pans the canvas. Below the minimap: a row of zoom controls — "−", zoom percentage, "+", "Reset" — laid out as a compact horizontal row. All disabled when no drawing is loaded.

*Panel footer (pinned at bottom, above the section nav spacer):*
- "Save" icon + label — triggers immediate save. Disabled when no drawing is loaded.
- "Download PDF" icon + label — disabled when no drawing is loaded.
- Both are compact full-width rows with an icon on the left and label on the right.

---

#### Tool Controls — Legend

- Instruction: "Draw rectangles around the legend regions on the drawing."
- "Remove last region" button — disabled when no regions exist
- "Scan legend" primary button — disabled when no regions exist. Replaced by a loading spinner while OCR runs. After completion, an inline result banner replaces the button temporarily: "X items found" or a warning if no labels were identified.
- Separator
- **Legend items list** — each row: colour swatch, editable label (click to edit inline), remove icon. Compact row height.
- "Add item" link below the list
- Separator
- "Clear legend" danger button — inline confirmation on click (see Compact Confirmations)

---

#### Tool Controls — Count

- **Legend items list** — each row: colour swatch, label, count badge (total across all pages). Clicking a row selects it as the active item; selected row has a highlighted background.
- Instruction (below list): "Click on the drawing to place marks."
- Separator
- "Undo last mark" button — disabled when no marks exist
- "Clear marks on this page" button — disabled when no marks exist on this page
- Separator
- "Export counts CSV" button

---

#### Tool Controls — Annotate

- Instruction: "Click to place boundary points. Click the first point to close."
- "Cancel boundary" button — enabled only while a boundary is in progress
- Separator
- Saved rooms list — room name and page number per row (read-only in this view)
- "View annotations →" link — switches the active section to Annotations
- Separator
- "Clear all annotations" danger button — inline confirmation before proceeding

---

#### Tool Controls — Measure

- Instruction: "Click to place a start point, then click for the end point."
- "Cancel measurement" button — enabled only while a measurement is in progress
- Separator
- **Measurement categories list** — each row: colour swatch, editable label, total distance in metres, remove icon. Clicking a row makes it the active category.
- Controls disabled when no categories exist

---

#### Tool Controls — Area

- Instruction: "Click to place polygon points. Click the first point to close."
- "Cancel polygon" button — enabled only while a polygon is in progress
- Separator
- **Area categories list** — each row: colour swatch, editable label, total area in m², remove icon. Clicking a row makes it the active category.
- Controls disabled when no categories exist

---

#### Tool Controls — Erase

- Instruction: "Click any count mark, measurement line, or area polygon on the drawing to remove it."
- No other controls.

---

### Context Panel — Annotations Section

**Panel header (pinned):**
- "Annotations" heading
- Sub-heading: total room count ("3 rooms across 2 pages") or empty-state note

**Panel body (scrollable):**
- "Download annotated PDF" compact full-width button at top
- Separator
- **Room list** — one row per annotation: room name, page tag. Clicking a row scrolls the main content area to that room's section.
- Separator
- "Clear all annotations" danger button — inline confirmation before proceeding
- If no annotations exist: an empty-state message — "No room annotations yet." with a sub-line "Switch to Workspace and use Annotate mode to draw boundaries."

---

### Context Panel — Cost Table Section

**Panel header (pinned):**
- "Cost table" heading
- Scale note inline: "at 1:50" (or "— no drawing loaded" in muted text)

**Panel body:**
- **Grand total** displayed prominently — a large number with a "£" prefix, labelled "Grand total" in small muted text above it. Updates live as inputs change in the main content area.
- Separator
- "Download annotated PDF" compact full-width button
- Separator
- **Item-type filter** — three toggle buttons in a row: "All", "Counts", "Measurements / Areas". Selecting one filters the rows shown in the main content table.
- If no data exists: empty-state message with a note to switch to Workspace and begin counting or measuring.

---

## Canvas (Main Content Area — Workspace)

Occupies the entire remaining width to the right of the context panel, full viewport height. No chrome above or below.

When no drawing is loaded: a large drop target fills the canvas with a centred prompt — upload icon, "Drop a PDF here", and "or use the panel to browse files". Responds visually to drag-over.

When a drawing is loaded:
- The PDF drawing is centred in the canvas. The user can pan by dragging.
- Markup overlays composited over the drawing in real-time.
- Interactive markup for the active tool is drawn as the user places points.

**Canvas overlay — mode label pill:**
- A small pill anchored to the top edge of the canvas (horizontally centred) showing the active mode name. Purely informational; not interactive.

There are no other persistent overlays on the canvas. The minimap and zoom controls live in the context panel. No floating panels, no overlays from the tool system.

---

## Inline Data Entry — Name Room

When the user closes an annotation boundary, instead of opening a dialog, the context panel (in Workspace section, Annotate tool controls) transitions:

The annotate tool controls slide out upward and a compact "Name this room" form slides down in their place:
- "Room contents" read-only list — legend items and counts found inside the boundary. If none: "No items found inside this boundary."
- "Room name" label and text input (pre-populated with a default name)
- "Save" primary button and "Cancel" link side by side

On Save or Cancel the form slides back out and the normal annotate controls slide in.

---

## Inline Data Entry — Label Measurement

When the user places both measurement endpoints, the measure tool controls transition:
- Calculated distance in large bold text ("8.450 m"), with "At scale 1:50 on A1" in small muted text below
- "Label" input with an existing-label datalist. Hint: "New label creates a category; existing label adds to its total."
- "Save" button and "Cancel" link

On Save or Cancel the normal measure controls return.

---

## Inline Data Entry — Label Area

When the user closes an area polygon, the area tool controls transition:
- Calculated area in large bold text ("24.75 m²"), with scale note below
- "Label" input with datalist. Hint text.
- "Save" and "Cancel"

On Save or Cancel the normal area controls return.

---

## Main Content Area — Annotations

When the Annotations section is active the main content area becomes a scrollable report, identical in content to Concept 1's Annotations Report page but without a top bar — the full height is report content.

**Layout:**
- A pinned sub-header at the top of the main content area: "Annotations report" heading, "3 rooms · 2 pages" sub-label
- Scrollable body below:
  - **Plain-text summary card**: a bordered box with the plain-text listing. "Copy" button in the top-right corner.
  - **Per-room sections**: room name sub-heading, page tag, data table (Item | Count, Total footer row). One card per room.
  - If no annotations: centred empty-state message, link to switch to Workspace.

---

## Main Content Area — Cost Table

When the Cost Table section is active the main content area is a scrollable cost table view.

**Layout:**
- A pinned sub-header: "Cost table" heading, "Block A — Ground Floor · Scale 1:50 · A1 paper" sub-label
- Scrollable body:
  - **Main cost table** (Item, Qty, Labour, Material, Rate override, Total columns; grouped rows; grand total row; all labour/material/rate inputs editable with live total recalculation). Horizontally scrollable within the content area on narrow viewports.
  - **Measurements detail table** below, shown when measurement categories exist.
  - If nothing to price: empty-state message with a note to switch to Workspace.

---

## Compact Confirmations

Destructive confirmations appear as inline replacements within the triggering element.

**Clear workspace** — the compact filename row in the context panel's load area expands: the clear icon button is replaced by "Clear workspace? This removes the drawing and all markup." with "Confirm" (danger style) and "Cancel" buttons on the same row. Clicking outside or Cancel collapses back.

**Clear legend** — the "Clear legend" button is replaced in-place by "Clear legend? [Clear] [Cancel]" with no layout shift.

**Clear annotations** (in Annotate tool controls or Annotations panel) — same inline replacement pattern on the triggering button.

---

## Notification System

- **Offline banner**: a 30 px strip inserted above all content (pushing the section nav and context panel down slightly) when the device is offline. Text: "You are offline — changes are saved locally." Auto-hides on reconnection.
- **Status icon indicator**: the section nav Status icon changes colour to amber when offline or when trial is nearly expired. Clicking it shows the full detail popover.
- **Save feedback**: a brief toast in the top-right corner of the main content area — "Saved" (tick icon) on success or an error reason on failure. Auto-dismisses after 3 seconds.
- **Scan result**: inline within the Legend tool controls section of the context panel, in a result banner that replaces the Scan button temporarily.
- **Offline-ready notice**: a brief toast in the top-right corner of the main content area — "Available offline" — auto-dismisses after 3 seconds.
- **PWA install prompt**: a compact dismissible card pinned to the bottom of the context panel, above the footer links. "Install Take-off as an app" with an "Install" button and "×" dismiss link.
