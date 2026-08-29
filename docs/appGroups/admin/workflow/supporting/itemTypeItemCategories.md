# Item Types : Item Categories

[← Back to Workflow Supporting Overview](index.md)

The **Workflow Item Types : Item Categories** page is used to map a workflow item type to the categories its items can belong to.

The mapping controls which [categories](itemCategories.md) a user can choose from when creating an item of that type.

## Overview

Use the Workflow Item Types : Item Categories page to:

- make a category available to an item type
- mark which category is the primary one for the item type
- limit a mapping to a period of time using effective dates

An item type can be mapped to more than one category, and a category can be used by more than one item type.

## Workflow Item Type Item Category Record Fields

The following fields are used for a Workflow Item Type Item Category record.

| Field | Data Type| Purpose | Notes |
|---|---|---|---|
| Workflow Item Type | int | The item type being mapped. | Set automatically when adding from within an item type.
| Workflow Item Category | int | The category being made available to the item type. | Required. See [Item Categories](itemCategories.md).
| Is Primary | bit | Marks this as the item type's primary category. | Identifies the default where an item type is mapped to more than one category.
| Effective Start Date | date | Date the mapping becomes active. | Defaults to 1900-01-01.
| Effective End Date | date | Date the mapping stops being active. | Defaults to 2999-12-31.
| Is Enabled | bit | Indicates whether the mapping is enabled for use. |
| Created Date | datetime | Date and time the record was created. |
| Modified Date | datetime | Date and time the record was last modified. |
| Created By | int | User who created the mapping record. |
| Modified By | int | User who last modified the mapping record. |
| Workflow Item Type Item Category Id | int | Unique identifier for the mapping record. | If you leave blank, the system will auto assign

## Create a new Mapping

1. Go to **Admin | Workflow | Supporting**
2. Open the **Item Types** page and select the item type
3. Open the **Item Categories** child grid
4. Click on '**Add+**' or '**Enable Inline Adding & Editing**'
5. Select the Workflow Item Category and click **Save**

**Enable Inline Adding & Editing** allows adding and modifying rows directly in the grid

!!!Note Important Notes
    The Workflow Item Type is set from the item type the mapping is added under

    See [General Actions](../../../../concepts/metadataDrivenUI/actions.md#general-actions) for more information about adding records

## Notes

- Mark exactly one category as primary per item type, so the default is unambiguous.
- A category's own [member sets](itemCategoryMemberSets.md) apply to items in that category, whichever item type they belong to.
- Use effective dates to withdraw a mapping rather than deleting it.
