# Workflow Units

[← Back to Admin](../../index.md)

Workflow Units define **who or what a workflow is performed for**.

A workflow unit is the organizational entity a planning or reporting process is carried out against — a department, cost center, entity, region, or any other structure the process is divided by.

## Overview

Use Workflow Units to:

- define the entities a workflow process is performed for
- control which users can see and edit each unit's data through security groups
- limit a unit to a period of time using effective dates
- map each unit to the cube and dimension members its data belongs to

Each workflow unit definition consists of:

- [Units](units.md) – name, effective dates, security groups, and other unit-level settings
- [Member Sets](memberSets.md) – the cube and dimension members a unit's data is written to and summarized at, per workflow cycle type
- [Unit Hierarchies](hierarchies.md) – how units are arranged under one another, so a unit can review the work of those beneath it

## Key Concepts

- A workflow unit is paired with a workflow cycle to identify a specific piece of work: the cycle says *when*, the unit says *for whom*.
- Units are reusable across workflow cycles — the same unit participates in each cycle without being redefined.
- A unit's member set is what connects it to the cube. It holds only the unit's exceptions — usually its Entity — over the [cycle type's member set](../cycles/cycleTypes.md#member-sets-every-units-defaults), which every unit of the type shares.
- Member sets are effective-dated and scoped per cycle type, so the same unit can map differently for different kinds of workflow.
- Read-write and read-only access are granted separately, through two different security groups.
- A unit is either a **Base** unit at the bottom of a structure or a **Parent** unit that groups others. Both can hold records of their own — see [Unit Profile Type](units.md#unit-profile-type).
- Units are arranged into review and roll-up structures by [Unit Hierarchies](hierarchies.md), and a unit can belong to more than one.

## Typical Use Cases

- Departments submitting a budget
- Cost centers forecasting spend
- Regions or business units reporting actuals
- Entities completing a planning cycle

## Create a new Workflow Unit

1. Go to **Admin | Workflow | Units**
2. Click on '**Add+**' or '**Add & Edit in Grid**'
3. Click on the '**+**' button on the top left of the grid
4. Enter required fields and click **Save**
5. Create the unit's [Member Sets](memberSets.md) for each workflow cycle type it participates in

**Add & Edit in Grid** allows adding and modifying rows directly in the grid

!!!Note Important Notes
    The Integration Code and Workflow Unit Id will be auto-assigned

    See [General Actions](../../../../concepts/metadataDrivenUI/actions.md#general-actions) for more information about adding records

## Notes

- Name units after the business structure they represent, not after the workflow they take part in.
- A unit with no member set of its own gets only the cycle type's, which has no Entity, so in practice a posting unit needs one.
- Use effective dates rather than deleting units, so historical workflow data stays intact.
- A hierarchy has to be processed before anything reads it. Adding units to one is not enough on its own — see [Unit Hierarchies](hierarchies.md#process).
