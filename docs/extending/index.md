# Extending the Framework

Revfore Framework is designed so that most of a solution is **configured**, not coded. Tables, models, views, actions and security are all defined as metadata, and the framework builds the screens from that.

Some behaviour cannot be expressed as configuration — a validation rule that depends on other records, a calculation that runs when a row is saved, a button that pushes data to the cube. That is what the **extension assembly** is for.

## Overview

Use this section to:

- understand which parts of the framework you can change and which you cannot
- decide whether a requirement needs configuration or code
- write and wire up an extension handler
- work with the framework's record objects from your own code

## What you can and cannot change

Revfore Framework ships as a set of OneStream workspace assemblies. All but one are closed product code.

| Assembly | Yours to edit? | What it is |
|---|---|---|
| **`rfa_actnExtension_os`** | **Yes** | The extension assembly. The only unencrypted, editable assembly in the stack. All solution-specific code lives here. |
| `rfa_os` | No | Core engine — builds screens from metadata and calls into your extension code. |
| `rfa_shared_os` | No | Shared services used across the framework. |
| `rfa_wf_os` | No | Workflow engine, cube synchronise and clear. |
| `rfa_actnFile_os` | No | File import and export actions. |
| `rfa_actnOther_os` | No | Additional built-in actions. |
| `rfa_conSubItmCnfg_os`, `rfa_expressionEditor_os`, `rfa_solCnfg_os` | No | Configuration and editor support. |

This matters more than it might appear. Because the closed assemblies call *into* your extension assembly at fixed points, upgrading the framework does not overwrite your code — and your code cannot destabilise the engine. You extend at defined seams rather than by modifying the product.

!!!Note Two levels of extension
    Partners building a solution on the framework and customers extending that partner's solution both write code in the same place. There is one extension assembly per instance, so plan with your partner how it is shared if both parties will be adding handlers.

## Configuration first

Before writing code, check whether configuration already covers it. Code is harder to test, harder to upgrade around, and invisible to the administrators maintaining the solution.

Configuration usually handles:

- **Field-level rules** — required, read-only, default values, formats, and lookups are all view column settings
- **Row-level security** — join to the flattened security views rather than filtering in code
- **Derived display values** — expression columns compute values in the model
- **Navigation** — actions can open another view without any code

Reach for code when the requirement involves something configuration genuinely cannot express: cross-record validation, writing to other tables, calling an external service, or driving a cube operation.

## The shape of an extension

Everything you write plugs in through one interface, `IExtensionHandler`, and one dispatcher.

1. The user does something in a view — saves a row, copies records, clicks an action
2. The closed engine calls the extension dispatcher
3. The dispatcher works out which of your handlers owns that view, based on its **model name**
4. Your handler runs, and can add messages, change values, block the operation, or do additional work
5. If no handler claims the view, nothing custom happens and the standard behaviour continues

That last point is worth holding on to: a view with no matching handler is not an error. Extension code is opt-in per model family.

See [Extension Handlers](handlers/index.md) for how the dispatch works and what each extension point can do.

## Where the code lives

Inside OneStream:

1. Go to **Application | Presentation | Workspaces**
2. Select the **Revfore Framework (RFA)** workspace
3. Open **XCP_xRfaDlg_ActnExtension**, the extension assembly's maintenance unit
4. The files are under **'Assemblies | rfa_actnExtension_os`**

The structure you will find:

```
rfa_actnExtension_os/
└── DashboardExtenders/
    ├── IExtensionHandler.cs            the interface every handler implements
    ├── ExtensionHandlerDispatcher.cs   routes a view to its handler
    ├── SolutionHelper.cs               entry point the closed assemblies call
    ├── SharedMethods.cs                helpers for opening views, reading context
    └── ExtensionHandlers/
        ├── WfItemHandlers
            ├── WfItemHandler.cs            Can have many files in the folder
        ├── CpxItemHandlers
            ├── CpxHandler.cs
        ├── OtherHandlers
            └── ...
```

You will normally only add folders and files under **`ExtensionHandlers/`** and add one line to **`ExtensionHandlerDispatcher.cs`**.

## Notes

- There is one extension assembly per instance. Separate instances have entirely separate code, as they have separate schemas.
- Handlers are stateless and shared. Do not store per-user or per-request state in fields on a handler class.
- Changes take effect when the assembly is saved in OneStream; there is no separate deployment step.
- Because the calling assemblies are closed, you can only extend at the points they already call. You cannot add a new extension point yourself.
