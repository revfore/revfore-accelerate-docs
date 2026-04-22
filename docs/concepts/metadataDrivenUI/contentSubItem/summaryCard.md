# Summary Cards Content Sub Item

[← Back to Content Sub Items](index.md)

A **Summary Cards** Content Sub Item is used to render a Relational View in a summary cards experience within a Content Item layout.

Summary Cards Content Sub Items allow you to tailor the presentation and behavior of a Relational View for a specific page. This means the same Relational View can be reused across multiple Content Sub Items and configured differently depending on the user experience you want to create.

## Overview

Use a Summary Cards Content Sub Item to:

- render a Relational View in a summary cards
- tailor the summary cards for a specific page
- control the number of cards, content of the cards and font size

## Key Concept

A Summary Cards Content Sub Item does not replace the underlying Relational View. Instead, it provides a page-specific layer of configuration on top of that view.

This allows you to:

- reuse the same Relational View in multiple places
- apply different filters on different pages
- show different columns on different pages
- enable different actions on different pages
- create different user experiences from the same underlying relational structure

## General Settings

The following settings are available for a Summary Cards Content Sub Item.

| Setting | Purpose | Notes |
|---|---|---|
| Top Dashboard Name | Defines the outer dashboard for the Content Sub Item. | This will auto populate from the content type but can be overridden with a custom dashboard. |
| Body Dashboard Name | Defines the inner dashboard that holds only the Summary Cards. | This will auto populate from the content type but can be overridden with a custom dashboard. |
| Relational View | Selects the Relational View used by the Content Sub Item. | This is chosen from a dropdown of available Relational Views. |
| Edit Relational View | Opens the Relational View Edit+ page. | Allows you to edit the parent and child views of the selected Relational View. |
| Edit Relational Model | Opens the Relational Model Edit+ page. | Allows you to edit the parent and child views of the selected Relational Model. |
| Display Name | Defines the display name shown for the Content Sub Item that will show in the Title Bar | Defaults from the Relational View but can be overridden. |
| Filter | Applies a predefined filter from the Relational View. | This applies a base filter to the view records. |
| Dashboard Height | Defines the height of the Content Sub Item dashboard. | Examples include `*`, `Auto`, or `250`. Applies to multi-sub-item vertical layouts and may not take effect until dashboards are switched. |
| Dashboard Width | Defines the width of the Content Sub Item dashboard. | Examples include `*`, `Auto`, or `250`. Applies to multi-sub-item horizontal layouts and may not take effect until dashboards are switched. |
| Summary Font Size | Defines the font size for summary items. |  |
| Auto Populate | Controls whether the summary cards automatically loads records when opened. | Not applicable for Summary Cards at this time. |
| Show Title Bar | Controls whether the title bar is shown. | |
| Show Filter Bar | Controls whether the filter bar is shown. | Not applicable to Summary Cards |
| Show Inline Entry | Controls whether the Inline Entry option is available. | Not applicable for Summary Cards |

## Column Tab

The **Column** tab is used to control which columns appear in the summary cards and how they behave.

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
- summary type (Sum, Min, Max, Count, Avg)
- summary name (This will override the display name)
- data format (i.e. N1, P1, MM/DD/YYYY)

## Actions Tab

The **Actions** tab does not apply to Summary Cards

## Dependents Tab

The **Dependents** tab is used to define how this Content Sub Item interacts with other Content Sub Items on the same page.

Summary Cards do not affect other Content Sub Items but other Content Sub Items, such as Dynamic Grids and List, can affect Summary Cards Content Sub Items.

## Typical Use Cases

Examples of Summary Cards Content Sub Item use cases include:

- using the same Relational View on multiple pages with different filters
- creating Summary Cards above a Dynamic Grid or Pivot Grid
- showing different subsets of columns depending on the audience

## Design Guidance

When configuring a Summary Cards Content Sub Item:

- start with the underlying Relational View
- only expose the columns needed for the summary cards
- use page-level filters to reuse views without duplicating them
- keep the configuration focused on the intended use case

## Notes

- A Summary Cards Content Sub Item is a page-specific configuration layer on top of a Relational View.
- The same Relational View can be reused across multiple Content Sub Items and configured differently.
- Column, filter, and dependent settings allow each summary cards to be tailored for a specific page.