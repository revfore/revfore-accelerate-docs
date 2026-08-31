# Adding a Handler

[← Back to Extension Handlers](index.md)

This page walks through adding custom behaviour for a new model family — creating the handler class and wiring it into the dispatcher.

## Before you start

You need:

- a **model** whose name starts with a distinctive prefix, and at least one view over it
- a prefix that does not overlap an existing one (see [prefix order](index.md#how-dispatch-works))
- access to the **Revfore Framework (RFA)** workspace in OneStream

Check first that [configuration](../index.md#configuration-first) does not already cover the requirement.

## Step 1: Create the handler class

In OneStream, go to **Application | Presentation | Workspaces**, select the **Revfore Framework (RFA)** workspace, and open **XCP_xRfaDlg_ActnExtension**, the extension assembly's maintenance unit.

Under **Assemblies | rfa_actnExtension_os**, add a folder for the model family beneath `DashboardExtenders/ExtensionHandlers/` and create the handler inside it:

```
DashboardExtenders/
└── ExtensionHandlers/
    └── PrjHandlers/
        └── PrjHandler.cs
```

Grouping by family keeps related files together — a family often grows to several, with the handler alongside its own helpers and service clients.

```csharp
namespace Workspace.__WsNamespacePrefix.rfa_actnExtension_os {

  public class PrjHandler : IExtensionHandler {

    public void BeforeSaveHandler(RFParams parms, int relViewId, rel.RecordSet recordSet,
        ref bool lookupsNeedRefresh,
        ref System.Text.StringBuilder infoMessage,
        ref System.Text.StringBuilder errorMessage) {

      // your rules here
    }

    // every other IExtensionHandler member, as an empty stub
  }
}
```

The interface must be implemented in full, so include every member even if the body is empty. Only the ones you fill in will do anything.

## Step 2: Write the behaviour

A validation rule in `BeforeSaveHandler` looks like this:

```csharp
foreach(rel.Record oRecord in recordSet.Records) {

  decimal dBudget = oRecord.Field("Prj_BudgetAmount").ModifiedValueDecimal;

  if(dBudget < 0) {
    errorMessage.AppendLine("Budget Amount cannot be negative.");
  }
}
```

Appending to `errorMessage` stops the save and shows the message. To correct a value instead of rejecting it, set it on the record and let the save continue.

Field names are the **view column** names, which carry the model's alias prefix — `Prj_BudgetAmount`, not `BudgetAmount`. The View+ screen shows the exact names.

## Step 3: Register the handler

Open `ExtensionHandlerDispatcher.cs`. Add a singleton alongside the others:

```csharp
private static readonly IExtensionHandler _oPrjHandler =
    new Workspace.__WsNamespacePrefix.rfa_actnExtension_os.PrjHandler();
```

Then add one line to `ResolveExtensionHandler`:

```csharp
if(relView.ModelName.StartsWith("Prj", StringComparison.InvariantCultureIgnoreCase)){ return _oPrjHandler; }
```

Place it so it does not shadow, or get shadowed by, an existing prefix. Keep it as its own `if` / `return` on one line — that is what allows the handler to be disabled later by commenting out a single line.

## Step 4: Save and test

Save the assembly. There is no separate deployment step.

Open a view over the model and exercise the behaviour — save a row that should fail, and one that should pass. If nothing happens at all, the usual cause is the prefix: confirm the **model** name really does start with it, since dispatch ignores view names.

## Adding a custom action

Actions follow the same pattern but arrive through `ActionHandler`, and the action must exist in configuration first.

1. Define the action on the view, with the **Business Rule Flag** set to **Custom** — see [Actions](../../concepts/metadataDrivenUI/actions.md)
2. Handle it in your handler, matched on the action name:

```csharp
public void ActionHandler(RFParams parms, /* ... */ string actionName, /* ... */
    ref string infoMessage, ref string errorMessage) {

  if(actionName == "RecalculateBudget") {
    // do the work
    infoMessage = "Budget recalculated.";
  }
}
```

Built-in actions never reach here — only the ones you have defined as custom.

## Notes

- Handlers are shared static singletons. Keep them stateless; everything you need arrives as a parameter.
- One handler can own several models if they share a prefix, which is usually what you want for a solution's tables.
- To disable a handler, comment out its line in `ResolveExtensionHandler`. The class can stay.
- If two handlers could claim a view, the first matching prefix wins.
