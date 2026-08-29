# Cubes

[← Back to Admin](../../index.md)

Cubes are the **OneStream cubes the Framework reads from and writes to**.

Registering a cube makes it available for selection wherever relational data is mapped to cube data — workflow areas, unit member sets, item types, and item category member sets.

## Overview

Use Cubes to:

- register the OneStream cubes the solution uses
- review the cubes already available for selection
- limit a cube to a period of time using effective dates
- enable or disable a cube without removing it

A cube record here is a reference to a cube that exists in OneStream. It does not create the cube.

## Cube Record Fields

The following fields are used for a Cube record.

| Field | Data Type| Purpose | Notes |
|---|---|---|---|
| Cube Name | nvarchar | Name of the cube. | Should match the cube name in OneStream.
| Cube Description | nvarchar | Description of the cube and its purpose. |
| Effective Start Date | date | Date the cube becomes available for selection. | Defaults to 1900-01-01.
| Effective End Date | date | Date the cube stops being available for selection. | Defaults to 2999-12-31.
| Is Enabled | bit | Indicates whether the cube is enabled for use. |
| Integration Code | nvarchar | Unique value for the cube record. | This is readonly and provides a unique value for the record that is used for importing data
| Created Date | datetime | Date and time the record was created. |
| Modified Date | datetime | Date and time the record was last modified. |
| Created By | int | User who created the cube record. |
| Modified By | int | User who last modified the cube record. |
| Cube Id | int | Unique identifier for the cube record. | If you leave blank, the system will auto assign

## Where Cubes are Used

A cube is selected when configuring:

- [Workflow Areas](../../workflow/areas/index.md) – the cube an area's data belongs to
- [Workflow Unit Member Sets](../../workflow/units/memberSets.md) – the cube a unit's data is written to
- [Workflow Item Types](../../workflow/supporting/itemTypes.md) – the cube an item type's data belongs to
- [Item Category Member Sets](../../workflow/supporting/itemCategoryMemberSets.md) – the cube a category's data is written to

## Create a new Cube

1. Go to **Admin | Cube | Cubes**
2. Click on '**Add+**' or '**Add & Edit in Grid**'
3. Click on the '**+**' button on the top left of the grid
4. Enter the cube name and any remaining fields, then click **Save**

**Add & Edit in Grid** allows adding and modifying rows directly in the grid

!!!Note Important Notes
    The Integration Code and Cube Id will be auto-assigned

    See [General Actions](../../../../concepts/metadataDrivenUI/actions.md#general-actions) for more information about adding records

## Notes

- Register only the cubes the solution actually posts to or reads from.
- Disabling a cube removes it from selection lists but leaves existing configuration intact.
- Cube names should match OneStream exactly, since the name is how the cube is identified when data is synced.
