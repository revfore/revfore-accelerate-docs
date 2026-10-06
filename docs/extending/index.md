# Extending the Framework

Revfore Framework is designed so that most of a solution is **configured**, not coded. Tables, models, views, actions and security are all defined as metadata, and the framework builds the screens from that.

Some behaviour cannot be expressed as configuration — a validation rule that depends on other records, a calculation that runs when a row is saved, a button that pushes data to the cube. That is what the **extension assembly** is for.

## Overview

Use this section to:

- understand which parts of the framework you can change and which you cannot
- decide whether a requirement needs configuration or code
- find out what extension code can do, and where the details live

To design and build a solution on your own machine, with Claude and a compiler checking the work before anything reaches OneStream, see [Extension Kit (VS Code)](extension-kit.md).

## What you can and cannot change

Revfore Framework ships as a set of OneStream workspace assemblies. All but one are closed product code - the one that is yours is where the framework calls your code, and you can add assemblies of your own alongside it.

| Assembly | Yours to edit? | What it is |
|---|---|---|
| **`rfa_actnExtension_os`** | **Yes** | The extension assembly — where the framework calls your code. Every handler's entry point lives here. |
| `rfa_os` | No | Core engine — builds screens from metadata and calls into your extension code. |
| `rfa_shared_os` | No | Shared services used across the framework. |
| `rfa_wf_os` | No | Workflow engine, cube synchronise and clear. |
| `rfa_actnFile_os` | No | File import and export actions. |
| `rfa_actnOther_os` | No | Additional built-in actions. |
| `rfa_conSubItmCnfg_os`, `rfa_expressionEditor_os`, `rfa_solCnfg_os` | No | Configuration and editor support. |

This matters more than it might appear. Because the closed assemblies call *into* your extension assembly at fixed points, upgrading the framework does not overwrite your code — and your code cannot destabilise the engine. You extend at defined seams rather than by modifying the product.

### Adding your own assemblies

`rfa_actnExtension_os` is not the only place your code can live. You can add further assemblies of your own:

- in the extension assembly's maintenance unit, **XCP_xRfaDlg_ActnExtension**, or
- in new maintenance units whose names start with **XCP_x**.

The framework calls only into `rfa_actnExtension_os` — that is where its extension points are — so a handler there is always the entry point, and it calls the code in your other assemblies. That is a good home for logic that is shared across several handlers, large enough to deserve its own assembly, or owned by a different team.

!!!Note Two levels of extension
    Partners building a solution on the framework and customers extending that partner's solution both hook in at the same place. There is one extension assembly per instance, so plan with your partner how its handlers are shared — each party can keep the bulk of its own code in a separate **XCP_x** maintenance unit and leave only the entry points in `rfa_actnExtension_os`.

## Configuration first

Before writing code, check whether configuration already covers it. Code is harder to test, harder to upgrade around, and invisible to the administrators maintaining the solution.

Configuration usually handles:

- **Field-level rules** — required, read-only, default values, formats, and lookups are all view column settings
- **Row-level security** — join to the flattened security views rather than filtering in code
- **Derived display values** — expression columns compute values in the model
- **Navigation** — actions can open another view without any code

Reach for code when the requirement involves something configuration genuinely cannot express: cross-record validation, writing to other tables, calling an external service, or driving a cube operation.

## What code can do

Code runs at fixed **extension points** — when records are saved, copied, deleted or bulk updated, and when a custom action is clicked — and is organised by model family, so a view with no code behaves exactly as configured. See [Writing Extension Code](writing-code.md) for what each point is for. The full API reference ships in the [Extension Kit](extension-kit.md).

## Where the code lives

Inside OneStream:

1. Go to **Application | Presentation | Workspaces**
2. Select your Revfore Framework workspace — **Revfore Framework (RFA)**, or *name (code)* if you [created your own instance](../appGroups/config/create-new-instance.md)
3. Open **XCP_xRfaDlg_ActnExtension**, the extension assembly's maintenance unit
4. The files are under **'Assemblies | rfa_actnExtension_os`**

Add your code there, or in assemblies of your own as described above. The [Extension Kit](extension-kit.md) has the same assembly as a buildable project, with the reference for every file in it.

## Notes

- There is one extension assembly per instance. Separate instances have entirely separate code, as they have separate schemas.
- Handlers are stateless and shared. Do not store per-user or per-request state in fields on a handler class.
- Changes take effect when the assembly is saved in OneStream; there is no separate deployment step.
- Because the calling assemblies are closed, you can only extend at the points they already call. You cannot add a new extension point yourself.
