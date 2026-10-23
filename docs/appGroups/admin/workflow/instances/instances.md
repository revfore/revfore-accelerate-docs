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
- name the cycles this one is compared with

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
| Status | int | Current status of the cycle. | Required. Chosen from the [Statuses](statuses.md) marked for use at instance level.
| Default Unit Status | int | The status a unit starts at within this cycle. | Set this if units submit independently. Without it no [Instance Unit](instanceUnits.md) records are created, and a submission grid comes up empty.
| Start Date | date | Date the cycle begins. |
| End Date | date | Date the cycle ends. |
| Start Period | int | First period the cycle covers. | Optional. Bounds the cycle in periods, alongside the calendar dates above.
| End Period | int | Last period the cycle covers. | Optional. Set with Start Period rather than on its own.
| Actual End Period | int | The last period containing actuals for the cycle. | Used where a cycle mixes actual and planned data, to mark where actuals stop.
| Compare To 1 | int | Another instance this cycle is compared with - last year's actuals, the prior forecast. | Optional. Any instance type, never the instance itself. See [Compare-to instances](#compare-to-instances).
| Compare To 2 | int | A second instance to compare with. | Optional. Same rules as Compare To 1.
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
    Take the key as OneStream gives it rather than normalizing it to a month. A OneStream workflow run **by period** produces a period-level key such as `2026003000`; one run **by year** produces `2026000000`, the FY offset.

    That means the same reference format yields one workflow instance per scenario-period for a period workflow, and one per scenario-year for a yearly one. Seed instances to match the grain the OneStream workflow actually runs at.

The unit side of the mapping works the same way — see the **OneStream Workflow Reference** field on [Units](../units/units.md), which holds the OneStream parent workflow profile name. Leave both blank when the solution is not driven by OneStream workflow.

## Status

Status is maintained on the [Statuses](statuses.md) page rather than being a fixed list, so a solution names its stages after its own process.

An instance offers the statuses marked for use at instance level. What a status restricts is set on the status itself: one marked **Read Only** stops data being entered for the whole cycle.

Use status to move a cycle through its lifecycle rather than deleting instances that have finished.

### Per-unit status

A cycle's status covers everything in it. Where units submit independently — one department still drafting while another has been approved — each unit also carries its own status, held as an [Instance Unit](instanceUnits.md) record.

The two combine as restrictions: **whatever either level forbids is forbidden.** A submitted unit is locked while its cycle stays open, and everything in a closed cycle is locked whatever the individual units say.

Set **Default Unit Status** on the instance to turn this on. It is the stage a unit starts at, and without it no per-unit records are created.

## Compare-to instances

A cycle is rarely looked at on its own. A budget is read against last year's actuals; a forecast against the one before it. **Compare To 1** and **Compare To 2** name up to two other instances to compare with, so the comparison is configured once on the cycle rather than built into every report.

Pick any instance — it does not have to be the same instance type, and actuals and plan usually are not. An instance cannot compare with itself.

What the solution gets from it, without looking anything up:

| Where | What |
|---|---|
| The workflow context | Each compare-to instance's id, display name, scenario and year |
| Member sets | The compare-to time (the year) and scenario, next to the set's own |
| View filters, lookup filters and run-time where clauses | Tokens for each compare-to instance — see [Tokens](#tokens) |
| Extension code | The same values as properties of the full workflow context — see [In extension code](#in-extension-code) |

Only the compared instance's own values are carried. If last year's budget itself compares with the year before, that second comparison is not followed.

### Setting it up

1. Create — or load — the instance you want to compare with. It has to exist before you can pick it.
2. On the cycle that does the comparing, pick it in **Compare To 1**, and optionally a second in **Compare To 2**.

The drop-down lists **every** instance, including disabled ones and those of other instance types, by name. Choosing the instance itself is refused when you save.

!!! warning "Clear the reference before deleting an instance"
    An instance that another instance compares with can't be deleted while that reference is there — the delete is refused. Clear **Compare To 1** or **2** on the instance that refers to it first.

### Tokens

| Token | Value |
|---|---|
| `{WfCompareTo1InstanceId}` | The compare-to instance's id |
| `{WfCompareTo1DisplayName}` | Its display name |
| `{WfCompareTo1ScenarioName}` | The name of its scenario member |
| `{WfCompareTo1Year}` | Its year |

The same four exist for `2`. A token is 0, or empty, when no compare-to is set. Text values are quoted for you, so write `= {WfCompareTo1ScenarioName}`, not `= '{WfCompareTo1ScenarioName}'`.

The tokens resolve wherever the user's workflow is known:

- **View filters** and **lookup filters**
- A view's **run-time Where Clause** (Flags 2), which is applied each time the view is read — in the grid, lookups, Top Count and File Export. See [Where Clause](../../relational/relationalViews/views.md#where-clause).

They do **not** resolve in a built-in Where Clause, which is compiled into the SQL view with no workflow context, so a token there would always be 0 or empty.

For example, a view of actuals that shows only the year this cycle compares with:

```sql
ActYear = {WfCompareTo1Year}
```

### In cube views

Each member set in the member set context carries `CompareTo1TimeName`, `CompareTo1ScenarioName`, `CompareTo2TimeName` and `CompareTo2ScenarioName`, next to its own time and scenario. A cube view reads the comparison with the same members, with only the time and scenario swapped.

When a compare-to isn't set — or has no year or scenario — these **fall back to the set's own time and scenario**, so a cube view column built on them still resolves, to the current cycle, rather than failing.

In a cube view, read them from `param_RfaSharedWf_MemberSetContextJson` with the set's name and the key, for example:

```
T#XFBR(Workspace.Current.rfa_os.ParamHelper, GetKeyValueFromJsonDictionary, Json = [|!param_RfaSharedWf_MemberSetContextJson!|], LookupKey = ControlledScope_CompareTo1TimeName)
S#XFBR(Workspace.Current.rfa_os.ParamHelper, GetKeyValueFromJsonDictionary, Json = [|!param_RfaSharedWf_MemberSetContextJson!|], LookupKey = ControlledScope_CompareTo1ScenarioName)
```

The Framework's own workflow cube views already have a **Compare To Year** column built this way, on Compare To 1.

### In extension code

The compare-to values are on the **full** workflow context:

```csharp
var oWfContext = args.ViewerBaseAndContextSettings.ViewerBaseSettings.Get_FullWorkflowContext(parms);
int iCompareYear = oWfContext.CompareToWfInstance1Year;
string sCompareScenario = oWfContext.CompareToWfInstance1ScenarioDmMemberName;
```

| Property | |
|---|---|
| `CompareToWfInstance1Id` | The instance's id |
| `CompareToWfInstance1DisplayName` | Its display name |
| `CompareToWfInstance1ScenarioDmMemberId` | Its scenario member's id |
| `CompareToWfInstance1ScenarioDmMemberName` | Its scenario member's name |
| `CompareToWfInstance1Year` | Its year |

The same five end in `2` for Compare To 2. All are 0 or empty when not set. `Get_WorkflowContext`, the lighter call, carries ids and names only, without the compare-to values.

### Security

Compare To names a comparison; it does not grant access to it. The instance's name, scenario and year are available to everyone using the cycle, but the data shown against them is still governed by each view's security and, for cube data, by OneStream's.

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
- Set Default Unit Status when creating the instance, not later. It is what per-unit records start at, and they are only created for cycles that have one.
- Actual End Period is a different thing again - it marks where actuals stop *within* the cycle, and only matters where a cycle contains both actual and planned data.
- Compare To 1 and 2 only name the comparison. What is shown against it is up to the solution's views and cube views.
