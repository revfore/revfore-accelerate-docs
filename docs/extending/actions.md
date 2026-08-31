# View Actions in Code

[← Back to Extending the Framework](index.md)

An **action** is a button on a view. Most are built in and need no code. A **custom** action is one you define in configuration and implement in your extension handler.

This page covers the code side. For defining the action itself, see [Actions](../concepts/metadataDrivenUI/actions.md).

## Overview

Actions divide cleanly in two:

| Kind | Examples | Where it runs |
|---|---|---|
| **Built-in** | Save, Add, Edit, Delete, Copy, Bulk Update, Navigate, Audit, Reset | Entirely inside the closed assemblies |
| **Custom** | Anything you define | Your handler's `ActionHandler` |

Built-in actions never reach your code. If you want to influence what a Save or Delete does, use the [Before and After extension points](handlers/index.md#the-extension-points) instead — that is what they are for.

## Defining the action

Before writing any code, the action has to exist:

1. Open the view's **Actions** child grid
2. Add a row, setting the **Business Rule Flag** to **Custom**
3. Give it an action name — this is the string your code matches on
4. Save

The action name is the contract between configuration and code, so pick something stable and descriptive. Renaming it later means changing both.

## Handling it

`ActionHandler` receives every custom action for views your handler owns, so switch on the name:

```csharp
public void ActionHandler(RFParams parms,
    ref XFSelectionChangedTaskResult selectionChangedTaskResult,
    ref KeyValues keyValues,
    string actionName, string displayActionName,
    int relViewId, rel.View relView, int parameterSetId,
    List<string> selectedPrimaryKeyIds,
    /* ... */
    ref string infoMessage, ref string errorMessage) {

  switch(actionName) {

    case "RecalculateBudget":

      if(selectedPrimaryKeyIds.Count == 0) {
        errorMessage = "Select at least one row first.";
        return;
      }

      // do the work

      infoMessage = $"Recalculated {selectedPrimaryKeyIds.Count} row(s).";
      break;
  }
}
```

What you get to work with:

| Parameter | What it gives you |
|---|---|
| `actionName` | The name from configuration — what you match on |
| `selectedPrimaryKeyIds` | The rows the user had selected |
| `relView` | The view, including its model name, schema and base object |
| `infoMessage` / `errorMessage` | What the user sees afterwards |

Unlike the save and delete points, these two are plain strings rather than accumulating builders — assign to them rather than appending.

## Opening another view

A common action opens a different view, either standalone or filtered to the selected row. `SharedMethods` does the work — see [Shared Methods](shared-methods.md) for the full signatures.

```csharp
case "OpenSchedule":

  SharedMethods.OpenChildRelView(parms, ref selectionChangedTaskResult,
      viewerBaseAndContextSettings, ref launchingViewViewerBaseSettings,
      relViewId, relView, "PrjSchedule_vE",
      rel.ViewerBaseSettings.enuEditabilityModeFlags.ReadOnly,
      selectedPrimaryKeyIds, ref infoMessage, ref errorMessage);
  break;
```

`OpenRelView` opens a view on its own; `OpenChildRelView` opens one filtered by its relationship to the selected parent rows. Both take either a view **name** or a view **id** — the name overload looks the id up in the same schema as the current view, which is usually what you want.

## Getting the workflow context

When an action needs to know which workflow unit or instance the user is in:

```csharp
rfa_wf.WorkflowContext oWfContext =
    viewerBaseAndContextSettings.ViewerBaseSettings.Get_WorkflowContext(parms);

int iWfUnitId = oWfContext.WfUnitId;
int iWfInstanceId = oWfContext.WfInstanceId;
```

This is the same context the framework uses to resolve the unit's member set, so values taken from it line up with what synchronise and clear will act on.

## Notes

- Match on `actionName`, not `displayActionName` — the display name is a label and may be changed by an administrator.
- Always check `selectedPrimaryKeyIds` before acting on it; an action can be clicked with nothing selected.
- An action that only navigates usually needs no code at all — configure it as a Navigate action instead.
- Actions are matched inside the handler that owns the view, so two model families can use the same action name without colliding.
