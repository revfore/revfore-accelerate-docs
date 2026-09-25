# Statuses

[← Back to Workflow Instances Overview](index.md)

A **Workflow Status** is a named stage a cycle — or a single unit within a cycle — can be in, together with what that stage restricts.

Statuses used to be a fixed list built into the application. They are now records you maintain, so a solution can name its stages after its own process rather than working around someone else's.

## Overview

Use Statuses to:

- define the stages your process moves through, in the language your users already use
- mark which stages **stop data being entered**, so a closed or submitted stage locks its data
- decide whether a stage applies to a whole cycle, to a single unit, or to both
- control the order the stages appear in

## Workflow Status Record Fields

The following fields are used for a Workflow Status record.

| Field | Data Type | Purpose | Notes |
|---|---|---|---|
| Sequence | int | The order the status appears in lists. | Number in tens — 10, 20, 30 — so a stage can be added between two later without renumbering. |
| Name | nvarchar | Internal name of the status. | Must be unique. No spaces or special characters are allowed. |
| Display Name | nvarchar | User-friendly display name shown in the application. | This is what users see on the workflow bar. |
| Description | nvarchar | Description of the status and what it means. | |
| Behavior | int | What the status restricts. | Blank restricts nothing. **Read Only** stops data being entered. See [Behavior](#behavior). |
| Usage | int | Whether the status applies to instances, to instance units, or to both. | See [Usage](#usage). |
| Is Enabled | bit | Indicates whether the status is available for use. | Disable a status you no longer want chosen rather than deleting one that historical records still point at. |
| Integration Code | nvarchar | Unique value for the status record. | Used when importing data. |
| XRef Code | nvarchar | External reference for the status. | Optional. |
| Created Date | datetime | Date and time the record was created. | |
| Modified Date | datetime | Date and time the record was last modified. | |
| Created By | int | User who created the record. | |
| Modified By | int | User who last modified the record. | |
| WfStatusId | int | Unique identifier for the status record. | If you leave blank, the system will auto assign |

## Behavior

**Behavior** says what a status takes away. It is a restriction, not a permission — a status with no behaviour set does not grant anything, it simply restricts nothing.

| Value | Behavior | Effect |
|---|---|---|
| *(blank)* | — | The stage restricts nothing. Data can be entered as normal. |
| 1 | Read Only | Data entry is off. Grids are not editable and Add, Edit and Delete buttons are unavailable. |

Because it is a restriction, the instance's status and the unit's status combine: **whatever either level forbids is forbidden**. A unit that has been submitted is read-only even while its cycle is still open, and everything in a closed cycle is read-only whatever the individual units say.

!!!Note
    Read Only does not stop the workflow itself moving on. A **Recall** or **Approve** button still works on a submitted unit — see [Workflow actions](../../relational/relationalViews/actions.md#workflow-actions) for how an action is marked as a workflow transition rather than an edit.

## Usage

**Usage** says where a status can be chosen.

| Value | Usage | Meaning |
|---|---|---|
| 1 | Instance | Offered for a whole cycle only. |
| 2 | Instance Unit | Offered for a single unit within a cycle only. |
| 3 | Both | Offered at either level. |

Most solutions want a few of each. *Draft*, *Open* and *Closed* describe a cycle; *Not Started*, *In Progress*, *Submitted* and *Approved* describe one unit's progress within it. A status such as *Locked* may be worth having at both levels.

## Typical Use Cases

- Cycle stages — Draft, Open, Closed
- Unit submission stages — Not Started, In Progress, Submitted, Approved, Rejected
- A locked stage that freezes data mid-cycle without closing it

## Create a new Status

1. Go to **Admin | Workflow | Instances**
2. Select **Workflow Statuses**
3. Click on '**Add+**' or '**Add & Edit in Grid**'
4. Enter the name, display name and sequence
5. Set **Behavior** to Read Only if the stage should stop data entry, and set **Usage** to say where it can be chosen
6. Click **Save**

**Add & Edit in Grid** allows adding and modifying rows directly in the grid

!!!Note Important Notes
    The Integration Code and WfStatusId will be auto-assigned

    See [General Actions](../../../../concepts/metadataDrivenUI/actions.md#general-actions) for more information about adding records

## Notes

- Decide the behaviour before the name. A status that reads as final but restricts nothing is the most common configuration mistake here.
- Number sequences in tens, so a stage can be inserted later without renumbering the ones after it.
- Set Usage deliberately. A unit-level status offered on an instance, or the other way round, will be picked by someone eventually.
- Statuses are referenced by instances and by [Instance Units](instanceUnits.md), so disable rather than delete.
