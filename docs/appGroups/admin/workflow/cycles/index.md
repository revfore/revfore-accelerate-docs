# Workflow Cycles

[← Back to Admin](../../index.md)

Workflow Cycles define **a specific run of a workflow process**.

Where a workflow unit says who the work is performed for, a workflow cycle says which round of the process it belongs to — the 2026 Budget, the Q3 Forecast, the March close.

## Overview

Use Workflow Cycles to:

- define each cycle of a planning, forecasting or reporting process
- set the period a cycle covers and the workflow scenario it plans
- track the status of a cycle as it progresses
- control who can see and edit a cycle's data through security groups

Each workflow cycle definition consists of:

- [Cycles](cycles.md) – the individual cycles, their dates, workflow scenario, status and what each is compared with
- [Cycle Types](cycleTypes.md) – the kinds of cycle available, such as Budget or Forecast, the [member set](cycleTypes.md#member-sets-every-units-defaults) and [data source](cycleTypes.md#data-source) every unit of the type shares, and the [unit hierarchy](cycleTypes.md#time-which-hierarchy-applies) that governs each over time
- [Workflow Scenarios](workflowScenarios.md) – what a cycle plans, such as Budget, Forecast or Actual: the OneStream scenario member and any Flow or UD members that override the unit's
- [Statuses](statuses.md) – the stages a cycle or a unit can be in, and what each stage restricts
- [Cycle Units](cycleUnits.md) – each unit's own status within a cycle, for processes where units submit independently

## Key Concepts

- A cycle is paired with a workflow unit to identify a specific piece of work: the cycle says *when*, the unit says *for whom*.
- Every cycle has a cycle type, which is what workflow units, areas, and member sets are configured against.
- The cycle type is the reusable definition; the cycle is the dated occurrence of it.
- A cycle carries its own status, so cycles can be opened, worked, and closed independently.
- Security groups on a cycle apply to that cycle, and work alongside the groups set on the unit.
- A cycle's status and a unit's own status combine as restrictions, so a unit can be locked inside a cycle that is still open.
- Statuses are records you maintain, not a fixed list, so the stages can be named after your own process.
- A cycle plans one workflow scenario, and compares with up to two others, each at a year offset. No cycle has to exist to be compared with.

## Typical Use Cases

- An annual budget cycle
- A quarterly forecast round
- A monthly close
- A re-forecast or scenario-planning exercise

## Create a new Workflow Cycle

1. Go to **Admin | Workflow | Cycles**
2. Click on '**Add+**' or '**Add & Edit in Grid**'
3. Click on the '**+**' button on the top left of the grid
4. Select the [Cycle Type](cycleTypes.md) and enter the remaining required fields
5. Click **Save**

**Add & Edit in Grid** allows adding and modifying rows directly in the grid

!!!Note Important Notes
    The Integration Code and Workflow Cycle Id will be auto-assigned

    See [General Actions](../../../../concepts/metadataDrivenUI/actions.md#general-actions) for more information about adding records

## Notes

- Create the cycle type and workflow scenario before the cycles that use them.
- Name cycles so the period and purpose are obvious at a glance, for example `FY26 Budget` rather than `Budget`.
- Use status to manage a cycle's lifecycle rather than deleting cycles that have finished.
- If units submit independently, set the cycle's **Default Unit Status** — per-unit records are only created for cycles that have one.
