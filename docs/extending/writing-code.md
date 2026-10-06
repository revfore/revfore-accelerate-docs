# Writing Extension Code

[← Back to Extending the Framework](index.md)

Most of a solution is configuration. Code is for the rules configuration cannot express, and it plugs into the framework at fixed **extension points** that the framework calls as users work.

## What extension code can do

| When | What your code can do |
|---|---|
| A record is **saved** | Validate across records, default or correct values before they are written, or stop the save with a message. Afterwards, update related tables or start a calculation. |
| Records are **copied** | Decide which records may be copied, then fix up the copies — for example, copying their child records too. |
| Records are **deleted** | Decide what may be deleted, and remove dependent records yourself so the delete can go ahead. |
| Records are **bulk updated** | Decide which of the selected records may be changed, then cascade or recalculate. |
| A **custom action** is clicked | Run the action: open another view, call a service, post to the cube, move a workflow along. |
| A **selection** changes | React to what the user picked. |

Code can also read the user's **workflow context** — the unit, instance, cube and dimension members they are working in — which is how cube posting lines up with what the user sees.

Code is organised by **model family**: each family of related views has its own handler, so solutions do not interfere with one another, and a view with no handler simply behaves as configured.

## Where the details are

The API reference — the extension points and what they receive, working with records, actions and the workflow context, Revfore's shared helpers — ships in the **[Extension Kit](extension-kit.md)**, in its `docs/` folder, alongside a complete worked example and the Claude skill that writes this code. Keeping it with the kit means it always describes the same release as the kit's reference stubs.

To get the kit, see [Extension Kit (VS Code)](extension-kit.md).
