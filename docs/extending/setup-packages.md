# Setup Packages

[← Back to Extending the Framework](index.md)

A **setup package** is how a solution installs and upgrades itself. Everything the solution needs — its structure, its data, its page settings and any SQL of its own — goes into **one zip**, `rfaExtensionSetupPackage.zip`, uploaded to the workspace. An administrator presses **Setup and Upgrade** and the package's steps run in order.

Nobody loads files or presses Sync by hand. That replaces the manual sequence in [Loading a file](import/index.md#loading-a-file) (load the structure, sync the tables, sync the views, load the data) with one upload, and it is what makes an upgrade repeatable: the same zip installs a new environment and upgrades an existing one.

The Framework uses the same mechanism for its own database scripts.

## Where the package lives

| | |
|---|---|
| **File** | `rfaExtensionSetupPackage.zip` |
| **Where** | The **Files** of the **XCP_xRfaDlg_ActnExtension** maintenance unit, in your Revfore Framework workspace |
| **Run by** | **Config \| Setup & Upgrade**, after the Framework's own setup. See [Setup & Upgrade](../appGroups/config/setup-upgrade.md) |

The workspace ships with an **empty placeholder** of that zip, which Setup skips. A solution replaces it with its own build rather than adding a new file.

## Starting a solution

In the [Extension Kit](extension-kit.md), `solutions/_template/` holds the starting point:

| File | What it is |
|---|---|
| `setup-package.json` | The steps: import the structure, sync it, import the data, import the pages |
| `420-Solution-pages.json` | Each page's setup — which grids it shows — as data. It holds a placeholder page until you write yours |

Copy the folder to `solutions/<Solution>/`, replace `{Solution}` in the file names and inside `setup-package.json`, and set `SolutionCode`. The builder refuses the package while a placeholder is left. `samples/GLEntry/` in the kit is a complete example in the same shape.

When Claude designs a solution in the kit, it writes these files for you.

## The package source

`setup-package.json` sits next to the files it lists, in the solution folder. The default, for most solutions, is four steps:

```json
{
  "SolutionCode": "Prj",
  "Revision": 1,
  "Steps": [
    { "Type": "Import",        "File": "300-Prj-structure.json" },
    { "Type": "SyncStructure" },
    { "Type": "Import",        "File": "410-Prj-data.json" },
    { "Type": "Import",        "File": "420-Prj-pages.json", "When": "Install" }
  ]
}
```

| Property | |
|---|---|
| `SolutionCode` | The solution's own code. Never `Rfa` |
| `Revision` | Your release number. Raise it with every release — see [Versions](#versions) |
| `Steps` | What Setup runs, in order |
| `Uninstall` | Optional. See [Uninstall](#uninstall) |

### Step types

| Step | What it does |
|---|---|
| `Import` | Loads a Framework JSON file, structure or data — the same files as [Load & Extract](import/index.md). A data import also processes the hierarchies its rows touch |
| `SyncStructure` | Creates or alters the physical tables, then the SQL views, for every table and view the `Import` steps before it named. It is the **Sync** action, done for you |
| `Sql` | Runs a SQL script, split on `GO`, with the schema tokens replaced |
| `ProcessHierarchies` | Processes one hierarchy set, named by `SetName`, when nothing in the package's data touches it |

The usual order is:

1. SQL that must run before the structure changes (`500-`) — a rename, or a data fix ahead of a column change
2. The structure file (`300-`)
3. `SyncStructure`
4. SQL that needs the new columns (`510-`), such as a stored procedure
5. The data file (`410-`)
6. The pages (`420-`), on install only

Add steps beyond the default four only when there is a reason for them.

!!! warning "SyncStructure between structure and data"
    A structure import only defines tables and views as metadata; `SyncStructure` is what creates them. A data import into a view that has not been synced fails, so a `SyncStructure` must come between the last structure file and the first data file that needs it. The builder refuses a package that gets this wrong.

    `SyncStructure` syncs only the tables and views named by the `Import` steps before it, since the previous `SyncStructure`.

### More than one file

Use as many structure and data files as reads best — the package loads them all in one run. Split by area (`310-Prj-structure-reporting.json`, `430-Prj-data-workflow.json`), list each as its own `Import` step in load order, and keep a `SyncStructure` after the structure files and before the data that needs them.

A file is also what `When` and `SinceVersion` apply to, so a release's new rows or its migration can sit in a file of its own.

**Sample data (`490-`) never goes in the package.** It is a demo load the customer chooses.

## When a step runs

| `When` / `SinceVersion` | Runs |
|---|---|
| neither, or `"When": "Always"` | Every run — a new install, an upgrade, and Setup pressed again |
| `"When": "Install"` | Only when the package has never been applied |
| `"SinceVersion": "PV912-SV200-R003"` | Upgrades only: when the installed version is below it and the package's is at or above it. Skipped on a new install |

A step cannot have both.

Because a new install skips every `SinceVersion` step, the files it does run must already produce the current state on their own. A migration step is for bringing an **existing** install forward, and must never be the only place a change is made.

!!! note "Every step must be safe to run again"
    Steps without `When` run on every Setup press. Imports match rows by integration code, and `SyncStructure` only changes what differs. SQL is yours to write that way — `if not exists`, `merge`, `create or alter`.

    A data file entry's `Mode` decides what a re-run does to existing rows: leave it unset for rows the file owns, `"Insert"` for starter rows the customer may edit afterwards, and `"Update"` to change a few columns on rows that already exist. See [Update only, or insert only](import/data.md#update-only-or-insert-only).

A column, source, relationship, action, filter or security level that a release drops needs no migration script: keep its entry in the structure file marked `"Remove": true`. See [Removing columns and other items](import/structure.md#removing-columns-and-other-items).

## Pages

What each page shows goes in `420-<Solution>-pages.json`, as rows for the Framework's Linked Page Settings view. Claude writes the page settings from the design workbook's Pages sheet, with a list of the navigation group, page name and layout for each page. You create the pages by hand in OneStream from that list. The settings take effect once a page with matching names exists, and a page configured by hand in OneStream can be captured the same way with **Extract**. See [Solution Pages](pages.md).

The pages step is `"When": "Install"`. An import updates the rows it matches, so without that every Setup press would overwrite the customer's own later changes to a page.

### Pages in later releases

Because `420` runs on install only, a changed page does not reach existing installs on its own. For each release that adds or changes pages, put those page rows in a new file as well, as a step for that release:

```json
{ "Type": "Import", "File": "422-Prj-pages-R020.json", "SinceVersion": "PV912-SV200-R020" }
```

- **Keep `420` up to date** — it holds every page in its current form, for new installs.
- **One file per release**, numbered 421, 422, … in release order, holding only that release's new and changed pages.
- **Never rename or retag an earlier release's file.** A page that changes again goes into the newer file too. An install several releases behind runs each file in turn and ends on the newest.
- A release file overwrites the pages it lists, including any customer changes to them. Pages it doesn't list are never touched.

!!! warning "A layout change needs a manual re-link"
    The layout a page uses — one content sub item, two stacked, two side by side — is the OneStream dashboard it is linked to, and the package cannot change it. When a release moves a page to a different layout, the release notes must tell the administrator to re-link it after Setup: select the page, click **Link an Existing Content Item from a Shared Workspace**, and pick the new layout's dashboard.

## Versions

A package version looks like `PV912-SV200-R003`: the Framework build it was built against, then your revision. Every part is exactly three digits.

- **Raise `Revision` with every release**, and give that release's migration steps a `SinceVersion` equal to the new version. A step whose `SinceVersion` is above the package's version would never run, so the builder refuses it.
- **Never type the core part.** The builder stamps it from the kit's `KIT_VERSION`.
- A package built against a **newer** Framework than the one installed is refused — upgrade the Framework first. One built against an older Framework runs, with a note.
- A package **older** than the one already applied is refused.

The version applied is recorded only after every step has succeeded.

## Building

From the kit folder:

```
python .claude/skills/revfore-framework/scripts/build_setup_package.py solutions/Prj/setup-package.json
```

The builder checks the manifest, [validates](extension-kit.md#step-4-generate-the-import-json) every `Import` file, refuses a data import that follows an unsynced structure import, stamps the version and a checksum of every file, and writes `rfaExtensionSetupPackage.zip` next to the manifest.

```
python .claude/skills/revfore-framework/scripts/build_setup_package.py solutions/Prj/setup-package.json --check
```

`--check` compares an existing zip with its source and fails if any file, step or the version has changed since it was built. Run it before you hand a zip to anyone.

!!! warning "Never zip a package by hand"
    The checksum of every file is checked when the package runs, and a file that doesn't match is refused. Rebuild with the builder; don't edit the zip.

On Windows, use `py` in place of `python`.

## Installing it

### With Setup

1. In OneStream, go to **Application | Presentation | Workspaces** and select your Revfore Framework workspace
2. Open **XCP_xRfaDlg_ActnExtension** and, under **Files**, replace `rfaExtensionSetupPackage.zip` with your build
3. Go to **Config | Setup & Upgrade** and run Setup

Setup runs the Framework's own package first, then your solution's custom setup code and package, then relinks OneStream records and rebuilds the SQL views for every relational view. Each stage runs only if the one before it succeeded. A failure names the step and file — and, for SQL, the batch — and leaves Setup unfinished, so it runs again from the start next time.

This is how customers install and upgrade a solution.

### By loading it while you build

An administrator can also run a package without going through Setup: the **Load** action on **Admin | Relational | Tables** (see [Loading a file](import/index.md#loading-a-file)) takes the `.zip` as well as a `.json`. Loading it:

1. replaces the workspace's `rfaExtensionSetupPackage.zip` with the one you picked, so the next Setup runs the same package
2. runs the package's steps
3. relinks OneStream records and rebuilds the SQL views the change affected

A load picks steps a little differently from Setup, so you can reload the release you are working on as often as you need to:

| Step | Setup pressed again | Load, same version again |
|---|---|---|
| no `When`, or `"Always"` | runs | runs |
| `"When": "Install"` | skipped | **runs** |
| `SinceVersion` equal to the package's version | skipped | **runs** |
| `SinceVersion` of an earlier version | skipped | skipped |

The first load of a package, or of a new version, runs exactly what Setup would. So while you build a release, leave `Revision` at that release and tag its migrations with it: every load reruns them, and at release the tags are already right for your customers' upgrades. Write migrations to cope with a database that is already migrated.

- A zip that is not a valid package, or is refused for its version, is not kept: the previous file is put back and nothing runs. Once a step has run, the new zip stays even if a later step fails — fix the cause and load it again.
- Custom setup code does not run on a load. Press Setup for that.
- A load reapplies the `Install` steps, so in a customer's environment it overwrites their changes to the shipped pages. **Use loading in your development environment; customers use Setup.**

## Uninstall

A **Full** uninstall (see [Uninstall](../appGroups/config/uninstall.md)) drops **everything in the extension schema** — every table, view, stored procedure and function, including the SQL views built for your relational views. For most solutions that is everything, and the package needs no `Uninstall` section.

Add one only for objects your scripts created **outside** the extension schema:

```json
  "Uninstall": [
    { "Type": "Sql", "File": "590-Prj-drop.sql" }
  ]
```

It runs before the Framework's own drop, every script in order — `Sql` steps only, with no `When` or `SinceVersion`. Setup never reads it. Write it to work against any installed version: `if exists`, dependents first.

Both the solution's package and the Framework's are loaded and checked before anything is dropped. A failure names the step, file and batch, and stops the uninstall.

## Who can run one

A package can run any SQL it holds, so only a **OneStream administrator who is not [impersonating](../security/impersonation.md)** can run, load or uninstall one. The Framework enforces this where the SQL runs, not only on the buttons, so custom code that calls the package as anyone else is refused too.

## Custom setup code

Most solutions need nothing beyond the package. For the rare step that has to be code, `SolutionHelper.Setup_RunSolutionSetupScriptsAndProcesses` in `rfa_actnExtension_os` runs the package and ships with that call already in it. Code that must run before the package goes above the call, and code that must run after goes below it. **Never remove the call** — without it Setup no longer installs the solution. See the kit's `docs/shared-methods.md`.

## Troubleshooting

| Symptom | Cause | Fix |
|---|---|---|
| The builder refuses the package naming `{Solution}` or a placeholder | The template wasn't fully renamed | Replace every `{Solution}`, and set `SolutionCode` |
| The builder refuses a data import | No `SyncStructure` between it and the structure import before it | Add a `SyncStructure` step |
| Setup refuses the package for its version | It was built against a newer Framework than the one installed, or it is older than the package already applied | Upgrade the Framework first, or build from the kit for the installed release; raise `Revision` |
| Setup refuses a file's checksum | The zip was edited or assembled by hand | Rebuild it with the builder |
| A changed page didn't reach an existing install | `420` runs on install only | Add the page to a `42x-…-pages-R0NN.json` file as a `SinceVersion` step |
| A migration step didn't run on a new install | `SinceVersion` steps are for upgrades only | Make the change in the install-path files too |

Setup and loading write a line per step to the OneStream error log, with each file's checksum prefix — which is how you tell which build of a zip was actually uploaded. Turn on [Debug logging](../appGroups/admin/security/Users/index.md#debug-logging) for the administrator running it to see them.
