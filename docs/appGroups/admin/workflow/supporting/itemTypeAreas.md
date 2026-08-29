# Item Types : Workflow Areas

[← Back to Workflow Supporting Overview](index.md)

The **Workflow Item Types : Workflow Areas** page is used to map a workflow item type to the areas it appears in.

An item type is only available within a cycle once it has been mapped to at least one [workflow area](../areas/index.md).

## Overview

Use the Workflow Item Types : Workflow Areas page to:

- make an item type available in a workflow area
- mark which area is the primary one for the item type
- limit a mapping to a period of time using effective dates

An item type can be mapped to more than one area, and an area can host more than one item type.

## Workflow Item Type Area Record Fields

The following fields are used for a Workflow Item Type Area record.

| Field | Data Type| Purpose | Notes |
|---|---|---|---|
| Workflow Item Type | int | The item type being mapped. | Set automatically when adding from within an item type.
| Workflow Area | int | The area the item type is being made available in. | Required. See [Workflow Areas](../areas/index.md).
| Is Primary | bit | Marks this as the item type's primary area. | Identifies the default where an item type is mapped to more than one area.
| Effective Start Date | date | Date the mapping becomes active. | Defaults to 1900-01-01.
| Effective End Date | date | Date the mapping stops being active. | Defaults to 2999-12-31.
| Is Enabled | bit | Indicates whether the mapping is enabled for use. |
| Created Date | datetime | Date and time the record was created. |
| Modified Date | datetime | Date and time the record was last modified. |
| Created By | int | User who created the mapping record. |
| Modified By | int | User who last modified the mapping record. |
| Workflow Item Type Area Id | int | Unique identifier for the mapping record. | If you leave blank, the system will auto assign

## Create a new Mapping

1. Go to **Admin | Workflow | Supporting**
2. Open the **Item Types** page and select the item type
3. Open the **Workflow Areas** child grid
4. Click on '**Add+**' or '**Add & Edit in Grid**'
5. Select the Workflow Area and click **Save**

**Add & Edit in Grid** allows adding and modifying rows directly in the grid

!!!Note Important Notes
    The Workflow Item Type is set from the item type the mapping is added under

    See [General Actions](../../../../concepts/metadataDrivenUI/actions.md#general-actions) for more information about adding records

## Notes

- Mark exactly one area as primary per item type, so the default is unambiguous.
- Areas belong to an instance type, so mapping an item type to an area also determines which kind of cycle it appears in.
- Use effective dates to withdraw a mapping rather than deleting it.
