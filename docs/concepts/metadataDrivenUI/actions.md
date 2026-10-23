# Actions

[← Back to Relational Views Overview](index.md)

The **Actions** page describes the general actions that users can take on a metadata-driven UI.

These actions allow users to add, edit, copy, delete, import, export, navigate, and otherwise interact with relational data in different ways depending on the use case.

## Overview

Actions help turn a Relational View into an interactive experience.

Depending on how a view is configured, users may be able to:

- maintain records
- work with parent and child records together
- update many records at once
- export or import data
- navigate to related views
- refresh and save changes
- switch the current grid into inline add/edit mode

## General Actions

The following general actions can be added to most views.

| Action | Description | Notes |
|---|---|---|
| Add & Edit in Grid | A checkbox-based action that switches the current grid into add/edit mode, allowing the user to click a **+** button to add new records and edit selected existing records directly in the grid. | Records that need to be edited must be selected before the checkbox is checked. |
| Add | Opens a page with an empty grid that you can add records to. | Typically used to create new records. |
| Add+ | Opens a Parent/Children add page. | Useful when users need to enter related records together in a single experience.  Add only to Parent records. |
| Edit | Opens a page with an editable grid populated with the selected records. | Used to update existing records. |
| Edit+ | Opens a Parent/Children editable grids page with the selected records. | Useful when related records need to be maintained together.  Add only to Parent records. |
| Copy | Makes a copy of all selected records. | Helpful for creating similar records without re-entering all values. |
| Spreadsheet | Puts all the data in the view into a spreadsheet component that allows users to add and edit rows in an Excel-like fashion. | Useful for high-volume review and maintenance. |
| Bulk Update | Takes the selected records and allows users to update one or more fields across all selected records. | Useful for applying the same change across many records. |
| Delete | Deletes the selected records. | Use with care, especially for production data. |
| Save | Saves pending changes. | Typically used after data is added or changed. |
| File Export | Exports the records the grid's filters select, to a OneStream spreadsheet, to Excel, or to a JSON data file. | Useful for review, offline analysis, or sharing data. JSON also includes every level of child records - see [File Export and Import as JSON](#file-export-and-import-as-json). |
| File Import | Imports an Excel file or a JSON data file. Rows are shown for review before they are saved. | Useful for loading or updating larger sets of records, or moving them between environments. |
| Navigate | Opens a page showing a tree view of related views that can be navigated to. Users can also navigate within the current view by selecting columns to filter by. | Useful for moving across related data structures and drilling into related records; in addition, to filtering within a views based on the value of a column of all selected records |
| Refresh | Refreshes the current view. | Useful for reloading the latest data after updates or navigation. |

!!!Note Important Note
    When using the Add+ action, be sure to select any new records that you add before you attempt to add any children records.

    When using the Spreadsheet action, be sure to save any changes, using the spreadsheet ribbon, before you click on Import.

## File Export and Import as JSON

Besides Excel, **File Export** can write the grid's data as JSON, and **File Import** reads it back. The file is a [data file](../../extending/import/data.md) - the same format **Load** takes - so it also moves data between environments: lookups are written as integration codes, not ids that differ from one instance to the next.

Typical uses:

- **Moving data between environments** — configuration rows built in development, loaded into test and production
- **Copying a set of records with their children** — an order and all its lines — into another application
- **Editing outside the application** — export, change the file, import it back
- **Handing current data to Claude** as context, or getting a file from Claude to import

### Exporting

1. Filter the grid to the rows you want
2. Click **File Export**
3. Click **Export To Json**

The file is written to your **ExportAndImport** folder in the OneStream File Explorer, named after the view with a timestamp — `OrderHeaders_638…json` — and opened. Repeated exports sit side by side rather than overwriting each other.

**Which rows.** The rows the grid's filters currently select — the same rows the grid shows, including any [Top Count](../../appGroups/admin/relational/relationalViews/views.md#top-count) — then every level of their **child records**. These are the same child views [Add+ and Edit+](#general-actions) work with, so an export of three orders includes their lines, the lines' own children, and so on. Only children of the exported rows are included.

**What the file holds.** One `Data` entry per view, parents first, then each level of children. Rows are flat rather than nested: a child row names its parent by integration code, so it can be read back on its own.

```json
{
  "Data": [
    {
      "ViewId_IntegrationCode": "{RfaExtension}.OrderHeader_mE",
      "Rows": [
        { "OrdHdr_IntegrationCode": "SO-1001", "OrdHdr_CustomerId_IntegrationCode": "ACME", "OrdHdr_OrderDate": "2026-10-01T00:00:00.0000000" }
      ]
    },
    {
      "ViewId_IntegrationCode": "{RfaExtension}.OrderLine_mE",
      "Rows": [
        { "OrdLn_IntegrationCode": "SO-1001-01", "OrdLn_OrderHeaderId_IntegrationCode": "SO-1001", "OrdLn_Qty": 4 }
      ]
    }
  ]
}
```

- **Lookups** are written as the integration code of the row they point at, never an id, so the file works in an application whose ids differ.
- **Left out:** the primary key, the audit columns (created and updated, at and by), and empty values. The receiving application assigns its own keys and sets its own audit columns.
- **Rows with no integration code** are left out, since they could not be matched on import.
- **Child views you can't read** are left out, as are child views that don't meet the [requirements below](#what-a-view-needs-for-json) — and their own children with them.

### Importing

1. Click **File Import**
2. Click **Select** and pick the file. A `.json` file is read as a data file; anything else as Excel
3. Review the rows. Each one shows the view, the record, and whether it will be an **Insert** or an **Update**
4. Click **Import**

The file is checked in full before anything is saved, and the grid's own save logic runs — the solution's validation and its save handlers — exactly as if the rows had been entered in the grid.

- **Matching.** Rows are matched on the view's integration code, ignoring case: an existing code is updated, a new one inserted.
- **Only what the grid shows.** A row that exists but the grid's filters hide is **refused**, not updated — widen the filters to include it, or leave it out of the file.
- **Children.** A child row must name its parent, and the parent must be a row the grid shows or one in the same file.
- **Columns left out** keep their current value on an update, and get the column's default on an insert.
- **Lookups** can point at a row created earlier in the same file.
- **Only this grid's views.** The file can contain data for the grid's view and its child views, each in one entry, and nothing else — no structure, and no other views.
- **No deletes.** A row missing from the file is left alone.

!!! note "All or nothing"
    Every problem in the file is reported together, before anything is saved: an unknown column, a lookup code that matches nothing, a hidden row, a child without its parent. A failure in validation or a before-save handler saves nothing either, and the save itself is one transaction.

    Only an after-save handler runs once the rows are committed. If it reports a problem, the message says the rows were saved.

### File Import compared with Load

Both take the same file format, but they do different jobs.

| | File Import on a grid | [Load](../../extending/import/index.md#loading-a-file) |
|---|---|---|
| For | Users, on their own data | Administrators, setting up a solution |
| Views | The grid's view and its children | Any view |
| Rows | Only those the grid's filters show | Every row in the view |
| Save handlers and validation | Run, as for a grid save | Not run |
| Structure in the file | Refused | Loaded first |
| An entry's `Mode` | Ignored — every row is inserted or updated | Honored — see [Update only, or insert only](../../extending/import/data.md#update-only-or-insert-only) |

### What a view needs for JSON

- **An Integration column.** Every view involved needs a column flagged **Integration**, since that is what rows are matched on. A grid whose view has none can't export to JSON — the message says to use Excel — and a child view without one is left out.
- **A lookup to its parent.** Each child view's link to its parent must be a lookup column, which is how a child row names its parent.

## Custom Actions

Custom actions can also be added.

See [Custom Actions](../../appGroups/admin/relational/relationalViews/actions.md#custom-actions)

This allows developers to extend the standard action set with solution-specific functionality.

## Common Usage Patterns

Different actions are typically used for different purposes.

### Record Maintenance

These actions are commonly used to create, update, or remove records:

- Add
- Add+
- Edit
- Edit+
- Copy
- Delete
- Save

### High-Volume Data Maintenance

These actions are commonly used when working with many records at once:

- Spreadsheet
- Bulk Update
- File Import
- File Export

### Navigation and Review

These actions are commonly used when reviewing or moving through related data:

- Navigate
- Refresh

## Notes

- Not every view will use every action.
- Actions should be configured based on the intended use case of the view.
- Custom actions can be added when standard actions do not fully meet the use case.