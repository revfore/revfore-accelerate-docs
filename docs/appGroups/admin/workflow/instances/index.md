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
- [Instance Types](instanceTypes.md) – the kinds of cycle available, such as Budget or Forecast

## Key Concepts

- An instance is paired with a workflow unit to identify a specific piece of work: the instance says *when*, the unit says *for whom*.
- Every instance has an instance type, which is what workflow units, areas, and member sets are configured against.
- The instance type is the reusable definition; the instance is the dated occurrence of it.
- An instance carries its own status, so cycles can be opened, worked, and closed independently.
- Security groups on an instance apply to that cycle, and work alongside the groups set on the unit.

## Typical Use Cases

- An annual budget cycle
- A quarterly forecast round
- A monthly close
- A re-forecast or scenario-planning exercise

## Create a new Workflow Instance

1. Go to **Admin | Workflow | Instances**
2. Click on '**Add+**' or '**Enable Inline Adding & Editing**'
3. Click on the '**+**' button on the top left of the grid
4. Select the [Instance Type](instanceTypes.md) and enter the remaining required fields
5. Click **Save**

**Enable Inline Adding & Editing** allows adding and modifying rows directly in the grid

!!!Note Important Notes
    The Ext Ref Unique Code and Workflow Instance Id will be auto-assigned

    See [General Actions](../../../../concepts/metadataDrivenUI/actions.md#general-actions) for more information about adding records

## Notes

- Create the instance type before the instances that use it.
- Name instances so the cycle is obvious at a glance, for example `FY26 Budget` rather than `Budget`.
- Use status to manage a cycle's lifecycle rather than deleting instances that have finished.
