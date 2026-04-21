# Filters

[← Back to Relational Views Overview](index.md)

The **Filters** page is used to define how data is filtered within a Relational View.

Filters help users focus on the most relevant subset of data.

## Overview

Use the Filters page to:

- limit what data appears in a view
- support user-friendly data exploration
- make large datasets easier to work with
- tailor the view to a specific business context

## Relational View Filter Fields

The following fields are used for a Relational View Filter record.

| Field | Data Type | Purpose | Notes |
|---|---|---|---|
| Relational Model | int | Identifies the relational model associated with the filter. | This is readonly and can be ignored when creating new records |
| Relational View | int | Identifies the relational view that the filter belongs to. | This links the filter record back to its parent relational view. |
| Filter Name | nvarchar | Internal name of the filter. | No spaces or special characters are allowed. |
| Filter Description | nvarchar | Description of the filter and its purpose. | Use this to document how the filter should be used. |
| Filter Display Name | nvarchar | User-friendly display name shown in the application. | This is typically the label users will see. |
| Filter Expression | nvarchar | Defines the expression used by the filter. | Use the **Edit** button to open the Edit screen and then use the **Expression Editor** button to open the expression editor page |
| Is Enabled | bit | Indicates whether the filter is enabled for use. | Disabled filters are not intended for active use. |
| Ext Ref Unique Code | nvarchar | Unique value for the relational view filter record. | This is readonly and is typically auto set to provide a stable unique value for importing data. |
| Created Date | datetime | Date and time the record was created. | This is system maintained. |
| Modified Date | datetime | Date and time the record was last modified. | This is system maintained. |
| Created By | int | User who created the relational view filter record. | This is system maintained. |
| Modified By | int | User who last modified the relational view filter record. | This is system maintained. |
| Relational Filter Id | int | Unique identifier for the relational view filter record. | If left blank, the system will typically auto assign it. |

## Typical Use Cases

Examples include filtering by:

- status
- department
- location
- product category
- date or period

## Design Guidance

When defining filters:

- focus on the fields users are most likely to filter by
- keep filter options intuitive
- avoid unnecessary complexity
- support common reporting and workflow needs

## Create a new Relational View Filters

1. Go to **Admin | Relational Views**
2. Select the desired Relational Table and click on '**Edit+**'
3. In the Child Views list, Select '**Relational Views : Filters**'
4. In the '**Relational Views : Filters**' section, Click on '**Add**' or '**Inline Entry**' 
5. Click on the '**+**' button on the top left of the grid
6. Enter required fields and click **Save**
7. Click on **Edit** to open the Edit page
8. Click on **Expression Editor** to open the Expression Editor
9. Add the expression and Click on **OK**

## Notes

- Filters improve usability by narrowing the data shown.
- A well-filtered view is easier to understand and act on.
- Filters are especially helpful when views contain large volumes of data.