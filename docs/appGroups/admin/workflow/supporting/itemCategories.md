# Item Categories

[← Back to Workflow Supporting Overview](index.md)

The **Workflow Item Categories** page is used to define the categories a workflow item can belong to.

A category groups items for reporting, routing, or configuration — for example separating IT capital from facilities capital.

## Overview

Use the Workflow Item Categories page to:

- define the categories of work the solution uses
- review existing categories
- enable or disable a category
- manage each category's [Member Sets](itemCategoryMemberSets.md)

## Workflow Item Category Record Fields

The following fields are used for a Workflow Item Category record.

| Field | Data Type| Purpose | Notes |
|---|---|---|---|
| Workflow Item Category Name | nvarchar | Internal name of the item category. | Must be unique. No spaces or special characters are allowed.
| Workflow Item Category Display Name | nvarchar | User-friendly display name shown in the application. | Must be unique.
| Workflow Item Category Description | nvarchar | Description of the item category and its purpose. |
| Is Enabled | bit | Indicates whether the item category is enabled for use. |
| Ext Ref Unique Code | nvarchar | Unique value for the item category record. | This is readonly and provides a unique value for the record that is used for importing data
| Created Date | datetime | Date and time the record was created. |
| Modified Date | datetime | Date and time the record was last modified. |
| Created By | int | User who created the item category record. |
| Modified By | int | User who last modified the item category record. |
| Workflow Item Category Id | int | Unique identifier for the item category record. | If you leave blank, the system will auto assign

## Child Objects

- [Member Sets](itemCategoryMemberSets.md) – the cube and dimension members assigned to the category

## Typical Use Cases

- IT capital versus facilities capital
- Direct versus indirect expense
- Grouping items for approval routing
- Separating categories that post to different dimension members

## Create a new Workflow Item Category

1. Go to **Admin | Workflow | Supporting**
2. Open the **Item Categories** page
3. Click on '**Add+**' or '**Enable Inline Adding & Editing**'
4. Enter required fields and click **Save**
5. Create the category's [Member Sets](itemCategoryMemberSets.md)
6. Map the category to the [Item Types](itemTypeItemCategories.md) that use it

**Enable Inline Adding & Editing** allows adding and modifying rows directly in the grid

!!!Note Important Notes
    The Ext Ref Unique Code and Workflow Item Category Id will be auto-assigned

    See [General Actions](../../../../concepts/metadataDrivenUI/actions.md#general-actions) for more information about adding records

## Notes

- A category is only reachable from an item type once the two are mapped — see [Item Types : Item Categories](itemTypeItemCategories.md).
- Categories are reference data; avoid creating one per item.
