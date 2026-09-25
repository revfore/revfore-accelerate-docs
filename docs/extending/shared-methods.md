# Shared Methods

[← Back to Extending the Framework](index.md)

`SharedMethods` is a Revfore helper class in the extension assembly, holding operations that most handlers need. It is unencrypted like the rest of the assembly, so you can read it — but it is Revfore's file to change, not yours. Put your own helpers in [your own files](#adding-your-own).

## Overview

| Method | Use it to |
|---|---|
| `OpenRelView` | Open another view from an action |
| `OpenChildRelView` | Open a child view filtered to the selected parent rows |
| `HandleDefaultAction` | Fall back to the framework's standard handling for an action |
| `FirstPeriodValueChanged` | Detect that the user edited the first period in a spread |
| `AreAllOriginalPeriodValuesEqual` | Test whether periods were evenly spread before the edit |
| `AreAllModifiedPeriodValuesExceptTheFirstEqual` | Test whether the remaining periods are still even |
| `SetAllModifiedValuesToFirstModifiedValue` | Spread the first period's value across the rest |

## Opening views

### `OpenRelView`

Opens a view in its own right.

```csharp
SharedMethods.OpenRelView(parms, ref selectionChangedTaskResult,
    viewerBaseAndContextSettings, ref launchingViewViewerBaseSettings,
    sourceRelViewId, relView, "PrjBudget_vE",
    rel.ViewerBaseSettings.enuEditabilityModeFlags.ReadOnly,
    selectedPrimaryKeyIds, ref infoMessage, ref errorMessage);
```

There are two overloads — one taking the target view's **name**, one taking its **id**. The name overload resolves the id within the same schema as the current view, which is what you want unless you are crossing schemas deliberately.

The method sets the opened view up for you: it loads the viewer settings, applies the current user's security, and suppresses **Add+**, **Edit+** and **Navigate** on the opened view so users cannot nest navigation indefinitely.

### `OpenChildRelView`

Opens a view filtered by its relationship to the rows the user selected.

```csharp
SharedMethods.OpenChildRelView(parms, ref selectionChangedTaskResult,
    viewerBaseAndContextSettings, ref launchingViewViewerBaseSettings,
    parentRelViewId, relView, "PrjTask_vE",
    rel.ViewerBaseSettings.enuEditabilityModeFlags.Editable,
    selectedPrimaryKeyIds, ref infoMessage, ref errorMessage);
```

The difference from `OpenRelView` is that this one resolves the **relationship** between the parent view and the child, and applies the selected parent keys as the filter — so the user sees only the children of what they had selected.

Both take an editability flag, so the same view can be opened read-only from one action and editable from another.

## Workflow context

Both open methods read the workflow context internally, and you can do the same when an action needs to know where the user is:

```csharp
rfa_wf.WorkflowContext oWfContext =
    viewerBaseAndContextSettings.ViewerBaseSettings.Get_WorkflowContext(parms);
```

From it you can read `WfUnitId`, `WfInstanceId` and the unit's resolved member set values — including the [data source scope members](../appGroups/admin/workflow/units/memberSets.md#data-source-scope-members) that bound what the unit may read and change.

### The context is a snapshot

The workflow context is **cached state, not a live read.** It is written when the user sets their workflow settings, and nothing invalidates it when the underlying instance or unit changes — including when your own code is what changed it.

That bites hardest on a status change, because the context carries not just the status name but the behaviour behind it. An action that submits a unit and does not refresh the context leaves the rest of the session believing the unit is still open: the grid stays editable, the buttons stay live, and the first thing anyone notices is the save that refuses.

After any code that moves a status, refresh it:

```csharp
rfa_wf.WorkflowContext.RefreshCurrentStatuses(parms);
```

One line, after the status write has committed. It re-reads both the instance's status and the unit's, stores the result, and returns the refreshed context. It only writes when something actually moved, so calling it on a path that may not have changed anything costs nothing.

**That is the whole job — you do not also need to touch the viewer settings.** They rebuild themselves from the stored context on the next request.

!!!Note
    A grid refresh is not the same thing. `RequestGridRefresh` redraws the rows; it does not rebuild the context that decides whether they are editable. An action that changed a status usually wants both.

## Period helpers

Views that spread a value across periods share a common editing pattern: the user types into the first period and expects the rest to follow, but only while the row is still evenly spread. Once they have edited an individual period, overwriting their work would be wrong.

The four period helpers exist to express that rule. They take a dictionary of period number to field:

```csharp
if(SharedMethods.FirstPeriodValueChanged(dictPeriodFields)
   && SharedMethods.AreAllOriginalPeriodValuesEqual(dictPeriodFields)) {

  SharedMethods.SetAllModifiedValuesToFirstModifiedValue(dictPeriodFields);
}
```

Read as: *if the user changed the first period, and the row was evenly spread before they touched it, then spread the new value across all periods.* A row the user has already shaped by hand fails the second test and is left alone.

`AreAllModifiedPeriodValuesExceptTheFirstEqual` covers the variant where you care whether the trailing periods are still even after the edit.

## Solution setup

`SolutionHelper.Setup_RunSolutionSetupScriptsAndProcesses` is called during **Setup and Upgrade**, on both the install and the upgrade path, before the framework relinks its OneStream records.

```csharp
public static void Setup_RunSolutionSetupScriptsAndProcesses(
    RFParams parms, Guid workspaceId, int prevVersion, int currentVersion,
    ref string errorMessage, ref string infoMessage)
```

It is where a solution does the work that has to happen as part of being installed or upgraded, rather than by hand afterwards — running its own scripts, seeding reference data, or processing something the new version depends on.

`prevVersion` and `currentVersion` are what make it safe to call every time: compare them to decide whether a step is needed, so an upgrade that skips two versions still runs each step it should, and a re-run of the same version does nothing.

Report what happened through `infoMessage`, and anything that went wrong through `errorMessage` — both are surfaced on the setup screen.

See [Setup and Upgrade](../appGroups/config/setup-upgrade.md).

## Adding your own

!!!Note Leave `SharedMethods.cs` as it is
    `SharedMethods.cs` is a Revfore file. It sits in the extension assembly and is not encrypted, so nothing stops you editing it — but do not. Revfore ships enhanced versions of it, and taking a new one is a straight replacement only while the file is untouched. Edit it and every upgrade becomes a merge, with your additions to weigh against theirs.

Put your own helpers in **your own files** instead, alongside it in `DashboardExtenders/`:

```
DashboardExtenders/
├── SharedMethods.cs           Revfore's - replaced on upgrade, never edited
├── PrjSharedMethods.cs        yours
└── ExtensionHandlers/
    └── PrjHandlers/
        └── PrjHandler.cs
```

Name them for the solution or the area they serve rather than by function, so it stays obvious at a glance which files are yours and which arrived with the framework.

What belongs in one:

- anything two or more of your handlers need — resolving a code to an id, formatting a reference, applying a rule that spans model families
- wrappers that give a Revfore method your own defaults, rather than changing the original

Keep them static and stateless, as `SharedMethods` is — everything a method needs should arrive as a parameter. Anything specific to a single model family belongs in that family's handler, not in a shared file.

## Notes

- The open methods deliberately hide Add+, Edit+ and Navigate on the view they open. If you need those, the action should navigate rather than open.
- Pass a view name rather than an id where you can; it survives a rebuild of the metadata, an id may not.
- The period helpers only decide *whether* to spread — they do not save anything. Flag and save the records as usual afterwards.
- Do not edit `SharedMethods.cs`. Add your own file beside it, so an enhanced version from Revfore drops straight in.
