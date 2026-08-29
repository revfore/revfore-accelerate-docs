# Cubes

[← Back to Admin](../../index.md)

Cubes are the **OneStream cubes available to the Framework**.

This page lists the cubes brought across from OneStream. They are what you select wherever relational data is mapped to cube data — workflow areas, unit member sets, item types, and item category member sets.

!!! note "Read-only"
    Cubes come from OneStream and are refreshed by **Sync**. They cannot be added or edited here. To change a cube, change it in OneStream and sync again.

## Overview

Use the Cubes page to:

- see which OneStream cubes are available for selection
- confirm a cube exists before mapping data to it
- refresh the list after a cube is added or changed in OneStream

## Cube Record Fields

The following fields are shown for a Cube record. All are populated from OneStream.

| Field | Data Type| Purpose | Notes |
|---|---|---|---|
| Cube Name | nvarchar | Name of the cube in OneStream. |
| Cube Description | nvarchar | Description of the cube. |
| Effective Start Date | date | Date the cube becomes available for selection. |
| Effective End Date | date | Date the cube stops being available for selection. |
| Is Enabled | bit | Indicates whether the cube is enabled for use. |
| Integration Code | nvarchar | Unique value for the cube record. | Used for importing data
| Created Date | datetime | Date and time the record was created. |
| Modified Date | datetime | Date and time the record was last modified. |
| Created By | int | User who created the cube record. |
| Modified By | int | User who last modified the cube record. |
| Cube Id | int | Unique identifier for the cube record. |

## Where Cubes are Used

A cube is selected when configuring:

- [Workflow Areas](../../workflow/areas/index.md) – the cube an area's data belongs to
- [Workflow Unit Member Sets](../../workflow/units/memberSets.md) – the cube a unit's data is written to
- [Workflow Item Types](../../workflow/supporting/itemTypes.md) – the cube an item type's data belongs to
- [Item Category Member Sets](../../workflow/supporting/itemCategoryMemberSets.md) – the cube a category's data is written to

## Sync cubes from OneStream

1. Go to **Admin | Cube | Cubes**
2. Click **Sync**

Syncing brings the current set of cubes across from OneStream. Run it after a cube is added, renamed, or removed there.

## Notes

- A cube that is missing here almost always means it has been added in OneStream but not yet synced.
- Because the list mirrors OneStream, removing a cube there and re-syncing affects any configuration already pointing at it.
