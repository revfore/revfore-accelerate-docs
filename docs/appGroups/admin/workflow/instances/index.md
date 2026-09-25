# Workflow Instances

[← Back to Admin](../../index.md)

Workflow Instances define **a specific run of a workflow process**.

Where a workflow unit says who the work is performed for, a workflow instance says which cycle it belongs to — the 2026 Budget, the Q3 Forecast, the March close.

## Overview

Use Workflow Instances to:

- define each cycle of a planning, forecasting or reporting process
- set the period a cycle covers and the scenario its data belongs to
- track the status of a cycle as it progresses
- control who can see and edit a cycle's data through security groups

Each workflow instance definition consists of:

- [Instances](instances.md) – the individual cycles, their dates, scenario, and status
- [Instance Types](instanceTypes.md) – the kinds of cycle available, such as Budget or Forecast, and the [unit hierarchy](instanceTypes.md#time-which-hierarchy-applies) that governs each over time
- [Statuses](statuses.md) – the stages a cycle or a unit can be in, and what each stage restricts
- [Instance Units](instanceUnits.md) – each unit's own status within a cycle, for processes where units submit independently

## Key Concepts

- An instance is paired with a workflow unit to identify a specific piece of work: the instance says *when*, the unit says *for whom*.
- Every instance has an instance type, which is what workflow units, areas, and member sets are configured against.
- The instance type is the reusable definition; the instance is the dated occurrence of it.
- An instance carries its own status, so cycles can be opened, worked, and closed independently.
- Security groups on an instance apply to that cycle, and work alongside the groups set on the unit.
- A cycle's status and a unit's own status combine as restrictions, so a unit can be locked inside a cycle that is still open.
- Statuses are records you maintain, not a fixed list, so the stages can be named after your own process.

## Typical Use Cases

- An annual budget cycle
- A quarterly forecast round
- A monthly close
- A re-forecast or scenario-planning exercise

## Create a new Workflow Instance

1. Go to **Admin | Workflow | Instances**
2. Click on '**Add+**' or '**Add & Edit in Grid**'
3. Click on the '**+**' button on the top left of the grid
4. Select the [Instance Type](instanceTypes.md) and enter the remaining required fields
5. Click **Save**

**Add & Edit in Grid** allows adding and modifying rows directly in the grid

!!!Note Important Notes
    The Integration Code and Workflow Instance Id will be auto-assigned

    See [General Actions](../../../../concepts/metadataDrivenUI/actions.md#general-actions) for more information about adding records

## Notes

- Create the instance type before the instances that use it.
- Name instances so the cycle is obvious at a glance, for example `FY26 Budget` rather than `Budget`.
- Use status to manage a cycle's lifecycle rather than deleting instances that have finished.
- If units submit independently, set the instance's **Default Unit Status** — per-unit records are only created for cycles that have one.
