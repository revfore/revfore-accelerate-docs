# Dynamic Grid Content Sub Item

[← Back to Content Sub Items](index.md)

A **Dynamic Grid** Content Sub Item is used to render a Relational View in a standard grid experience within a Content Item layout.

Dynamic Grid Content Sub Items allow you to tailor the presentation and behavior of a Relational View for a specific page. This means the same Relational View can be reused across multiple Content Sub Items and configured differently depending on the user experience you want to create.

## Overview

Use a Dynamic Grid Content Sub Item to:

- render a Relational View in a standard grid
- tailor the view for a specific page
- control layout, filters, and display settings
- configure column behavior and ordering
- enable or disable actions
- define interactions with other Content Sub Items on the same page

## Key Concept

A Dynamic Grid Content Sub Item does not replace the underlying Relational View. Instead, it provides a page-specific layer of configuration on top of that view.

This allows you to:

- reuse the same Relational View in multiple places
- apply different filters on different pages
- show different columns on different pages
- enable different actions on different pages
- create different user experiences from the same underlying relational structure

## General Settings

The following settings are available for a Dynamic Grid Content Sub Item.

| Setting | Purpose | Notes |
|---|---|---|
| Top Dashboard Name | Defines the outer dashboard for the Content Sub Item. | This will auto populate from the content type but can be overridden with a custom dashboard. |
| Body Dashboard Name | Defines the inner dashboard that holds only the dynamic grid. | This will auto populate from the content type but can be overridden with a custom dashboard. |
| Relational View | Selects the Relational View used by the Content Sub Item. | This is chosen from a dropdown of available Relational Views. |
| Edit Relational View | Opens the Relational View Edit+ page. | Allows you to edit the parent and child views of the selected Relational View. |
| Edit Relational Model | Opens the Relational Model Edit+ page. | Allows you to edit the parent and child views of the selected Relational Model. |
| Display Name | Defines the display name shown for the Content Sub Item that will show in the Title Bar | Defaults from the Relational View but can be overridden. |
| Filter | Applies a predefined filter from the Relational View. | This applies a base filter to the view records. |
| Dashboard Height | Defines the height of the Content Sub Item dashboard. | Examples include `*`, `Auto`, or `250`. Applies to multi-sub-item vertical layouts and may not take effect until dashboards are switched. |
| Dashboard Width | Defines the width of the Content Sub Item dashboard. | Examples include `*`, `Auto`, or `250`. Applies to multi-sub-item horizontal layouts and may not take effect until dashboards are switched. |
| Summary Font Size | Defines the font size for summary items at the bottom of the grid. | Used when summary values are displayed. |
| Auto Populate | Controls whether the grid automatically loads records when opened. | If not checked, users must click Refresh or apply a filter. |
| Show Title Bar | Controls whether the title bar is shown. | |
| Show Filter Bar | Controls whether the filter bar is shown. | The filter bar includes filters and the Inline Entry checkbox. |
| Show Inline Entry | Controls whether the **Enable Inline Adding & Editing** option is available. | Used to allow inline add/edit behavior in the grid. |

## Column Tab

The **Column** tab is used to control which columns appear in the grid and how they behave.

### Included and Excluded Columns

The Column tab includes:

- an **Included Columns** list
- an **Excluded Columns** list

By default, the Included Columns list contains all columns from the selected Relational View.

You can move columns between these lists using the add and remove buttons.

You can also change the order of the included columns using the up and down buttons.

### Column-Level Configuration

For included columns, you can configure:

- display name overrides
- visibility
- allow updates
- summary type (i.e. Sum, Min, Max, Count, Avg)
- summary name
- order by columns and direction
- lookup primary key filter (i.e. {"Include":"1,2"} or {"Exclude":"1,2"})
- filter flag
- column width
- default value
- data format (i.e. N1, P1, MM/DD/YYYY)

### Summary Types

Supported summary types include:

- Sum
- Min
- Max
- Count
- Avg

These can be used to show summary values at the bottom of the grid.

## Actions Tab

The **Actions** tab is used to control which actions are available for the Dynamic Grid Content Sub Item.

This is presented as a multi-select checkbox list of all available actions.

Examples of actions may include:

- Add
- Edit
- Copy
- Delete
- Spreadsheet
- Bulk Update
- File Export
- File Import
- Navigate
- Refresh

See [Actions](../actions.md) for more information about standard and custom actions.

## Dependents Tab

The **Dependents** tab is used to define how this Content Sub Item interacts with other Content Sub Items on the same page.

This tab includes a list of other Content Sub Items and allows you to configure whether they should refresh when this Content Sub Item changes.

### Dependent Options

For each dependent Content Sub Item, you can configure:

- **Refresh on Data Change**
- **Refresh on Selection Change**

### Relationship Filter

If **Refresh on Selection Change** is enabled, you must populate the **Relationship Filter**.

This allows the system to understand how to filter the dependent Content Sub Item based on the selected record or records in the current grid.

## Typical Use Cases

Examples of Dynamic Grid Content Sub Item use cases include:

- using the same Relational View on multiple pages with different filters
- creating a maintenance-focused grid on one page and a read-only reporting grid on another
- showing different subsets of columns depending on the audience
- enabling different actions depending on the use case
- using one grid to filter and refresh another on the same page

## Design Guidance

When configuring a Dynamic Grid Content Sub Item:

- start with the underlying Relational View
- only expose the columns needed for the page
- use page-level filters to reuse views without duplicating them
- enable only the actions needed for the user experience
- use dependent settings carefully to create intuitive page interactions
- keep the configuration focused on the intended use case

## Notes

- A Dynamic Grid Content Sub Item is a page-specific configuration layer on top of a Relational View.
- The same Relational View can be reused across multiple Content Sub Items and configured differently.
- Column, action, and dependent settings allow each grid to be tailored for a specific page.
- Dynamic grids are one of the most flexible and commonly used Content Sub Item types in Revfore Accelerate.