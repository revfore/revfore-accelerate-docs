# Instance Types

[← Back to Workflow Instances Overview](index.md)

The **Workflow Instance Types** page is used to define and manage the kinds of workflow cycle available.

An instance type is the reusable definition — Budget, Forecast, Close — that individual workflow instances are created from.

## Overview

Use the Workflow Instance Types page to:

- define the kinds of cycle the solution supports
- review existing instance types
- enable or disable an instance type

The instance type is more than a label. Workflow units, workflow areas, and member sets are all configured **per instance type**, so it determines which configuration applies when a cycle runs.

## Workflow Instance Type Record Fields

The following fields are used for a Workflow Instance Type record.

| Field | Data Type| Purpose | Notes |
|---|---|---|---|
| Workflow Instance Type Name | nvarchar | Internal name of the workflow instance type. | Must be unique. No spaces or special characters are allowed.
| Workflow Instance Type Display Name | nvarchar | User-friendly display name shown in the application. | Must be unique.
| Workflow Instance Type Description | nvarchar | Description of the instance type and its purpose. |
| Is Enabled | bit | Indicates whether the instance type is enabled for use. |
| Integration Code | nvarchar | Unique value for the instance type record. | This is readonly and provides a unique value for the record that is used for importing data
| Created Date | datetime | Date and time the record was created. |
| Modified Date | datetime | Date and time the record was last modified. |
| Created By | int | User who created the instance type record. |
| Modified By | int | User who last modified the instance type record. |
| Workflow Instance Type Id | int | Unique identifier for the instance type record. | If you leave blank, the system will auto assign

## Time: which hierarchy applies

An instance type carries a **Time** grid, which says **which [unit hierarchy](../units/hierarchies.md) governs this kind of cycle, and from when**.

This is the only link between a workflow cycle and a hierarchy. An instance never names a hierarchy itself — it has an instance type and a start date, and those two resolve one.

| Field | Data Type | Purpose | Notes |
|---|---|---|---|
| Instance Type | int | The instance type this row belongs to. | Required. |
| Unit Hierarchy | int | The hierarchy that governs cycles of this type in this period. | Leave blank where the process is not hierarchical. |
| Effective Start Date | date | Date this row takes effect. | |
| Effective End Date | date | Date this row stops applying. | Defaults to 2999-12-31, meaning no end date. |
| Is Enabled | bit | Indicates whether the row is enabled. | A disabled row is skipped when resolving. |
| Integration Code | nvarchar | Unique value for the record. | Readonly — built from the instance type and the effective start date. |
| Created Date | datetime | Date and time the record was created. | |
| Modified Date | datetime | Date and time the record was last modified. | |
| Created By | int | User who created the record. | |
| Modified By | int | User who last modified the record. | |
| Workflow Instance Type Time Id | int | Unique identifier for the record. | If you leave blank, the system will auto assign |

### How the hierarchy is resolved

For a given cycle, the application takes the **instance's Start Date** and finds the enabled Time row for that instance type whose effective range contains it. Where more than one row qualifies, the **latest starting** one wins.

Two consequences worth knowing:

- **It is the instance's start date that decides, not today's date.** Reopening an old cycle resolves the hierarchy that applied when that cycle began, not the current one — so last year's budget still reviews up last year's structure.
- **Restructuring is a new row, not an edit.** End the current row and add one starting the day the new structure takes effect. Editing the existing row in place rewrites history, and cycles already closed will start resolving against a hierarchy that did not exist when they ran.

!!!Note
    No Time row, or one with no hierarchy, is a legitimate state — not every workflow is hierarchical. It simply means nothing resolves a hierarchy for these cycles, so views set to show descendant records will find nothing to show.

## Typical Use Cases

- Budget
- Forecast
- Close
- Long-range plan

## Create a new Workflow Instance Type

1. Go to **Admin | Workflow | Instances**
2. Open the **Instance Types** page
3. Click on '**Add+**' or '**Add & Edit in Grid**'
4. Enter required fields and click **Save**
5. Configure the [Workflow Units](../units/memberSets.md) and [Workflow Areas](../areas/index.md) that apply to this instance type
6. Add a **Time** row naming the [unit hierarchy](../units/hierarchies.md) that governs this kind of cycle, if the process is hierarchical

**Add & Edit in Grid** allows adding and modifying rows directly in the grid

!!!Note Important Notes
    The Integration Code and Workflow Instance Type Id will be auto-assigned

    See [General Actions](../../../../concepts/metadataDrivenUI/actions.md#general-actions) for more information about adding records

## Notes

- Keep the number of instance types small — one per genuinely different kind of process, not one per cycle.
- Changing an instance type after cycles exist affects which unit member sets and areas apply, so review the configuration before making changes.
- A `Default` instance type is supplied with the platform and can be used where a solution needs only one kind of cycle.
- If a review screen is empty when it should show the units below the current one, check the Time row before the hierarchy itself — a cycle whose start date falls outside every effective range resolves no hierarchy at all.
