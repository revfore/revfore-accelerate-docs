# Workflow Scenarios

[← Back to Workflow Cycles Overview](index.md)

The **Workflow Scenarios** page is used to define and manage what workflow cycles plan.

A workflow scenario is a named plan — Budget, Forecast, Actual — tied to one OneStream scenario member. Each workflow cycle has a workflow scenario, and a cycle compares with a workflow scenario and a year rather than with another cycle.

## Overview

Use the Workflow Scenarios page to:

- define the scenarios the solution's cycles plan
- name the OneStream scenario member each one posts to
- override the unit's Flow and UD members for a scenario, effective-dated
- define branches or versions of a plan, such as "Budget - Upside"

A workflow scenario is not limited to one cycle type. The scenario of a cycle is no longer set on the cycle or on the unit member set: it has one owner, the workflow scenario.

## Workflow Scenario Record Fields

The following fields are used for a Workflow Scenario record.

| Field | Data Type| Purpose | Notes |
|---|---|---|---|
| Workflow Scenario Name | nvarchar | Internal name of the workflow scenario. | Must be unique. No spaces or special characters are allowed.
| Workflow Scenario Display Name | nvarchar | User-friendly display name shown in the application. | Must be unique.
| Workflow Scenario Description | nvarchar | Description of the workflow scenario and its purpose. |
| Scenario Member | int | The OneStream scenario member this workflow scenario plans. | Required. Used for both base and scope — scenarios have no parents.
| Is Enabled | bit | Indicates whether the workflow scenario is enabled for use. | Only enabled workflow scenarios are offered in the change workflow dialog's compare-to fields.
| Integration Code | nvarchar | Unique value for the workflow scenario record. | This is readonly and provides a unique value for the record that is used for importing data
| Created Date | datetime | Date and time the record was created. |
| Modified Date | datetime | Date and time the record was last modified. |
| Created By | int | User who created the workflow scenario record. |
| Modified By | int | User who last modified the workflow scenario record. |
| Workflow Scenario Id | int | Unique identifier for the workflow scenario record. | If you leave blank, the system will auto assign

## Member Set: overriding the unit's members

A workflow scenario carries a **Member Set** grid. A row overrides members of the unit's [member set](../units/memberSets.md) for this scenario, from a date.

| Field | Data Type | Purpose | Notes |
|---|---|---|---|
| Workflow Scenario | int | The workflow scenario this row belongs to. | Set automatically when adding from within a workflow scenario.
| Effective Start Date | date | Date this row takes effect. | Defaults to 1900-01-01. One row per workflow scenario and start date.
| Effective End Date | date | Date this row stops applying. | Defaults to 2999-12-31, meaning no end date.
| Is Enabled | bit | Indicates whether the row is enabled. | A disabled row is skipped.
| Base Flow, Base UD1 to Base UD8 | int | Base members that replace the unit's. | Leave blank to use the unit's.
| Scope Flow, Scope UD1 to Scope UD8 | int | Scope members that replace the unit's. | Leave blank to use the unit's.
| Integration Code | nvarchar | Unique value for the record. | Readonly — built from the workflow scenario's integration code and the effective start date (`yyyymmdd`).
| Created Date | datetime | Date and time the record was created. |
| Modified Date | datetime | Date and time the record was last modified. |
| Created By | int | User who created the record. |
| Modified By | int | User who last modified the record. |
| Workflow Scenario Member Set Id | int | Unique identifier for the record. | If you leave blank, the system will auto assign

### How the member set is built

A cycle's member set — and each compare-to's — is built in steps, each over the last:

1. the **unit's member set** — its own exceptions, usually just the Entity
2. the **[cycle type's member set](cycleTypes.md#member-sets-every-units-defaults)** fills every member the unit leaves blank, and brings the [data source](cycleTypes.md#data-source)
3. the **workflow scenario's member set** in effect — any Flow or UD member it sets replaces the result
4. the workflow scenario's **Scenario Member** as the scenario
5. the combination's **data source** member last, so the data source always wins

"In effect" means on the cycle's **Start Date**. For a compare-to, that date is moved by the compare-to's year offset, so a comparison with last year uses the row that applied last year.

## Branches and versions

A branch or version of a plan is its own workflow scenario. "Budget - Upside" uses the same Scenario Member as "Budget", with a Member Set row that sets a different UD member, so its data sits beside the main budget in the same scenario.

Because there is one cycle per cycle type, workflow scenario and year, each branch is also its own cycle.

## Typical Use Cases

- Budget
- Forecast
- Actual
- Budget - Upside, Budget - Downside

## Create a new Workflow Scenario

1. Go to **Admin | Workflow | Cycles**
2. Open the **Workflow Scenarios** page
3. Click on '**Add+**' or '**Add & Edit in Grid**'
4. Enter required fields, including the **Scenario Member**, and click **Save**
5. Add **Member Set** rows only where this scenario should use Flow or UD members other than the unit's

**Add & Edit in Grid** allows adding and modifying rows directly in the grid

!!!Note Important Notes
    The Integration Code and Workflow Scenario Id will be auto-assigned

    See [General Actions](../../../../concepts/metadataDrivenUI/actions.md#general-actions) for more information about adding records

## Notes

- Create workflow scenarios before the cycles that use them.
- A cycle's compare-to is a workflow scenario and a year offset — see [Compare-to](cycles.md#compare-to). No cycle has to exist to be compared with.
- Change a scenario's members from a date by adding a Member Set row, not by editing the existing one, so earlier cycles keep resolving the members that applied to them.
