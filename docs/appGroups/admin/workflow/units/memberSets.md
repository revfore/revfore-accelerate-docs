# Member Sets

[← Back to Workflow Units Overview](index.md)

The **Workflow Units : Member Sets** page is used to define and manage the cube and dimension member assignments for a workflow unit.

A member set is what connects a workflow unit to the cube. It says which cube the unit's data belongs to, which dimension members it is written at, and which members it is summarised to.

## Overview

Use the Workflow Units : Member Sets page to:

- assign a workflow unit to a cube for a given workflow instance type
- set the base dimension members the unit's data is written at
- set the scope dimension members that bound the unit's slice of the cube
- set the data source members used for each data source scope
- limit an assignment to a period of time using effective dates

A member set is defined per **workflow unit** and **workflow instance type**, so the same unit can be mapped differently depending on the kind of workflow being performed.

## Workflow Unit Member Set Record Fields

The following fields are used for a Workflow Unit Member Set record.

| Field | Data Type| Purpose | Notes |
|---|---|---|---|
| Workflow Unit | int | The workflow unit this member set belongs to. | Set automatically when adding from within a unit.
| Workflow Instance Type | int | The workflow instance type this member set applies to. | Allows the same unit to map differently for different kinds of workflow.
| Cube | int | The cube the unit's data belongs to. | Required. Without a cube there is no destination for the unit's data.
| Base Dimension Members | int | The members the unit's data is written at, one per dimension. | Entity, Scenario, Account, Flow, IC and UD1 through UD8. Set only the dimensions the solution actually uses.
| Scope Dimension Members | int | The outer bounds of the unit's slice of the cube, one per dimension. | Same dimensions as the base members. See below - these are not a rollup target, they define a boundary.
| Data Source Dimension | int | The dimension the data source scope members are selected from. | Identifies which dimension the members below belong to.
| Data Source Scope Members | int | The members used for each data source scope. | One each for All, Controlled, Controlled Itemized, Controlled Direct and Controlled Actual.
| Effective Start Date | date | Date the member set becomes active. | Defaults to 1900-01-01.
| Effective End Date | date | Date the member set stops being active. | Defaults to 2999-12-31. Use this to re-map a unit from a point in time without losing the previous mapping.
| Is Enabled | bit | Indicates whether the member set is enabled for use. |
| Created Date | datetime | Date and time the record was created. |
| Modified Date | datetime | Date and time the record was last modified. |
| Created By | int | User who created the member set record. |
| Modified By | int | User who last modified the member set record. |
| Workflow Unit Member Set Id | int | Unique identifier for the member set record. | If you leave blank, the system will auto assign

A workflow unit can have more than one member set, but only one per combination of **Workflow Unit**, **Workflow Instance Type**, **Cube** and **Effective Start Date**.

### Base and scope members

**Base members** are where the unit's detail data is written.

**Scope members** are not a rollup target - they define the **outer bounds of the slice of the cube the unit is assigned to**. That boundary is used three ways:

- to present the unit's data in summary form in cube views
- to define the outer bounds of the data intersections that are synchronised with the detail data
- to define the data that is cleared for the unit

Because the scope members bound what gets synchronised and cleared, setting them wider than the unit actually owns risks clearing another unit's data, and setting them narrower than the detail risks leaving data behind.

## Typical Use Cases

- Mapping a department to the entity and account members its budget is written at
- Bounding a cost centre's slice of the cube so its data can be summarised, synchronised and cleared correctly
- Mapping the same unit to different cubes for budget and forecast instance types
- Re-mapping a unit from a given date after a restructure, without altering prior cycles

## Create a new Member Set

1. Go to **Admin | Workflow | Units**
2. Select the workflow unit the member set belongs to
3. Open the **Member Sets** child grid
4. Click on '**Add+**' or '**Add & Edit in Grid**'
5. Enter the Workflow Instance Type and Cube, then the dimension members required
6. Click **Save**

**Add & Edit in Grid** allows adding and modifying rows directly in the grid

!!!Note Important Notes
    The Workflow Unit is set from the unit the member set is added under

    See [General Actions](../../../../concepts/metadataDrivenUI/actions.md#general-actions) for more information about adding records

## Notes

- Only populate the dimensions the solution uses; leave the rest blank rather than selecting a default member.
- Base and scope members serve different purposes - base is where the unit's detail data is written; scope bounds the region of the cube that data occupies.
- A unit with no member set for the instance type being run has no cube destination, which is a common cause of data not appearing in the cube.
- Use effective dates to change a mapping over time rather than editing an existing member set in place.
