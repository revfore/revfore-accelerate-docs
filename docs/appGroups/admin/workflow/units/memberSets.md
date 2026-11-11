# Member Sets

[← Back to Workflow Units Overview](index.md)

The **Workflow Units : Member Sets** page is used to define and manage the cube and dimension member assignments for a workflow unit.

A member set is what connects a workflow unit to the cube. It says which cube the unit's data belongs to, which dimension members it is written at, and which members it is summarized to.

A unit's member set holds **only its exceptions**. The members every unit of a cycle type shares — typically IC, Flow and the unused UDs — and the data source are set once on the [cycle type's member set](../cycles/cycleTypes.md#member-sets-every-units-defaults); a member left blank here takes the cycle type's. Most units need only their Entity.

## Overview

Use the Workflow Units : Member Sets page to:

- assign a workflow unit to a cube for a given workflow cycle type
- set the base dimension members the unit's data is written at
- set the scope dimension members that bound the unit's slice of the cube
- override any of the cycle type's shared members for this unit
- limit an assignment to a period of time using effective dates

A member set is defined per **workflow unit** and **workflow cycle type**, so the same unit can be mapped differently depending on the kind of workflow being performed.

## Workflow Unit Member Set Record Fields

The following fields are used for a Workflow Unit Member Set record.

| Field | Data Type| Purpose | Notes |
|---|---|---|---|
| Workflow Unit | int | The workflow unit this member set belongs to. | Set automatically when adding from within a unit.
| Workflow Cycle Type | int | The workflow cycle type this member set applies to. | Allows the same unit to map differently for different kinds of workflow.
| Cube | int | The cube the unit's data belongs to. | Required. Without a cube there is no destination for the unit's data.
| Base Dimension Members | int | The members the unit's data is written at, one per dimension. | Entity, Flow, IC and UD1 through UD8 (Base Account is not used here: account comes from the item category). Set only what differs from the cycle type's member set - a blank member takes the cycle type's. There is no scenario here: it comes from the cycle's [Workflow Scenario](../cycles/workflowScenarios.md), which can also override Flow and the UDs.
| Scope Dimension Members | int | The outer bounds of the unit's slice of the cube, one per dimension. | Entity, Account, Flow, IC and UD1 through UD8. Blank takes the cycle type's. See below - these are not a rollup target, they define a boundary.
| Effective Start Date | date | Date the member set becomes active. | Defaults to 1900-01-01.
| Effective End Date | date | Date the member set stops being active. | Defaults to 2999-12-31. Use this to re-map a unit from a point in time without losing the previous mapping.
| Is Enabled | bit | Indicates whether the member set is enabled for use. |
| Created Date | datetime | Date and time the record was created. |
| Modified Date | datetime | Date and time the record was last modified. |
| Created By | int | User who created the member set record. |
| Modified By | int | User who last modified the member set record. |
| Workflow Unit Member Set Id | int | Unique identifier for the member set record. | If you leave blank, the system will auto assign

A workflow unit can have more than one member set, but only one per combination of **Workflow Unit**, **Workflow Cycle Type**, **Cube** and **Effective Start Date**.

There are no data source fields here. The data source — the dimension and the Readable and Controlled members that bound what the unit can read and change — is set on the [cycle type's member set](../cycles/cycleTypes.md#data-source), and every unit of the type gets it.

When you save, a blank **Base Entity** is filled with the cube's first base entity. Other blank members are left blank, so they take the cycle type's.

### How the member set is built

The member set a unit actually uses for a cycle is built in steps, each over the last:

1. the **unit's member set** — its own exceptions, usually just the Entity
2. the **cycle type's member set** for the same cube fills every member the unit leaves blank; a cube the unit has no member set for gets the cycle type's alone
3. the cycle type's **data source** — its dimension and scope members, always
4. the **workflow scenario's member set** in effect — any Flow or UD member it sets replaces the result
5. the workflow scenario's **Scenario Member** as the scenario
6. the combination's **data source** member last, so the data source always wins

"In effect" means on the cycle's **Start Date**; for a compare-to, that date moved by the compare-to's year offset. A unit, a [workflow area](../areas/index.md) or an item type never has a data source of its own; an area's Data Source Dim Member is a member of the cycle type's data source dimension.

### Base and scope members

**Base members** are where the unit's detail data is written.

**Scope members** are not a rollup target - they define the **outer bounds of the slice of the cube the unit is assigned to**. That boundary is used three ways:

- to present the unit's data in summary form in cube views
- to define the outer bounds of the data intersections that are synchronized with the detail data
- to define the data that is cleared for the unit

Because the scope members bound what gets synchronized and cleared, setting them wider than the unit actually owns risks clearing another unit's data, and setting them narrower than the detail risks leaving data behind.

### Data source scope members

The Readable, Controlled, Controlled Itemized, Controlled Direct and Controlled Actual members that used to be set here are now set once per cube, on the [cycle type's member set](../cycles/cycleTypes.md#data-source-scope-members). See that page for what each member covers and which synchronize or clear uses it.

## Typical Use Cases

- Mapping a department to the entity and account members its budget is written at
- Bounding a cost center's slice of the cube so its data can be summarized, synchronized and cleared correctly
- Mapping the same unit to different cubes for budget and forecast cycle types
- Re-mapping a unit from a given date after a restructure, without altering prior cycles

## Create a new Member Set

1. Go to **Admin | Workflow | Units**
2. Select the workflow unit the member set belongs to
3. Open the **Member Sets** child grid
4. Click on '**Add+**' or '**Add & Edit in Grid**'
5. Enter the Workflow Cycle Type and Cube, then only the members that differ from the cycle type's member set - usually just the Entity
6. Click **Save**

**Add & Edit in Grid** allows adding and modifying rows directly in the grid

!!!Note Important Notes
    The Workflow Unit is set from the unit the member set is added under

    See [General Actions](../../../../concepts/metadataDrivenUI/actions.md#general-actions) for more information about adding records

## Notes

- Only populate what is the unit's own; leave the rest blank so it takes the cycle type's member set rather than selecting a default member.
- Base and scope members serve different purposes - base is where the unit's detail data is written; scope bounds the region of the cube that data occupies.
- If a synchronize or clear affects more of the cube than expected, check the [data source](../cycles/cycleTypes.md#data-source-scope-members) member that operation uses before looking anywhere else; it is the boundary being applied.
- A unit with neither a member set of its own nor a cycle type member set for the cycle type being run has no cube destination, which is a common cause of data not appearing in the cube.
- Use effective dates to change a mapping over time rather than editing an existing member set in place.
