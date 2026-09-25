# Instance Units

[← Back to Workflow Instances Overview](index.md)

An **Instance Unit** record holds **one unit's own status inside one cycle**.

An instance has a single status covering the whole cycle. That is enough to open and close a round of work, but not enough to run a submission process: within an open cycle, one department may still be drafting while another has submitted and a third has been approved. Instance Units are where that per-unit progress lives.

## Overview

Use Instance Units to:

- track each unit's progress through a cycle independently
- let a unit's own status **lock its data** once it has been submitted, while the cycle stays open for everyone else
- record who submitted a unit and when
- hold the reviewer's comments against the unit they relate to

## Instance Unit Record Fields

| Field | Data Type | Purpose | Notes |
|---|---|---|---|
| Instance | int | The cycle this record belongs to. | Required. |
| Unit | int | The workflow unit. | Required. Shown only where the grid covers more than one unit — see [Where the Unit column appears](#where-the-unit-column-appears). |
| Current Status | nvarchar | The unit's status as it stands. | Read-only. Shown so the current stage is visible next to the one being chosen. |
| New Status | int | The status to move the unit to. | Chosen from the [Statuses](statuses.md) marked for use at instance-unit level. |
| Comments | nvarchar | Notes against this unit's submission or review. | |
| Submitted At | datetime | When the unit was submitted. | Stamped when a transition marks a submission. |
| Submitted By | int | Who submitted the unit. | Stamped at the same time. |
| Is Enabled | bit | Indicates whether the record is enabled. | |
| Integration Code | nvarchar | Unique value for the record. | This is readonly. |
| Created Date | datetime | Date and time the record was created. | |
| Modified Date | datetime | Date and time the record was last modified. | |
| Created By | int | User who created the record. | |
| Modified By | int | User who last modified the record. | |
| Workflow Instance Unit Id | int | Unique identifier for the record. | If you leave blank, the system will auto assign |

## A missing record means "inherit the cycle"

A unit with no record here is not a unit with no status. It means **the unit has not diverged from its cycle** — it takes the cycle's status, and behaves exactly as the instance says.

That matters for reading the grid. A unit showing nothing has not been forgotten; it simply has not moved yet.

## How records get created

You rarely create these by hand. When a user sets their workflow context — choosing an instance and a unit — the Framework makes sure that unit and everything beneath it in the [unit hierarchy](../units/hierarchies.md) has a record, so a submission grid has something in it when they arrive. The hierarchy used is the one the cycle resolves from its [instance type's Time](instanceTypes.md#time-which-hierarchy-applies) grid.

This happens only where it can:

- the instance must have a **Default Unit Status** set, which is the status new records start at
- the cycle must not be in a read-only status — there is no point seeding rows into a closed cycle

It never disturbs a status someone has already set: existing records are left exactly as they are, so re-entering a context is safe.

!!!Note
    If a submission grid comes up empty, check the instance's **Default Unit Status** first. Without one there is nothing for a new record to start at, and no records are created.

## Where the Unit column appears

The **Unit** column is hidden by default, because on most screens every row belongs to the same unit and the column would repeat one value down the page.

It appears by itself when the grid is genuinely showing more than one unit — that is, when the view is set to include descendant records and the unit you are working as is a **Parent**. Nothing needs configuring for this; it follows from the view's [Unit Records Shown](../../relational/relationalViews/views.md#workflow-behaviour) setting and the unit's [Profile Type](../units/units.md#unit-profile-type).

## Typical Use Cases

- A department submitting its budget while the cycle stays open for others
- A reviewer approving or rejecting the units beneath them
- Recalling a submitted unit so it can be corrected
- Reporting which units are still outstanding in a cycle

## Notes

- Statuses offered here are those with **Usage** set to Instance Unit or Both. See [Statuses](statuses.md#usage).
- A unit's status and its cycle's status combine as restrictions — whatever either forbids is forbidden. A submitted unit is locked even in an open cycle.
- Records are kept when a unit leaves a hierarchy, so a cycle's submission history stays intact.
- Comments belong to the unit and the cycle, not to the user, so they stay readable after a hand-over.
