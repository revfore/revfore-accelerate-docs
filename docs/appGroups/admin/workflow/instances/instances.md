# Instances

[← Back to Workflow Instances Overview](index.md)

The **Workflow Instances** page is used to define and manage the header fields for a workflow instance.

A workflow instance is one dated run of a workflow process — a single budget cycle, forecast round, or close.

## Overview

Use the Workflow Instances page to:

- create new workflow instances for each cycle
- review existing instances and their status
- set the period a cycle covers and the scenario its data belongs to
- assign the security groups that control access to the cycle's data

## Workflow Instance Header Record Fields

The following fields are used for a Workflow Instance header record.

| Field | Data Type| Purpose | Notes |
|---|---|---|---|
| Workflow Instance Name | nvarchar | Internal name of the workflow instance. | Must be unique. No spaces or special characters are allowed.
| Workflow Instance Display Name | nvarchar | User-friendly display name shown in the application. | Must be unique.
| Workflow Instance Description | nvarchar | Description of the workflow instance and its purpose. | Required.
| Workflow Instance Type | int | The kind of cycle this instance is. | Required. Workflow units, areas and member sets are configured per instance type, so this determines which configuration applies.
| Scenario Member | int | The scenario dimension member the cycle's data belongs to. | Used when posting the cycle's data to the cube.
| Year | int | The year the cycle relates to. |
| Status | int | Current status of the cycle. | Required. Chosen from a fixed list: Draft, Open, In-Review, In-Review & Locked, Pending Approval, Completed. See below.
| Start Date | date | Date the cycle begins. |
| End Date | date | Date the cycle ends. |
| Start Period | int | First period the cycle covers. | Optional. Bounds the cycle in periods, alongside the calendar dates above.
| End Period | int | Last period the cycle covers. | Optional. Set with Start Period rather than on its own.
| Actual End Period | int | The last period containing actuals for the cycle. | Used where a cycle mixes actual and planned data, to mark where actuals stop.
| Security Group | int | Security group granted read-write access to the cycle's data. |
| Read Security Group | int | Security group granted read-only access to the cycle's data. |
| Is Enabled | bit | Indicates whether the workflow instance is enabled for use. |
| Integration Code | nvarchar | Unique value for the workflow instance record. | This is readonly and provides a unique value for the record that is used for importing data
| Created Date | datetime | Date and time the record was created. |
| Modified Date | datetime | Date and time the record was last modified. |
| Created By | int | User who created the workflow instance record. |
| Modified By | int | User who last modified the workflow instance record. |
| Workflow Instance Id | int | Unique identifier for the workflow instance record. | If you leave blank, the system will auto assign

## Status values

Status is a fixed list rather than a lookup table, so the values are not maintained anywhere in the application:

| Value | Status |
|---|---|
| 1 | Draft |
| 2 | Open |
| 3 | In-Review |
| 4 | In-Review & Locked |
| 5 | Pending Approval |
| 6 | Completed |

Use status to move a cycle through its lifecycle rather than deleting instances that have finished.

## Typical Use Cases

- FY26 Budget
- Q3 2026 Forecast
- March 2026 Close
- Mid-year re-forecast

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

- The instance type drives which unit member sets and workflow areas apply, so set it correctly before work begins on the cycle.
- Description is required on an instance, unlike most other workflow records.
- Start Period and End Period express the cycle in periods; Start Date and End Date express it as calendar dates. They describe the same window in two forms, so keep them consistent.
- Actual End Period is a different thing again - it marks where actuals stop *within* the cycle, and only matters where a cycle contains both actual and planned data.
