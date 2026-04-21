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

The following actions may be available on a view.

| Action | Description | Notes |
|---|---|---|
| Inline Entry | A checkbox-based action that switches the current grid into add/edit mode, allowing the user to click a **+** button to add new records and edit selected existing records directly in the grid. | Records that need to be edited must be selected before the checkbox is checked. |
| Add | Opens a page with an empty grid that you can add records to. | Typically used to create new records. |
| Add+ | Opens a Parent/Children add page. | Useful when users need to enter related records together in a single experience. |
| Edit | Opens a page with an editable grid populated with the selected records. | Used to update existing records. |
| Edit+ | Opens a Parent/Children editable grids page with the selected records. | Useful when related records need to be maintained together. |
| Copy | Makes a copy of all selected records. | Helpful for creating similar records without re-entering all values. |
| Spreadsheet | Puts all the data in the view into a spreadsheet component that allows users to add and edit rows in an Excel-like fashion. | Useful for high-volume review and maintenance. |
| Bulk Update | Takes the selected records and allows users to update one or more fields across all selected records. | Useful for applying the same change across many records. |
| Delete | Deletes the selected records. | Use with care, especially for production data. |
| Save | Saves pending changes. | Typically used after add, edit and inline entry. |
| File Export | Allows a user to export all the view records to a OneStream spreadsheet or to Excel. | Useful for review, offline analysis, or sharing data. |
| File Import | Allows a user to select an Excel file and import the data into OneStream. | Useful for loading or updating larger sets of records. |
| Navigate | OPens a page showing a tree view of related views that can be navigated to. Users can also navigate within the current view by selecting columns to filter by. | Useful for moving across related data structures and drilling into related records; in addition, to filtering within a views based on the value of a column of all selected records |
| Refresh | Refreshes the current view. | Useful for reloading the latest data after updates or navigation. |

## Custom Actions

Custom actions can also be added.

See [Custom Actions](../../appGroups/admin/relationalViews/actions.md#custom-actions)


This allows developers to extend the standard action set with solution-specific functionality.

## Common Usage Patterns

Different actions are typically used for different purposes.

### Record Maintenance

These actions are commonly used to create, update, or remove records:

- Add
- Add+
- Edit
- Edit+
- Inline Entry
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
- Data entry and maintenance views may use more interactive actions such as Add, Edit, Inline Entry, Spreadsheet, and Bulk Update.
- Read-only reporting or analysis views may rely more on actions such as Navigate, Refresh, and File Export.
- Custom actions can be added when standard actions do not fully meet the use case.