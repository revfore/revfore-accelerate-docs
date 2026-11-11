# Cycle Types

[← Back to Workflow Cycles Overview](index.md)

The **Workflow Cycle Types** page is used to define and manage the kinds of workflow cycle available.

A cycle type is the reusable definition — Budget, Forecast, Close — that individual workflow cycles are created from.

## Overview

Use the Workflow Cycle Types page to:

- define the kinds of cycle the solution supports
- review existing cycle types
- enable or disable a cycle type

The cycle type is more than a label. Workflow units, workflow areas, and member sets are all configured **per cycle type**, so it determines which configuration applies when a cycle runs. Its own **Member Sets** hold the members and the data source every unit of the type shares.

## Workflow Cycle Type Record Fields

The following fields are used for a Workflow Cycle Type record.

| Field | Data Type| Purpose | Notes |
|---|---|---|---|
| Workflow Cycle Type Name | nvarchar | Internal name of the workflow cycle type. | Must be unique. No spaces or special characters are allowed.
| Workflow Cycle Type Display Name | nvarchar | User-friendly display name shown in the application. | Must be unique.
| Workflow Cycle Type Description | nvarchar | Description of the cycle type and its purpose. |
| Is Enabled | bit | Indicates whether the cycle type is enabled for use. |
| Integration Code | nvarchar | Unique value for the cycle type record. | This is readonly and provides a unique value for the record that is used for importing data
| Created Date | datetime | Date and time the record was created. |
| Modified Date | datetime | Date and time the record was last modified. |
| Created By | int | User who created the cycle type record. |
| Modified By | int | User who last modified the cycle type record. |
| Workflow Cycle Type Id | int | Unique identifier for the cycle type record. | If you leave blank, the system will auto assign
| Workflow Selection Form | int | The size of the **change workflow** dialog this type's pages open. | One of `pge_RfaDlg_ActnOther_ChangeWorkflow_0_Small`, `_Medium`, `_Tall`, `_Large`, `_Wide` or `_Max`. Leave blank for Small. Choose a larger one when the dialog shows extra fields for this type.
| Show Compare To | bit | Whether the **change workflow** dialog offers the compare-to fields for this type. | On by default. Turn it off for a solution that uses workflow without posting to the cube or comparing: the dialog then shows only the cycle, the unit and any fields the solution adds, and the type's cycles compare only to their own compare-tos, if set.

## Time: which hierarchy applies

A cycle type carries a **Time** grid, which says **which [unit hierarchy](../units/hierarchies.md) governs this kind of cycle, and from when**.

This is the only link between a workflow cycle and a hierarchy. A cycle never names a hierarchy itself — it has a cycle type and a start date, and those two resolve one.

| Field | Data Type | Purpose | Notes |
|---|---|---|---|
| Cycle Type | int | The cycle type this row belongs to. | Required. |
| Unit Hierarchy | int | The hierarchy that governs cycles of this type in this period. | Leave blank where the process is not hierarchical. |
| Effective Start Date | date | Date this row takes effect. | |
| Effective End Date | date | Date this row stops applying. | Defaults to 2999-12-31, meaning no end date. |
| Is Enabled | bit | Indicates whether the row is enabled. | A disabled row is skipped when resolving. |
| Integration Code | nvarchar | Unique value for the record. | Readonly — built from the cycle type and the effective start date. |
| Created Date | datetime | Date and time the record was created. | |
| Modified Date | datetime | Date and time the record was last modified. | |
| Created By | int | User who created the record. | |
| Modified By | int | User who last modified the record. | |
| Workflow Cycle Type Time Id | int | Unique identifier for the record. | If you leave blank, the system will auto assign |

### How the hierarchy is resolved

For a given cycle, the application takes the **cycle's Start Date** and finds the enabled Time row for that cycle type whose effective range contains it. Where more than one row qualifies, the **latest starting** one wins.

Two consequences worth knowing:

- **It is the cycle's start date that decides, not today's date.** Reopening an old cycle resolves the hierarchy that applied when that cycle began, not the current one — so last year's budget still reviews up last year's structure.
- **Restructuring is a new row, not an edit.** End the current row and add one starting the day the new structure takes effect. Editing the existing row in place rewrites history, and cycles already closed will start resolving against a hierarchy that did not exist when they ran.

!!!Note
    No Time row, or one with no hierarchy, is a legitimate state — not every workflow is hierarchical. It simply means nothing resolves a hierarchy for these cycles, so views set to show descendant records will find nothing to show.

## Member Sets: every unit's defaults

A cycle type carries a **Member Sets** grid: for each cube, the members **every unit of the type** gets unless its own [unit member set](../units/memberSets.md) says otherwise, and the **data source** the type posts through. Typically it holds the IC, Flow and unused UD members (None), so each unit's member set needs only what is its own — usually just its Entity.

| Field | Data Type | Purpose | Notes |
|---|---|---|---|
| Workflow Cycle Type | int | The cycle type this row belongs to. | Set automatically when adding from within a cycle type.
| Cube | int | The cube these defaults apply to. | Required. One row per cycle type, cube and start date.
| Effective Start Date | date | Date this row takes effect. | Defaults to 1900-01-01.
| Effective End Date | date | Date this row stops applying. | Defaults to 2999-12-31, meaning no end date.
| Is Enabled | bit | Indicates whether the row is enabled. | A disabled row is skipped.
| Base Account, Base Flow, Base IC, Base UD1 to Base UD8 | int | Default base members. | A unit's own member replaces it.
| Scope Account, Scope Flow, Scope IC, Scope UD1 to Scope UD8 | int | Default scope members. | A unit's own member replaces it.
| Data Source Dimension Type | int | The data source dimension - the UD the cube keeps data origins in, and the one the scope members below are selected from. | Optional, but needed to post to the cube. See [Data source](#data-source).
| Data Source Readable Scope | int | Everything a unit can **see**. | See [Data source scope members](#data-source-scope-members).
| Data Source Controlled Scope | int | Everything a unit can **control**. |
| Data Source Controlled Itemized Scope | int | The itemized part of the controlled region. |
| Data Source Controlled Direct Scope | int | The directly entered part of the controlled region. |
| Data Source Controlled Actual Scope | int | The actuals part of the controlled region. |
| Integration Code | nvarchar | Unique value for the record. | Readonly — built from the cycle type, the cube and the effective start date (`yyyymmdd`).
| Created Date | datetime | Date and time the record was created. |
| Modified Date | datetime | Date and time the record was last modified. |
| Created By | int | User who created the record. |
| Modified By | int | User who last modified the record. |
| Workflow Cycle Type Member Set Id | int | Unique identifier for the record. | If you leave blank, the system will auto assign

There is no Entity — that is each unit's own — and no Scenario, which comes from the cycle's [workflow scenario](workflowScenarios.md).

**When you save**, every Base and Scope Flow, IC and UD member left blank is filled with the cube's default member. Account is left blank: it normally comes from the [item category](../supporting/itemCategoryMemberSets.md).

A unit's member set fills its blanks from the cycle type's member set for the same cube, in effect on the cycle's start date. A unit with no member set of its own for a cube gets the cycle type's alone. The data source always comes from the cycle type's member set. See [How the member set is built](../units/memberSets.md#how-the-member-set-is-built).

### Data source

A cube with a data source dimension - one UD reserved for "where did this number come from" - needs it named here, with five parent members in it that bound what a workflow can read and change. The data source is the **cycle type's alone**, per cube: every unit of the type gets it, and a unit, a [workflow area](../areas/index.md) or an [item type](../supporting/itemTypes.md) never names its own. An area's or item type's **Data Source Dim Member** - the base member its rows post to - is a member of this dimension.

The data source used to be set on every unit's member set. It is now set once per cycle type and cube, so changing it changes it for every unit of that type.

### Data source scope members

While a unit's scope members bound its slice across the ordinary dimensions, the data source members bound it along the data source dimension — and they carry an extra meaning. They separate what a unit is allowed to **look at** from what it is allowed to **change**.

Each member marks a region of the data source dimension:

| Member | What it covers |
|---|---|
| **Readable** | Everything the unit can **see**. The widest of the five - it bounds what the unit may read, whether or not it may change it. |
| **Controlled** | Everything the unit can **control**, meaning view, modify and delete. Always the same as, or a subset of, Readable. |
| **Controlled Itemized** | The part of the controlled region holding **itemized** data - detail built up from workflow items. This is where itemized detail is written when a unit is synchronized to the cube. |
| **Controlled Direct** | The part of the controlled region holding data **entered directly** against the unit rather than itemized through workflow items. |
| **Controlled Actual** | The part of the controlled region holding **actuals**. |

The last three sit inside Controlled, which sits inside Readable:

```
Readable
└── Controlled
    ├── Controlled Itemized
    ├── Controlled Direct
    └── Controlled Actual
```

The Readable/Controlled split is what lets a unit see more than it owns. A contributor can be shown surrounding context - other departments, prior actuals, group-level targets - while only the controlled region can be overwritten or cleared by that unit's own operations.

#### Which member each operation uses

Every synchronize and clear is bounded by one of these members, and acts on that member's **base** descendants:

| Operation | Member used |
|---|---|
| Synchronize itemized detail to the cube | Controlled Itemized |
| Clear all controlled data | Controlled |
| Clear itemized data | Controlled Itemized |
| Clear direct data | Controlled Direct |
| Clear actuals | Controlled Actual |

This is why the boundary matters as much as it does: the member chosen for an operation is exactly how much of the cube that operation can touch. Setting **Controlled** wider than the unit genuinely owns risks clearing another unit's data.

The dimension and all five members must be set for a unit that synchronizes to the cube. If any is missing, synchronizing the unit stops and reports which one.

## Typical Use Cases

- Budget
- Forecast
- Close
- Long-range plan

## Create a new Workflow Cycle Type

1. Go to **Admin | Workflow | Cycles**
2. Open the **Cycle Types** page
3. Click on '**Add+**' or '**Add & Edit in Grid**'
4. Enter required fields and click **Save**
5. Add a **Member Sets** row for each cube the type posts to, with its [data source](#data-source) and the members every unit shares
6. Configure the [Workflow Units](../units/memberSets.md) and [Workflow Areas](../areas/index.md) that apply to this cycle type
7. Add a **Time** row naming the [unit hierarchy](../units/hierarchies.md) that governs this kind of cycle, if the process is hierarchical

**Add & Edit in Grid** allows adding and modifying rows directly in the grid

!!!Note Important Notes
    The Integration Code and Workflow Cycle Type Id will be auto-assigned

    See [General Actions](../../../../concepts/metadataDrivenUI/actions.md#general-actions) for more information about adding records

## Notes

- Keep the number of cycle types small — one per genuinely different kind of process, not one per cycle.
- Changing a cycle type after cycles exist affects which unit member sets and areas apply, so review the configuration before making changes.
- Change the shared members or data source from a date by adding a Member Sets row, not by editing the existing one, so earlier cycles keep resolving what applied to them.
- Readable and Controlled answer different questions - Readable is what the unit may look at, Controlled is what it may change. Controlled can never be wider than Readable.
- If a synchronize or clear affects more of the cube than expected, check the data source member that operation uses before looking anywhere else; it is the boundary being applied.
- A `Default` cycle type is supplied with the platform and can be used where a solution needs only one kind of cycle.
- If a review screen is empty when it should show the units below the current one, check the Time row before the hierarchy itself — a cycle whose start date falls outside every effective range resolves no hierarchy at all.
