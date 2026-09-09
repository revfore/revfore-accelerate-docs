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
| OneStream Workflow Reference | nvarchar | How this instance maps to a OneStream workflow. | Only needed when the solution is driven by OneStream workflow. Currently the scenario name and time key, as `Scenario\|TimeKey` (e.g. `Budget\|2026003000`). See [OneStream Workflow Reference](#onestream-workflow-reference) for the time key format. Leave blank otherwise.
| Created Date | datetime | Date and time the record was created. |
| Modified Date | datetime | Date and time the record was last modified. |
| Created By | int | User who created the workflow instance record. |
| Modified By | int | User who last modified the workflow instance record. |
| Workflow Instance Id | int | Unique identifier for the workflow instance record. | If you leave blank, the system will auto assign

## OneStream Workflow Reference

When a solution is driven by OneStream workflow, **OneStream Workflow Reference** is what ties a workflow instance to the OneStream workflow it represents. It takes the form `Scenario|TimeKey` — for example `Budget|2026003000`.

The time key is ten digits: the **four-digit year**, followed by a **six-digit period offset**.

| Offset | Period | Offset | Period | Offset | Period |
|---|---|---|---|---|---|
| `000000` | FY | `007000` | M4 | `013000` | M8 |
| `001000` | HY1 | `008000` | M5 | `014000` | M9 |
| `002000` | Q1 | `009000` | M6 | `015000` | Q4 |
| `003000` | M1 | `010000` | HY2 | `016000` | M10 |
| `004000` | M2 | `011000` | Q3 | `017000` | M11 |
| `005000` | M3 | `012000` | M7 | `018000` | M12 |
| `006000` | Q2 | | | | |

So `Budget|2026003000` is the Budget scenario for **M1 of 2026**, and `Budget|2026004000` is the same scenario for **M2**.

The offsets are not sequential by month — they step by 1000 through the time hierarchy in the order the members appear, with each summary level taking its own slot ahead of the periods beneath it:

```
FY ─┬─ HY1 ─┬─ Q1 ─┬─ M1  M2  M3
    │       │      └─ ...
    │       └─ Q2 ─── M4  M5  M6
    └─ HY2 ─┬─ Q3 ─── M7  M8  M9
            └─ Q4 ─── M10 M11 M12
```

Reading that top to bottom gives FY, HY1, Q1, M1, M2, M3, Q2, M4, M5, M6, HY2, Q3, M7, M8, M9, Q4, M10, M11, M12 — the offsets in order. This is why M4 is `007000` rather than `006000`: Q2 occupies the slot in between.

!!! note "The time key carries the workflow's grain"
    Take the key as OneStream gives it rather than normalising it to a month. A OneStream workflow run **by period** produces a period-level key such as `2026003000`; one run **by year** produces `2026000000`, the FY offset.

    That means the same reference format yields one workflow instance per scenario-period for a period workflow, and one per scenario-year for a yearly one. Seed instances to match the grain the OneStream workflow actually runs at.

The unit side of the mapping works the same way — see the **OneStream Workflow Reference** field on [Units](../units/units.md), which holds the OneStream parent workflow profile name. Leave both blank when the solution is not driven by OneStream workflow.

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
