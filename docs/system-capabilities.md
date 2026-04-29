# Take-off — System Capabilities

Take-off is a browser-based tool for annotating PDF construction drawings and generating quantity take-off reports.

---

## User Accounts and Access

### Account Creation

A new user can create an account by providing an email address and a password of at least 8 characters. On success, a 14-day free trial begins and the user is informed that an email confirmation has been sent to them. If account creation fails, the reason is shown. The user can navigate between the sign-in and sign-up flows.

### Signing In

A returning user can sign in with their email address and password. On success, they are taken to the main workspace. If sign-in fails, the reason is shown.

### Access Restrictions

Every time the application loads, it verifies the user's authentication and subscription status before displaying any content. An unauthenticated user is directed to sign in. An authenticated user whose trial has expired and who does not have a paid subscription is shown information about what is included with full access and directed to contact the team. An **Upgrade** action is present but currently unavailable pending payment integration.

### Access Status Indicator

All protected areas of the application show the user's current access status: either that they have an active paid subscription, or how many days remain in their free trial. When fewer than 3 days remain, this indicator is more prominent. A user can sign out from the restricted-access screen.

### Offline Authentication

When the device has no network connection, the application falls back to a locally cached snapshot of the user's access status, allowing continued access if the cached status confirms the user had access. The access status indicator shows when the locally cached status is in use rather than a live check.

---

## Connection and Availability

The application can be used without an internet connection after the first visit. On the first visit, all application assets are cached locally so subsequent visits work without network access.

The application shows a notification whenever the device is offline, confirming that changes continue to be saved locally. When online and the application has been successfully cached for offline use, the user is given a subtle indication that offline use is available.

Browsers that support it may offer the option to install the application on the device as a standalone app.

---

## Loading a Drawing

The user can load a PDF file as the active drawing.

When the application starts, the previously saved session is restored automatically, including the active drawing and all markup.

---

## Jobs

Each PDF represents a job. When a PDF contains multiple pages, each page is considered a drawing for the job.

When a PDF is loaded, it is associated with a job name. The job name is displayed whenever a page of the PDF is displayed. By default the job name is derived from the PDF filename. The user can change the job name. The user can choose to revert the job name to the PDF filename.

The job name is preserved when the session is saved and restored on reload.

---

## Drawing Navigation

**Zoom:** The user can zoom the active drawing in or out, between 50% and 400%. The view adjusts so that the area of interest remains centred. The zoom can be reset to 100%.

**Page navigation:** For jobs with multiple drawings, the user can move to the previous or next drawing. They can identify which drawing is currently displayed and can see how many drawings are in the job.

**Scrollable drawing:** When the drawing is larger than the visible area, the user can pan the view in any direction to view different parts of the drawing.

**Map overview:** A miniature thumbnail of the current page is shown when a drawing is loaded. The portion of the drawing currently visible is indicated on the thumbnail. The user can navigate to any part of the drawing by selecting that location on the thumbnail.

---

## Editing Modes

The user works in one of several editing modes at a time. The currently active mode is indicated. Modes that require a drawing are unavailable until one is loaded. Count mode additionally requires at least one legend item to exist.

If the user switches mode while a room boundary, area polygon, or measurement is in progress and incomplete, the incomplete item is discarded.

The available modes are: **Legend**, **Count**, **Annotate**, **Measure**, **Area**, and **Erase Item**.

---

## Legend Processing

The legend on a construction drawing lists the symbols used in the drawing. Take-off allows the user to identify the legend region and extract its contents as named items that can then be counted.

### Selecting the Legend Region

In Legend mode, the user marks one or more rectangular regions on the drawing that contain the legend. The user can remove the most recently drawn region. Multiple regions can be used to cover legends that span multiple columns.

### Scanning the Legend

When at least one region is marked, the user can run OCR on those regions to extract the legend item labels. On success, the recognised items appear in the legend items list and the mode switches automatically to Count mode. If OCR returns no results or falls back to placeholder labels, the user is informed.

The user can re-run the scan at any time to update the legend items from the existing regions without reselecting them. Rescanning is unavailable when no regions have been marked.

### Adding Items Manually

The user can add a new legend item at any time once a drawing is loaded, using a default label that can be renamed. This allows items missed by OCR to be added, or items to be created without scanning.

### Editing Item Labels

The user can rename any legend item. Renaming an item updates its label everywhere it appears in the application.

### Removing Items

The user can remove individual legend items. Removing an item also removes all count marks associated with that item. The removal controls are unavailable when no items exist.

### Clearing the Legend

The user can clear all legend regions and legend items at once. This also resets the selected item and returns to Legend mode. This action is unavailable when no regions exist.

---

## Counting Symbols

Count mode lets the user mark occurrences of each legend item on the drawing.

### Placing Marks

The user selects a legend item to count, then marks each occurrence on the drawing. Marks are colour-coded to their associated legend item. Each item's total count across all pages is displayed.

### Managing Marks

- The user can undo the most recently placed mark. This action is unavailable when no marks exist.
- The user can remove all marks on the current page. This action is unavailable when no marks exist on that page.
- In Erase Item mode, the user can remove individual marks on the current page by selecting them.

### Exporting Counts

The user can download a CSV file containing each legend item label and its total count across all pages.

---

## Annotating Rooms

Room annotation lets the user draw a boundary around a room or area on the drawing and record which counted items fall inside it.

### Drawing a Boundary

In Annotate mode, the user places a sequence of points to define a polygon boundary. Once three or more points have been placed, the user can close the polygon by connecting back to the first point. The user can cancel the boundary in progress at any time, discarding all placed points.

### Naming the Room

When a boundary is closed, the user is shown a list of the legend items and their counts found inside the polygon. If no marks fall inside, this is stated. The user provides a name for the room or area and saves the annotation. If no name is provided, a default name is used. The user can cancel to discard the annotation without saving.

Completed boundaries are shown on the drawing with the room name displayed inside.

### Annotations Report

The user can view a report of all saved room annotations. The report includes:

- A plain-text summary listing all room names and their item counts, which the user can copy to the clipboard.
- A section for each annotated room showing the room name, which page of the drawing it came from, and a breakdown of item label counts with a total.

If no annotations exist, the user is directed to create some.

### Clearing Annotations

The user can permanently delete all room annotations. A confirmation is required before deletion proceeds. This action is unavailable when no annotations exist.

---

## Measuring Distances

Distance measurement lets the user record labelled linear distances on the drawing and accumulate totals per label category.

### Recording a Measurement

In Measure mode, the user marks a start point and an end point on the drawing. The calculated distance in metres (to 3 decimal places) is then shown, and the user is asked to assign a label. Entering a new label creates a new measurement category; entering an existing label adds the distance to that category's total. The user can cancel to discard the measurement. Completed measurements are shown on the drawing colour-coded by their category.

The user can cancel after placing the start point but before placing the end point.

### Measurement Categories

The user can see a list of measurement categories with each category's total accumulated distance in metres. Selecting a category sets it as the active category for new measurements and switches to Measure mode.

### Editing and Removing Categories

- The user can rename measurement category labels. Renaming updates the label everywhere it appears.
- The user can remove individual measurement categories. Removing a category removes all its associated measurements.
- These controls are unavailable when no measurement categories exist.

### Erasing Individual Measurements

In Erase Item mode, the user can remove individual measurement lines on the current page. If removing a measurement leaves its category with no remaining measurements, that category is also removed.

### Page Size and Drawing Scale

The user can specify the paper size of the drawing (A1, A2, A3, A4; default A4) and the drawing scale (options 1:10 through 1:80; default 1:20). Together these determine how pixel distances on the drawing correspond to real-world metres. Changing either setting updates all displayed measurement values. Both settings are unavailable until a drawing is loaded.

---

## Measuring Areas

Area measurement lets the user record labelled polygonal areas on the drawing and accumulate totals per label category, expressed in square metres.

### Recording an Area

In Area mode, the user places a sequence of points to define a polygon. Once three or more points have been placed, the user can close the polygon. The calculated area in square metres (to 2 decimal places) is shown, and the user is asked to assign a label. Entering a new label creates a new area category; entering an existing label adds the area to that category's total. If no label is provided, an auto-generated name is used. The user can cancel to discard the area. Completed polygons are shown on the drawing colour-coded by their category. The user can cancel the polygon in progress at any time before closing it.

### Area Categories

The user can see a list of area categories with each category's total accumulated area in square metres. Selecting a category sets it as the active category for new areas and switches to Area mode.

### Editing and Removing Categories

- The user can rename area category labels. Renaming updates the label everywhere it appears.
- The user can remove individual area categories. Removing a category removes all its associated area polygons.
- These controls are unavailable when no area categories exist.

### Erasing Individual Areas

In Erase Item mode, the user can remove individual area polygons on the current page. If removing a polygon leaves its category empty, the category is also removed.

### Effect of Scale on Areas

Changing the drawing scale recalculates all area totals automatically, since the stored geometry is in drawing coordinates.

---

## Cost Table

The cost table builds a cost estimate from the items and quantities recorded in the main workspace: counted legend items, measurement categories, and area categories.

If none of these exist, the user is directed to the main workspace to create them.

### Item Rows and Quantities

Each legend item, measurement category, and area category appears as a row. For each row:

- **Legend items** show the item label and the total mark count across all pages as the quantity.
- **Measurement categories** show the category label and the total accumulated distance converted to whole metres (rounded up) at the current drawing scale.
- **Area categories** show the category label and the total accumulated area in square metres (to 2 decimal places).

All item labels and quantities are read-only in this view.

### Cost Columns

For each row the user can enter:

- **Labour** — a per-unit labour cost.
- **Material** — a per-unit material cost.
- **Rate** — an optional per-unit rate override. If left blank, the rate is Labour + Material. If provided, it overrides that sum.
- **Total** — calculated automatically as Quantity × Rate (to 2 decimal places), updating as values are entered.

A grand total row sums all item totals.

### Measurements Detail

When measurement categories exist, a supplementary table shows each category label and its total distance in metres (to 3 decimal places), with a footer row summing all distances. The table header shows the current drawing scale in use.

### Persistence

Entered labour, material, and rate values are saved automatically and persist across page reloads.

---

## Saving and Exporting

### Auto-Save

The application automatically saves all markup state — legend items, count marks, annotation polygons, measurements, area polygons, drawing scale, current page, and job name — along with the PDF, approximately 2 seconds after any change.

### Manual Save

The user can trigger an immediate save at any time. The user is shown confirmation when the save succeeds, or an error message if it fails. The save action is unavailable when no drawing is loaded or a save is already in progress.

### Download Annotated PDF

The user can download a PDF file with all markup composited onto the original drawing. The user is shown a progress indication while the file is being prepared. This action is available from the main workspace and from the annotations and cost table views. It is unavailable when no drawing is loaded.

---

## Clearing the Workspace

The user can discard the current session entirely, unloading the drawing and removing all markup. This resets the workspace to its initial empty state. This action is unavailable when no drawing is loaded.
