# Workflow Units

[← Back to Admin](../../index.md)

Workflow Units define **who or what a workflow is performed for**.

A workflow unit is the organisational entity a planning or reporting process is carried out against — a department, cost centre, entity, region, or any other structure the process is divided by.

## Overview

Use Workflow Units to:

- define the entities a workflow process is performed for
- control which users can see and edit each unit's data through security groups
- limit a unit to a period of time using effective dates
- map each unit to the cube and dimension members its data belongs to

Each workflow unit definition consists of:

- [Units](units.md) – name, effective dates, security groups, and other unit-level settings
- [Member Sets](memberSets.md) – the cube and dimension members a unit's data is written to and summarised at, per workflow instance type

## Key Concepts

- A workflow unit is paired with a workflow instance to identify a specific piece of work: the instance says *when*, the unit says *for whom*.
- Units are reusable across workflow instances — the same unit participates in each cycle without being redefined.
- A unit's member set is what connects it to the cube, so a unit without one has no cube destination for its data.
- Member sets are effective-dated and scoped per instance type, so the same unit can map differently for different kinds of workflow.
- Read-write and read-only access are granted separately, through two different security groups.

## Typical Use Cases

- Departments submitting a budget
- Cost centres forecasting spend
- Regions or business units reporting actuals
- Entities completing a planning cycle

## Create a new Workflow Unit

1. Go to **Admin | Workflow | Units**
2. Click on '**Add+**' or '**Enable Inline Adding & Editing**'
3. Click on the '**+**' button on the top left of the grid
4. Enter required fields and click **Save**
5. Create the unit's [Member Sets](memberSets.md) for each workflow instance type it participates in

**Enable Inline Adding & Editing** allows adding and modifying rows directly in the grid

!!!Note Important Notes
    The Ext Ref Unique Code and Workflow Unit Id will be auto-assigned

    See [General Actions](../../../../concepts/metadataDrivenUI/actions.md#general-actions) for more information about adding records

## Notes

- Name units after the business structure they represent, not after the workflow they take part in.
- A unit with no member set can still be created, but its data has nowhere to post to the cube.
- Use effective dates rather than deleting units, so historical workflow data stays intact.
