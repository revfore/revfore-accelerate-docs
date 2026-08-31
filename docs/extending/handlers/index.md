# Extension Handlers

[← Back to Extending the Framework](../index.md)

An **extension handler** is a class that owns the custom behaviour for one model family. It implements `IExtensionHandler`, and the framework calls it whenever a user saves, copies, deletes, bulk updates, or clicks an action on a view built over one of those models.

## Overview

Use this page to understand:

- how the framework decides which handler owns a view
- what each extension point does and when it fires
- how to block an operation or return a message to the user

To create one, see [Adding a Handler](adding.md).

## How dispatch works

When something happens in a view, the closed engine calls the dispatcher, which resolves the handler by **model name prefix**:

```csharp
public static IExtensionHandler ResolveExtensionHandler(rel.View relView){

  if(relView.ModelName.StartsWith("Cpx", StringComparison.InvariantCultureIgnoreCase)){ return _oCpxHandler; }

  if(relView.ModelName.StartsWith("Drv", StringComparison.InvariantCultureIgnoreCase)){ return _oDrvHandler; }

  if(relView.ModelName.StartsWith("WfItem", StringComparison.InvariantCultureIgnoreCase)){ return _oWfItemHandler; }

  return null;
}
```

Three things follow from this:

**Matching is on the model name, not the view name.** Several views over the same model all route to the same handler. This is deliberate — the rules belong to the data, not to a particular screen.

**Returning `null` is normal.** A view whose model matches no prefix simply has no custom behaviour. Standard save, copy and delete all still work.

**Each handler is its own `if` / `return`.** Not an `else if` chain. That means a single line can be commented out to switch off one handler without disturbing its neighbours — which an `else if` chain cannot do, since removing the first branch leaves the next stranded.

!!!Note Prefix order matters
    The first matching prefix wins. If one prefix is itself the start of another, the shorter one shadows the longer. `RevRec` placed above `RevRecScheduleLine` would swallow every `RevRecScheduleLine` view. Put the more specific prefix first, or choose prefixes that cannot overlap.

Handlers are held as **static singletons** and reused across every call, so they must be stateless. Anything specific to the current user, record or request comes in through the method parameters — never stored on the class.

## The extension points

`IExtensionHandler` defines the points the framework will call. Implement the ones you need and leave the rest as empty stubs.

### Saving

| Method | When it fires | Typical use |
|---|---|---|
| `BeforeSaveHandler` | After the user's edits are collected, before they are written | Validate across records, default or correct values, block the save |
| `AfterSaveHandler` | After the rows are committed | Write to related tables, trigger a calculation, refresh dependent data |

`BeforeSaveHandler` receives the pending edits as a `rel.RecordSet` and can change them in place, so a value corrected here is what gets saved. `AfterSaveHandler` receives the primary keys of what was written, split into all, original and new — which lets you tell an insert from an update.

### Copying

| Method | When it fires | Typical use |
|---|---|---|
| `BeforeCopyHandler` | Before records are copied | Filter which records may be copied |
| `AfterCopyHandler` | After the copies exist | Fix up the new rows, copy child records |

`BeforeCopyHandler` **returns the list of primary keys to proceed with**. Return a shorter list to copy fewer records, or an empty list to copy none. `AfterCopyHandler` receives a dictionary mapping each original key to its new one, which is what you need to re-point child records.

### Deleting

| Method | When it fires | Typical use |
|---|---|---|
| `BeforeDeleteHandler` | Before anything is deleted | Filter what may be deleted; declare which referencing records you will handle |
| `BeforeDeleteCommitHandler` | Inside the delete, before commit | Delete dependent records yourself |
| `AfterDeleteHandler` | After the delete has committed | Clean up anything outside the model |

Delete has three points rather than two because of referential integrity. `BeforeDeleteHandler` returns the keys to proceed with, and also lets you list the referencing objects you intend to clear yourself — the framework then stops treating those references as blockers and leaves them to your `BeforeDeleteCommitHandler`.

### Bulk update

| Method | When it fires | Typical use |
|---|---|---|
| `BeforeBulkUpdate` | Before a bulk update is applied | Filter which of the selected records may be updated |
| `AfterBulkUpdate` | After the update is applied | Cascade the change, recalculate |

`AfterBulkUpdate` tells you which column was changed and gives the new value in whichever form fits its type — text, lookup, boolean or date.

### Actions and selection

| Method | When it fires | Typical use |
|---|---|---|
| `ActionHandler` | A custom action is clicked | Run the action — open a view, call a service, start a process |
| `ComponentSelectionChangedHandler` | A component's selection changes | React to the user picking something |

`ActionHandler` only receives **custom** actions. Built-in actions — Save, Add, Edit, Delete, Copy, Bulk Update, Navigate, Audit, Reset — are handled entirely inside the closed assemblies and never reach your code. See [Actions](../../concepts/metadataDrivenUI/actions.md) for defining one.

## Messages and blocking

Most extension points take `infoMessage` and `errorMessage` by reference:

- append to **`infoMessage`** to tell the user something while letting the operation continue
- append to **`errorMessage`** to stop the operation and show the reason

This is how validation is reported. There is no need to throw — writing to `errorMessage` is the supported way to reject an edit, and it produces a message the user can act on.

The `Before*` methods that return a `List<string>` block differently: rather than failing the whole operation, they narrow it. Returning fewer keys than you were given quietly proceeds with only those records.

Save and bulk update also take `lookupsNeedRefresh` by reference. Set it to `true` when your code has changed data that a lookup on the screen draws from, so the user sees current values without reloading.

## Notes

- Handlers are stateless singletons — never store request state in fields.
- Match on the model name; view names are not used for routing.
- A model family with no handler is perfectly valid and needs no stub.
- You cannot add a new extension point. The methods on `IExtensionHandler` are the ones the closed assemblies call, and adding to the interface will not cause anything to call it.
