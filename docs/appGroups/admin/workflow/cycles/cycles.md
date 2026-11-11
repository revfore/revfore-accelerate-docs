# Cycles

[← Back to Workflow Cycles Overview](index.md)

The **Workflow Cycles** page is used to define and manage the header fields for a workflow cycle.

A workflow cycle is one dated run of a workflow process — a single budget, forecast round, or close.

## Overview

Use the Workflow Cycles page to:

- create a workflow cycle for each budget, forecast or close
- review existing cycles and their status
- set the period a cycle covers and the [workflow scenario](workflowScenarios.md) it plans
- assign the security groups that control access to the cycle's data
- set what this cycle is compared with — a workflow scenario and a year

## Workflow Cycle Header Record Fields

The following fields are used for a Workflow Cycle header record.

| Field | Data Type| Purpose | Notes |
|---|---|---|---|
| Workflow Cycle Name | nvarchar | Internal name of the workflow cycle. | Must be unique. No spaces or special characters are allowed.
| Workflow Cycle Display Name | nvarchar | User-friendly display name shown in the application. | Must be unique.
| Workflow Cycle Description | nvarchar | Description of the workflow cycle and its purpose. | Required.
| Workflow Cycle Type | int | The kind of cycle this is. | Required. Workflow units, areas and member sets are configured per cycle type, so this determines which configuration applies.
| Workflow Scenario | int | What the cycle plans — Budget, Forecast, Actual. | Chosen from the [Workflow Scenarios](workflowScenarios.md). Its scenario member is the one the cycle's data posts to. One cycle per cycle type, workflow scenario and year.
| Year | int | The year the cycle relates to. |
| Status | int | Current status of the cycle. | Required. Chosen from the [Statuses](statuses.md) marked for use at cycle level.
| Default Unit Status | int | The status a unit starts at within this cycle. | Set this if units submit independently. Without it no [Cycle Unit](cycleUnits.md) records are created, and a submission grid comes up empty.
| Start Date | date | Date the cycle begins. |
| End Date | date | Date the cycle ends. |
| Start Period | int | First period the cycle covers. | Optional. Bounds the cycle in periods, alongside the calendar dates above.
| End Period | int | Last period the cycle covers. | Optional. Set with Start Period rather than on its own.
| Actual End Period | int | The last period containing actuals for the cycle. | Used where a cycle mixes actual and planned data, to mark where actuals stop.
| Compare To 1 Scenario | int | The workflow scenario this cycle is compared with - Actual for last year's actuals, Forecast for the prior forecast. | Optional. See [Compare-to](#compare-to).
| Compare To 1 Year Offset | int | The compare year, relative to this cycle's Year. | 0 is the same year, -1 the year before.
| Compare To 2 Scenario | int | A second workflow scenario to compare with. | Optional. Same rules as Compare To 1.
| Compare To 2 Year Offset | int | The year offset for Compare To 2. |
| Security Group | int | Security group granted read-write access to the cycle's data. |
| Read Security Group | int | Security group granted read-only access to the cycle's data. |
| Is Enabled | bit | Indicates whether the workflow cycle is enabled for use. |
| Integration Code | nvarchar | Unique value for the workflow cycle record. | This is readonly and provides a unique value for the record that is used for importing data
| OneStream Workflow Reference | nvarchar | How this cycle maps to a OneStream workflow. | Only needed when the solution is driven by OneStream workflow. Currently the scenario name and time key, as `Scenario\|TimeKey` (e.g. `Budget\|2026003000`). See [OneStream Workflow Reference](#onestream-workflow-reference) for the time key format. Leave blank otherwise.
| Created Date | datetime | Date and time the record was created. |
| Modified Date | datetime | Date and time the record was last modified. |
| Created By | int | User who created the workflow cycle record. |
| Modified By | int | User who last modified the workflow cycle record. |
| Workflow Cycle Id | int | Unique identifier for the workflow cycle record. | If you leave blank, the system will auto assign

## OneStream Workflow Reference

When a solution is driven by OneStream workflow, **OneStream Workflow Reference** is what ties a workflow cycle to the OneStream workflow it represents. It takes the form `Scenario|TimeKey` — for example `Budget|2026003000`.

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

    That means the same reference format yields one workflow cycle per scenario-period for a period workflow, and one per scenario-year for a yearly one. Seed cycles to match the grain the OneStream workflow actually runs at.

The unit side of the mapping works the same way — see the **OneStream Workflow Reference** field on [Units](../units/units.md), which holds the OneStream parent workflow profile name. Leave both blank when the solution is not driven by OneStream workflow.

## Status

Status is maintained on the [Statuses](statuses.md) page rather than being a fixed list, so a solution names its stages after its own process.

A cycle offers the statuses marked for use at cycle level. What a status restricts is set on the status itself: one marked **Read Only** stops data being entered for the whole cycle.

Use status to move a cycle through its lifecycle rather than deleting cycles that have finished.

### Per-unit status

A cycle's status covers everything in it. Where units submit independently — one department still drafting while another has been approved — each unit also carries its own status, held as a [Cycle Unit](cycleUnits.md) record.

The two combine as restrictions: **whatever either level forbids is forbidden.** A submitted unit is locked while its cycle stays open, and everything in a closed cycle is locked whatever the individual units say.

Set **Default Unit Status** on the cycle to turn this on. It is the stage a unit starts at, and without it no per-unit records are created.

## Compare-to

A cycle is rarely looked at on its own. A budget is read against last year's actuals; a forecast against the one before it. Each cycle has up to two compare-tos, each a **workflow scenario** and a **year offset**, so the comparison is configured once on the cycle rather than built into every report.

The compare year is the cycle's **Year** plus the offset: 0 is the same year, -1 the year before. FY26 Budget comparing with **Actual**, offset **-1**, compares with 2025 actuals.

**No cycle has to exist to be compared with.** The compare-to member sets are built from the workflow scenario and the year, so a cube comparison works whether or not a matching cycle was ever created.

What the solution gets from it, without looking anything up:

| Where | What |
|---|---|
| The workflow context | Each compare-to's workflow scenario, its scenario member, the year offset and the compare year |
| Member sets | A full compare-to member set beside the set's own — see [In cube views](#in-cube-views) |
| View filters, lookup filters and run-time where clauses | Tokens for the cycle's workflow scenario and each compare-to — see [Tokens](#tokens) |
| Extension code | The same values as properties of the full workflow context — see [In extension code](#in-extension-code) |

### Setting it up

1. Create the [workflow scenario](workflowScenarios.md) to compare with, if it doesn't exist yet.
2. On the cycle that does the comparing, pick it in **Compare To 1 Scenario** and set **Compare To 1 Year Offset**. Optionally set a second in **Compare To 2**.

### Tokens

| Token | Value |
|---|---|
| `{WfScenarioId}` | The cycle's workflow scenario id |
| `{WfScenarioName}` | Its name |
| `{WfScenarioDisplayName}` | Its display name |
| `{WfScenarioMemberName}` | The name of its scenario member. Replaces `{WfInstanceScenarioName}`, which has been removed |
| `{WfCompareTo1ScenarioId}` | Compare To 1's workflow scenario id |
| `{WfCompareTo1ScenarioName}` | Its name |
| `{WfCompareTo1ScenarioDisplayName}` | Its display name |
| `{WfCompareTo1ScenarioMemberName}` | The name of its scenario member |
| `{WfCompareTo1Year}` | The compare year — the cycle's year plus the offset |

The same five exist for `2`. A token is 0, or empty, when it isn't set. Text values are quoted for you, so write `= {WfCompareTo1ScenarioMemberName}`, not `= '{WfCompareTo1ScenarioMemberName}'`.

!!! warning "`{WfCompareTo1ScenarioName}` has changed meaning"
    It is now the compare-to **workflow scenario's** name. For the scenario member's name, use `{WfCompareTo1ScenarioMemberName}`. `{WfCompareTo1InstanceId}` and `{WfCompareTo1DisplayName}` (and their `2` forms) have been removed.

The tokens resolve wherever the user's workflow is known:

- **View filters** and **lookup filters**
- A view's **run-time Where Clause** (Flags 2), which is applied each time the view is read — in the grid, lookups, Top Count and File Export. See [Where Clause](../../relational/relationalViews/views.md#where-clause).

They do **not** resolve in a built-in Where Clause, which is compiled into the SQL view with no workflow context, so a token there would always be 0 or empty.

For example, a view of actuals that shows only the year this cycle compares with:

```sql
ActYear = {WfCompareTo1Year}
```

### In cube views

Every member set in the member set context has a **full compare-to set** for Compare To 1 and Compare To 2 — every dimension, not just the time and scenario. It is built the same way as the cycle's own (see [How the member set is built](workflowScenarios.md#how-the-member-set-is-built)), from the compare-to workflow scenario, with the cycle's start date moved by the year offset. Its time is the compare year and its scenario the compare-to workflow scenario's member.

When a compare-to isn't set, its values **fall back to the set's own**, so a cube view column built on them still resolves, to the current cycle, rather than failing.

In a cube view, read them from `param_RfaSharedWf_MemberSetContextJson` with `GetMemberSetValue` — the set (`Combo`), the dimension (`Field`) and the compare-to (`CompareTo`), for example:

```
T#XFBR(Workspace.Current.rfa_os.ParamHelper, GetMemberSetValue, Json = [|!param_RfaSharedWf_MemberSetContextJson!|], Combo = ControlledScope, Field = TimeName, CompareTo = 1)
S#XFBR(Workspace.Current.rfa_os.ParamHelper, GetMemberSetValue, Json = [|!param_RfaSharedWf_MemberSetContextJson!|], Combo = ControlledScope, Field = ScenarioName, CompareTo = 1)
E#XFBR(Workspace.Current.rfa_os.ParamHelper, GetMemberSetValue, Json = [|!param_RfaSharedWf_MemberSetContextJson!|], Combo = ControlledScope, Field = EntityName, CompareTo = 1)
```

Leave `CompareTo` out (or 0) for the current cycle. Older references that use `GetKeyValueFromJsonDictionary` with keys such as `ControlledScope_CompareTo1TimeName` still work.

A compare-to can differ from the cycle in more than its scenario — a workflow scenario's member set can override a version in UD5, say — and a cube view column can't know which. Use **`Field = ScenarioMemberFilter`** for those columns: it returns the compare-to's scenario plus every dimension where its set differs from the cycle's, ready to use as the column's member filter:

```
XFBR(Workspace.Current.rfa_os.ParamHelper, GetMemberSetValue, Json = [|!param_RfaSharedWf_MemberSetContextJson!|], Combo = ControlledItemizedScope, Field = ScenarioMemberFilter, CompareTo = 1)
```

This might return `S#Forecast:U5#V1`, or just `S#Actual` when only the scenario differs. Time and cube are never included: the cube view's columns and POV supply them.

Users can also choose their own compare-tos in the **change workflow** dialog: for each, a **Compare to** workflow scenario ("(The cycle's own)" or any enabled workflow scenario) and a **Year** ("Same year", "1 year earlier" to "5 years earlier", "1 year later" to "3 years later"). Their choice holds for the cycle they chose it for, and every compare-to value — member sets, expression tokens, extension code — follows it. A cycle type can hide these fields with **Show Compare To** (see [Cycle Types](cycleTypes.md)), for solutions that don't compare.

The Framework's own workflow cube views already have a **Compare To Year** column built this way, on Compare To 1.

### In extension code

The compare-to values are on the **full** workflow context:

```csharp
var oWfContext = args.ViewerBaseAndContextSettings.ViewerBaseSettings.Get_FullWorkflowContext(parms);
int iCompareYear = oWfContext.CompareTo1Year;
string sCompareScenario = oWfContext.CompareTo1ScenarioDmMemberName;
```

| Property | |
|---|---|
| `CompareTo1WfScenarioId` | The compare-to workflow scenario's id |
| `CompareTo1WfScenarioName` | Its name |
| `CompareTo1WfScenarioDisplayName` | Its display name |
| `CompareTo1YearOffset` | The year offset |
| `CompareTo1Year` | The compare year — the cycle's year plus the offset |
| `CompareTo1ScenarioDmMemberId` | Its scenario member's id |
| `CompareTo1ScenarioDmMemberName` | Its scenario member's name |

The same end in `2` for Compare To 2. All are 0 or empty when not set. The cycle's own workflow scenario is `WfScenarioId`, `WfScenarioName` and `WfScenarioDisplayName`. `Get_WorkflowContext`, the lighter call, carries ids and names only, without these values.

**Comparing relational data.** A cube comparison needs only the member sets. To compare relational rows, get the compare-to cycle — the one of the same cycle type with that workflow scenario and year — at run time:

```csharp
int iCompareWfCycleId = oWfContext.GetCompareToWfCycleId(parms, 1); // 0 when there is no such cycle
```

### Security

A compare-to names a comparison; it does not grant access to it. The compare-to's scenario and year are available to everyone using the cycle, but the data shown against them is still governed by each view's security and, for cube data, by OneStream's.

## Typical Use Cases

- FY26 Budget
- Q3 2026 Forecast
- March 2026 Close
- Mid-year re-forecast

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

- The cycle type drives which unit member sets and workflow areas apply, so set it correctly before work begins on the cycle.
- Description is required on a cycle, unlike most other workflow records.
- Start Period and End Period express the cycle in periods; Start Date and End Date express it as calendar dates. They describe the same window in two forms, so keep them consistent.
- Set Default Unit Status when creating the cycle, not later. It is what per-unit records start at, and they are only created for cycles that have one.
- Actual End Period is a different thing again - it marks where actuals stop *within* the cycle, and only matters where a cycle contains both actual and planned data.
- Compare To 1 and 2 only name the comparison. What is shown against it is up to the solution's views and cube views.
- There is one cycle per cycle type, workflow scenario and year. A branch or version of a plan, such as "Budget - Upside", is its own [workflow scenario](workflowScenarios.md#branches-and-versions), and so its own cycle.
