# Item Types

[← Back to Workflow Supporting Overview](index.md)

The **Workflow Item Types** page is used to define the types of workflow item the solution supports.

An item type describes a kind of thing a user creates and works on within a cycle — a capital request, a headcount change, a rate adjustment.

## Overview

Use the Workflow Item Types page to:

- define the types of item the solution captures
- set the numbering prefix applied to items of each type
- set the cube and data source members the type's data belongs to
- map each type to its [Areas](itemTypeAreas.md) and [Item Categories](itemTypeItemCategories.md)

## Workflow Item Type Record Fields

The following fields are used for a Workflow Item Type record.

| Field | Data Type| Purpose | Notes |
|---|---|---|---|
| Workflow Item Type Name | nvarchar | Internal name of the item type. | Must be unique. No spaces or special characters are allowed.
| Workflow Item Type Display Name | nvarchar | User-friendly display name shown in the application. | Must be unique.
| Workflow Item Type Description | nvarchar | Description of the item type and its purpose. |
| Number Prefix | nvarchar | Prefix applied to numbers generated for items of this type. | For example a prefix of CR produces CR10000, CR10001.
| Cube | int | The cube the item type's data belongs to. | Leave blank where the type does not post to a cube.
| Data Source Dimension | int | The dimension the data source member is selected from. |
| Data Source Member | int | The data source dimension member used for this item type's data. |
| Effective Start Date | date | Date the item type becomes available. | Defaults to 1900-01-01.
| Effective End Date | date | Date the item type stops being available. | Defaults to 2999-12-31.
| Is Enabled | bit | Indicates whether the item type is enabled for use. |
| Ext Ref Unique Code | nvarchar | Unique value for the item type record. | This is readonly and provides a unique value for the record that is used for importing data
| Created Date | datetime | Date and time the record was created. |
| Modified Date | datetime | Date and time the record was last modified. |
| Created By | int | User who created the item type record. |
| Modified By | int | User who last modified the item type record. |
| Workflow Item Type Id | int | Unique identifier for the item type record. | If you leave blank, the system will auto assign

## Child Objects

- [Workflow Areas](itemTypeAreas.md) – the areas this item type appears in
- [Item Categories](itemTypeItemCategories.md) – the categories items of this type can belong to

## Typical Use Cases

- Capital request
- Headcount change
- Rate adjustment
- Project or initiative

## Create a new Workflow Item Type

1. Go to **Admin | Workflow | Supporting**
2. Open the **Item Types** page
3. Click on '**Add+**' or '**Enable Inline Adding & Editing**'
4. Enter required fields and click **Save**
5. Map the type to its [Workflow Areas](itemTypeAreas.md)
6. Map the type to its [Item Categories](itemTypeItemCategories.md)

**Enable Inline Adding & Editing** allows adding and modifying rows directly in the grid

!!!Note Important Notes
    The Ext Ref Unique Code and Workflow Item Type Id will be auto-assigned

    See [General Actions](../../../../concepts/metadataDrivenUI/actions.md#general-actions) for more information about adding records

## Notes

- An item type is not usable in a cycle until it is mapped to at least one workflow area.
- Number Prefix is applied when items are numbered automatically; keep it short and distinctive.
- Use effective dates to retire an item type rather than deleting it, so existing items stay valid.
