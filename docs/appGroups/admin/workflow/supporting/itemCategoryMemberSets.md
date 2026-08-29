# Item Categories : Member Sets

[← Back to Workflow Supporting Overview](index.md)

The **Workflow Item Categories : Member Sets** page is used to assign cube and dimension members to a workflow item category.

This is the category-level equivalent of a [workflow unit's member set](../units/memberSets.md): it says where a category's data belongs in the cube.

## Overview

Use the Workflow Item Categories : Member Sets page to:

- assign an item category to a cube for a given member set type
- set the base dimension members the category's data is written at
- set the scope dimension members that bound the category's slice of the cube
- limit an assignment to a period of time using effective dates

A member set is defined per **item category**, **member set type** and **cube**.

## Workflow Item Category Member Set Record Fields

The following fields are used for a Workflow Item Category Member Set record.

| Field | Data Type| Purpose | Notes |
|---|---|---|---|
| Workflow Item Category | int | The item category this member set belongs to. | Set automatically when adding from within a category.
| Workflow Member Set Type | int | The kind of member set assignment this record represents. | Required. See [Member Set Types](memberSetTypes.md).
| Cube | int | The cube the category's data belongs to. | Required.
| Base Dimension Members | int | The members the category's data is written at, one per dimension. | Entity, Scenario, Account, Flow, IC and UD1 through UD8. Set only the dimensions the solution actually uses.
| Scope Dimension Members | int | The outer bounds of the category's slice of the cube, one per dimension. | Same dimensions as the base members. Not a rollup target - see the equivalent section under [Workflow Unit Member Sets](../units/memberSets.md#base-and-scope-members).
| Effective Start Date | date | Date the member set becomes active. | Defaults to 1900-01-01.
| Effective End Date | date | Date the member set stops being active. | Defaults to 2999-12-31.
| Is Enabled | bit | Indicates whether the member set is enabled for use. |
| Created Date | datetime | Date and time the record was created. |
| Modified Date | datetime | Date and time the record was last modified. |
| Created By | int | User who created the member set record. |
| Modified By | int | User who last modified the member set record. |
| Workflow Item Category Member Set Id | int | Unique identifier for the member set record. | If you leave blank, the system will auto assign

## Create a new Member Set

1. Go to **Admin | Workflow | Supporting**
2. Open the **Item Categories** page and select the category
3. Open the **Member Sets** child grid
4. Click on '**Add+**' or '**Add & Edit in Grid**'
5. Enter the Member Set Type and Cube, then the dimension members required
6. Click **Save**

**Add & Edit in Grid** allows adding and modifying rows directly in the grid

!!!Note Important Notes
    The Workflow Item Category is set from the category the member set is added under

    See [General Actions](../../../../concepts/metadataDrivenUI/actions.md#general-actions) for more information about adding records

## Notes

- Only populate the dimensions the solution uses; leave the rest blank.
- Category member sets and [unit member sets](../units/memberSets.md) serve different scopes and can both apply.
- Use effective dates to change a mapping over time rather than editing an existing record in place.
