# Extension Kit (VS Code)

[← Back to Extending the Framework](index.md)

The **Extension Kit** is a VS Code folder where you design and build a Framework solution on your own machine. You can take a solution from the first requirements conversation through to validated import JSON and compiled extension code without connecting to OneStream.

It brings together three things that are otherwise separate:

- the **Claude skills**, already installed in the folder, so Claude Code knows the Framework's schema and conventions as soon as you open it
- a **design area** (`solutions/`) for the design documents, workbooks and import JSON of each solution
- a **buildable copy of the extension assembly**, `rfa_actnExtension_os`, which compiles against reference copies of the Revfore and OneStream APIs — their method signatures, without the code behind them — so a wrong name, argument or type shows up in the editor rather than in OneStream. You download it, and the Revfore references, from your own OneStream environment with OneStream Developer Studio, so the kit always matches the release you're running

Nothing in the kit runs your code. The Revfore references are stubs generated from each release, where every method throws, and the OneStream references are OneStream's own reference assemblies, which you supply. A clean build therefore proves your code matches the APIs' shapes — names, parameter types and signatures — and nothing more. The logic itself is tested on a OneStream environment after you deploy.

## Requirements

| | |
|---|---|
| **VS Code** | With the **C# Dev Kit** extension, for editing and building the handlers |
| **.NET SDK 10.0** or later | `dotnet build` compiles the extension assembly |
| **OneStream reference assemblies** | `OneStream.Platform.ReferenceAssemblies.<version>.nupkg`, from OneStream. See [Adding the OneStream reference assemblies](#step-2-add-the-onestream-reference-assemblies) |
| **OneStream Developer Studio** | OneStream's desktop app and its `ods` command line, for downloading the Revfore assemblies into the kit and pushing your code back. See [Downloading the assemblies](#step-3-download-the-assemblies-with-developer-studio) |
| **Python 3.9+** | With `jsonschema` and `openpyxl` (`pip install jsonschema openpyxl`), for the validator and the design-workbook scripts |
| **Claude Code** | In the terminal or the VS Code extension. It reads the skills from the kit folder |

## Setting up

### Step 1: Unpack the kit

The kit ships with the product, in the same maintenance unit as the extension assembly and the Claude skills file.

1. In OneStream, go to **Application | Presentation | Workspaces**
2. Select your Revfore Framework workspace — **Revfore Framework (RFA)**, or *name (code)* if you [created your own instance](../appGroups/config/create-new-instance.md)
3. Open **XCP_xRfaDlg_ActnExtension**, the extension assembly's maintenance unit
4. Under **Files**, download **rfaExtensionKit.zip**

Extract it to a folder for your work, one per project or customer, and open that folder in VS Code. The zip has a single `rfaExtensionKit` folder at its root; rename it to suit the project.

!!! note "The kit includes the skills"
    You don't need to download **rfaClaudeSkills.zip** as well. The kit carries the same skills in its `.claude/skills` folder, and Claude Code loads them when you open the folder. **rfaClaudeSkills.zip** is for Claude Desktop and claude.ai, which only accept a zip that contains skill folders.

```
MyProject/
├── CLAUDE.md                  instructions Claude Code reads on startup
├── README.md
├── KIT_VERSION                the Revfore build the kit was generated from
├── RfaExtension.sln
├── Directory.Build.props      your OneStream version - yours to edit
├── nuget.config               points restore at packages/
├── packages/                  put the OneStream package here
├── rfa_actnExtension_os/      your extension code - empty until you download it
├── rfaRef/                    Revfore reference stubs - empty until you download them; never edit
├── samples/GLEntry/           a complete worked solution
├── solutions/                 your design work
│   └── _template/             copy this to start a solution
├── docs/                      the extension-code and setup package reference
└── .claude/skills/            the Framework skills
```

### Step 2: Add the OneStream reference assemblies

The kit compiles against OneStream's official reference assemblies. **Revfore cannot redistribute this package**, so the kit does not include it. Get it from your OneStream channel under your own license.

1. Copy `OneStream.Platform.ReferenceAssemblies.<version>.nupkg` into the kit's `packages/` folder
2. If its version differs from the one in `Directory.Build.props`, edit that file to match:

```xml
<Project>
  <PropertyGroup>
    <OneStreamReferenceAssembliesVersion>9.4.0.50904</OneStreamReferenceAssembliesVersion>
    <OneStreamTargetFramework>net10.0</OneStreamTargetFramework>
  </PropertyGroup>
</Project>
```

`OneStreamTargetFramework` must match the framework folder under `ref/` inside the package. It only changes when OneStream moves to a new .NET version.

!!! tip "Using your own NuGet feed"
    If your organization already hosts the package on an internal feed, add that feed to `nuget.config` instead of copying the file. Keep the `packageSourceMapping` entry and point it at your feed. Machines that use package source mapping will otherwise fail restore with **NU1100**.

### Step 3: Download the assemblies with Developer Studio

The kit ships `rfaRef/` and `rfa_actnExtension_os/` **empty**. Their code comes from your OneStream environment, so it always matches the release you're running. Both assemblies are in the **XCP_xRfaDlg_ActnExtension** maintenance unit of your Revfore Framework workspace:

| Kit folder | Assembly | What it is |
|---|---|---|
| `rfaRef/` | **rfaRef** | The Revfore reference stubs. Download only. Never edit or push it |
| `rfa_actnExtension_os/` | **rfa_actnExtension_os** | The extension assembly — your handlers. Download it once, then push your changes back |

1. Install **OneStream Developer Studio** and sign in to your OneStream server
2. Register the kit folder as a Developer Studio repository, and map `rfaRef/` and `rfa_actnExtension_os/` to the two assemblies above. Developer Studio writes a small mapping file into each folder (`.osconfig`)
3. Pull both assemblies into their folders
4. Start Claude Code in the kit folder and ask it to **hook up the project references**

Step 4 is needed because the project files Developer Studio writes don't build as they are. Claude sets the target framework and adds the OneStream package reference to both, plus a reference from `rfa_actnExtension_os` to `rfaRef`. The kit's `CLAUDE.md` lists exactly what it changes.

!!! tip "Let Claude drive Developer Studio's command line"
    Developer Studio installs a command line, `ods`. Run `ods agent skill init cli --tool claude` in the kit folder to give Claude Code ODS's own skill for it, then restart Claude Code. Claude can then check the setup (`ods status`, `ods validate`) and preview a push or pull on its own. Anything that writes to OneStream or overwrites your files — `push`, `pull`, `load`, `extract` — always waits for your confirmation. Signing in is interactive, so you run that yourself: `! ods connect https://<server>:<port>` in Claude Code's prompt.

!!! warning "A pull overwrites your local files"
    Pulling replaces local files with the server's copy, project files included. Never pull over work that isn't on the server yet, such as new handlers or a dispatcher registration — push it first. `ods preview-pull` shows what a pull would replace without changing anything. After a pull, ask Claude to check the project references again.

### Step 4: Check the baseline

From a terminal in the kit folder:

```
dotnet build RfaExtension.sln
python .claude/skills/revfore-framework/scripts/validate_framework_json.py samples/GLEntry/300-GLEntry-structure.json
```

Both should pass before you change anything. If one fails, fix it first, because every later step assumes this baseline. See [Troubleshooting](#troubleshooting).

## Designing a solution

Start Claude Code in the kit folder:

- **From a terminal:** open a terminal in the kit folder (in VS Code, **Terminal | New Terminal**) and type `claude`.
- **From the VS Code extension:** open the kit folder in VS Code and start Claude Code from the extension.

It loads `CLAUDE.md` and the Framework skills on its own; you don't need to upload or configure anything.

!!! tip "PowerShell says running scripts is disabled"
    If typing `claude` in PowerShell fails with an error about `claude.ps1` and script execution, type `claude.cmd` instead. That starts the same program without going through PowerShell's script policy.

The design follows the same stages described in [AI Model Integrations](../integrations/aiModels/index.md#how-a-solution-gets-built). The difference is that every file lands in your project folder, where you can version, diff and review it.

### Step 1: Describe the requirement

Tell Claude what the solution needs to do in plain English: what gets planned or tracked, by whom, how it is approved, and whether and how it posts to the cube. Include anything you know about existing configuration, such as workflow units, cubes, dimensions and accounts.

You don't have to type it all out. If the requirements already exist as documents, such as an Excel workbook of the current process, a Word requirements document or a Markdown file of notes, copy them into the solution folder, or anywhere in the kit folder, and ask Claude to read them. Claude works from the files and asks about whatever they leave open.

You can also start from a **Revfore Solution Pattern**: a Markdown file that describes a proven design for a common use case. Revfore will publish these on its website. Copy the pattern that fits into the kit folder alongside your own requirements, and ask Claude to use it as the starting point, adapting it to what is different about your solution.

Optionally, you can also give Claude the existing configuration as files:

- **Lists from the admin screens.** Open the screen, click **[File Export](../concepts/metadataDrivenUI/actions.md#file-export-and-import-as-json)**, and copy the file from your **ExportAndImport** folder into the kit. This is especially helpful for **Admin | Security | Users**, **Admin | Cube | Cubes** and **Admin | Cube | Dimension Members**: Claude gets the real names and integration codes rather than asking you for each one, or guessing.
- **A dimension hierarchy, for workflow units.** If the solution's [workflow unit hierarchy](../appGroups/admin/workflow/units/hierarchies.md) should follow an existing dimension hierarchy, such as entities or cost centers, use OneStream's **Application | Tools | Load/Extract** to extract the dimension's metadata as an XML file, and give that to Claude. It builds the units and their hierarchy from the members and parent-child relationships in the file.

You don't need every export before you start. Once the [design workbook](#step-2-review-the-design-and-workbook) exists, you can add these data points to it at any round: paste the exported rows into the workbook's **Users**, **Dimension members** or **Workflow units** sheet, lining the columns up with the sheet's headings and deleting any exported columns the sheet doesn't have, or save the export in the solution folder and ask Claude to bring it into the next round of the workbook.

Claude asks about anything a design needs but you haven't said. Answer those questions rather than letting it guess. A missing prerequisite, such as cube posting or a parent/child review, usually means table changes later.

### Step 2: Review the design and workbook

Claude creates a folder for the solution under `solutions/`, named for the solution alone (`solutions/SpendPlanning/`, no date), starting from `solutions/_template/`, and writes:

| File | What it is |
|---|---|
| `010-<Solution>-design.md` | The design as prose, for reviewers |
| `100-<Solution>-workbook-<yyyymmdd>-r01.xlsx` | The [design workbook](../integrations/aiModels/index.md#the-design-workbook), for you and your stakeholders to edit |
| `200-<Solution>-spec.json` | The design in machine-readable form. Everything else is generated from it |

The numbers at the front keep the folder in working order - see [Solution folder file names](#solution-folder-file-names).

Edit the workbook directly, or share it. Use the **Review Comment** column for questions and requests that aren't a simple cell change.

### Step 3: Iterate

Save the edited workbook back into the solution folder and ask Claude to read it. It reports only what changed, answers each review comment, updates the spec, and writes the next round of the workbook (`-r02`, `-r03`, ...) next to the previous ones. A round on a new day starts again at `-r01` under that day's date.

Expect several rounds. This is the cheapest stage to change your mind in.

### Step 4: Generate the import JSON

When the design has settled, ask Claude to generate the JSON. It doesn't do this before you ask, because JSON generated early goes stale with the next workbook change.

You'll usually get:

| File | Holds | Load with |
|---|---|---|
| `300-<Solution>-structure.json` | Tables, Models, Views and Lookups | [Structure Definitions](import/structure.md) |
| `410-<Solution>-data.json` | Every row the solution needs to run — lookups, reference and configuration rows, workflow setup — in one file | [Data Files](import/data.md) |
| `420-<Solution>-pages.json` | Each page's settings — which grids it shows, their names and edit modes — as rows for the Linked Page Settings view | [Solution Pages](pages.md) |
| `490-<Solution>-data-sample.json` | Optional sample data to seed a demo or prototype. Load it only if you want it | [Data Files](import/data.md) |
| `setup-package.json` | The steps that install the solution: import `300`, sync, import `410`, import `420` | [Setup Packages](setup-packages.md) |

Claude validates each file before handing it over. You can also validate a file yourself at any time:

```
python .claude/skills/revfore-framework/scripts/validate_framework_json.py solutions/<folder>/<file>.json
```

The validator checks the schema and the semantic rules the schema can't express: lookup pairings, model source wiring, sequence numbers, and whether data rows target real view columns. Those mistakes produce valid-looking JSON that fails, or silently misbehaves, at import.

!!! note "Integration codes from your instance"
    A data file that refers to records already in OneStream, such as users, cubes, dimension members, workflow units or cycles, needs their real integration codes. Claude asks for any it can't derive. The quickest way to supply them is a **File Export** from the relevant admin screen, copied into the kit, as described in [Step 1](#step-1-describe-the-requirement). Otherwise read them off the screen. Never let Claude guess one, because a guessed code can load against the wrong record.

### Step 5: Build the setup package

A solution installs as **one zip**. `setup-package.json` lists the steps, and the builder turns it into `<SolutionCode>SetupPackage.zip` (for example `PrjSetupPackage.zip`):

```
python .claude/skills/revfore-framework/scripts/build_setup_package.py solutions/<folder>/setup-package.json
```

It validates every file the package imports, refuses a data import that has no sync before it, and stamps the version. Raise `Revision` in `setup-package.json` for each release. Never zip the files by hand. See [Setup Packages](setup-packages.md).

### Step 6: Install it in OneStream

Load the zip: **Load** on **Admin | Relational | Tables** takes a `.zip` as well as a `.json`. It runs the whole package — import, sync, data, pages — in one go, and adds it to the workspace's `rfaExtensionSetupPackage.zip`, beside any other solution's, so **Config | Setup & Upgrade** runs it from then on. The same load installs it for a customer and upgrades it with a new release; while you build, you can reload the current release as often as you need. See [Installing it](setup-packages.md#installing-it).

Either way, nobody loads files or presses Sync by hand. You can still load a single file the manual way — structure (`300`), sync tables, sync views, then data (`4xx`) in file-name order, as in [Loading a file](import/index.md#loading-a-file) — which is useful for trying one change on its own. Sample data (`490`) is never in the package, so it is always loaded that way.

To change a solution that already exists in an instance, **extract it first** with **Load/Extract | Extract | Both** on **Admin | Relational | Tables**, and save the file in the solution folder. Claude then works from what's actually in the instance, not from a description of it. See [Extracting a solution](import/index.md#extracting-a-solution).

## Solution folder file names

Everything Claude writes into a solution folder is named so that **sorting by name puts it in working order**: notes first, then the design rounds, then the files to load, in the order to load them.

`{NNN}-{Solution}-{kind}[-{detail}]`

| Prefix | Group | Example |
|---|---|---|
| `000`-`099` | Overview and notes | `000-SpendPlanning-README.md`, `010-SpendPlanning-design.md` |
| `100`-`199` | Design workbook rounds | `100-SpendPlanning-workbook-20261005-r01.xlsx` |
| `200`-`299` | Spec | `200-SpendPlanning-spec.json` |
| `300`-`399` | Structure files to load | `300-SpendPlanning-structure.json` |
| `400`-`499` | Data files to load — one, plus page settings and optional sample data | `410-SpendPlanning-data.json`, `420-SpendPlanning-pages.json`, `490-SpendPlanning-data-sample.json` |
| `500`-`599` | Hand-written SQL — `500` before the structure, `510` after the sync, `590` for uninstall | `500-SpendPlanning-view-SpdPlanTall.sql` |
| `600`-`699` | Notes on the handler code | `600-SpendPlanning-handlers.md` |

Two files sit outside the numbering: `setup-package.json`, the [setup package](setup-packages.md) source, and the `<SolutionCode>SetupPackage.zip` the builder writes next to it.

- **The solution folder has no date** - `solutions/SpendPlanning/`. The workbook files carry the dates instead.
- **Every workbook round has a two-digit round number**, the first one included (`-r01`), so rounds sort correctly - `r10` after `r09`, and the first round before the second.
- **Numbers step by 10 within a group**, leaving room to add a file later without renaming the others.
- **One data file to start with.** Every row the solution needs to run goes in `410-<Solution>-data.json`, ordered so that rows are created before anything that refers to them. A larger solution can split structure or data by area (`310-`, `430-`), since the setup package loads them all in one run. Optional sample data for a demo or prototype goes in `490-<Solution>-data-sample.json`, outside the package.
- **The full solution name is in every file name**, because files are loaded into OneStream and shared on their own, away from the folder.
- **No spaces**, so any file can be named on a command line without quotes.
- Scripts Claude writes to produce large files go in a `scripts/` subfolder, and superseded drafts in `archive/`.

Solutions designed before this convention keep their existing names - nothing is renamed.

## Writing the extension code

Once the structure is settled, ask Claude to write the handlers, or write them yourself. The kit's `rfa_actnExtension_os` folder mirrors the assembly in OneStream, and the kit's **`docs/` folder is the reference for it** — start at `docs/README.md`. It covers how the framework calls your code, adding and registering a handler, working with records, custom actions and the shared helpers. The sample in `samples/GLEntry/` shows all of it working together.

### The inner loop

1. Edit a handler under `DashboardExtenders/ExtensionHandlers/`
2. Run `dotnet build RfaExtension.sln`, or build from VS Code
3. Fix what the compiler reports: misspelled members, wrong parameter types, missing hooks

The build compiles against **reference stubs**, not the real Revfore engine. `rfaRef` has the real Revfore signatures, but every method throws; OneStream's own types come from the reference assemblies package. A clean build proves the code will compile in OneStream. It doesn't prove the logic is right.

### Deploying to OneStream

The kit doesn't publish anything itself. There are two ways to get your changes into OneStream:

- **With OneStream Developer Studio** — the usual way, since the kit folder is already mapped to the assembly from [Step 3](#step-3-download-the-assemblies-with-developer-studio). See below.
- **Manually**, by copying the files into the workspace — the steps under [Deploying manually](#deploying-manually).

Either way, deploy the same files to the same place: the **rfa_actnExtension_os** assembly in the **XCP_xRfaDlg_ActnExtension** maintenance unit, plus any **XCP_x** units of your own.

#### Deploying with Developer Studio

1. Build: `dotnet build RfaExtension.sln` must be clean
2. If you added a handler, check it is registered in `ExtensionHandlerDispatcher.cs`. An unregistered handler deploys without error but never runs
3. Preview the push of `rfa_actnExtension_os` (`ods preview-push rfa_actnExtension_os`) and check what will change
4. Push it (`ods push rfa_actnExtension_os`)

Push `rfa_actnExtension_os` on its own, never the whole kit folder, and never push `rfaRef`. If Claude has the ODS skill, it previews the push and shows you the changes. It pushes only after you confirm.

#### Deploying manually

1. In OneStream, go to **Application | Presentation | Workspaces** and select your Revfore Framework workspace (**Revfore Framework (RFA)**, or *name (code)* if you created your own instance)
2. Open **XCP_xRfaDlg_ActnExtension** and go to **Assemblies | rfa_actnExtension_os**
3. Add or update the files you changed, using the same folder structure as the kit. This includes `ExtensionHandlerDispatcher.cs` if you registered a new handler
4. Save. The change takes effect immediately

Then test on a real environment. That is where behavior, as opposed to shape, gets proven.

!!! warning "Keep one source of truth"
    If someone edits a handler directly in OneStream, copy the change back into the kit before your next deploy, or your next deploy overwrites it. This matters even more when deploys are pushed with Developer Studio, since a push replaces what is in the workspace without anyone looking at it first. Keeping the kit folder in source control makes that drift easy to see.

## Kit updates

Each Revfore release comes with a new kit. The skills, samples and documentation are regenerated to match that release. The reference stubs come with the release itself: after the upgrade, pull **rfaRef** again with Developer Studio and rebuild.

- **`solutions/` is never overwritten.** Your design work is safe across updates.
- **`rfa_actnExtension_os/` holds your code.** Keep your handlers and your dispatcher registrations. Revfore's own files, such as `IExtensionHandler.cs`, `SolutionHelper.cs` and `SharedMethods.cs`, are updated in OneStream by the upgrade. Preview a pull of `rfa_actnExtension_os` to see what changed, and merge before you push again.
- **`Directory.Build.props` and `packages/` are yours.** Keep them unless you're also moving to a new OneStream version.
- **Developer Studio's setup stays with the folder.** Keep `rfaRef/`, `rfa_actnExtension_os/` and their `.osconfig` mapping files, and any skills of your own, such as ODS's `ods-cli`.

To take a new kit, download **rfaExtensionKit.zip** from the maintenance unit again, as in [Step 1](#step-1-unpack-the-kit). Then copy your `solutions/`, `packages/`, `Directory.Build.props`, `rfaRef/` and `rfa_actnExtension_os/` folders across from your old kit folder. Register the new folder with Developer Studio in place of the old one.

The skills describe the schema and conventions of the release they shipped with, so update the kit whenever you upgrade the Framework in OneStream. Otherwise Claude generates against an older schema.

## Troubleshooting

| Symptom | Cause | Fix |
|---|---|---|
| **NU1101** Unable to find package OneStream.Platform.ReferenceAssemblies | The package isn't in `packages/`, or its version doesn't match `Directory.Build.props` | Copy the `.nupkg` in, or correct the version |
| **NU1100** Unable to resolve ... | Package source mapping on your machine blocks the source | Keep the `packageSourceMapping` entry in `nuget.config`, pointed at whichever source holds the package |
| **CS0117** '*Type*' does not contain a definition for '*Member*' | Your code uses an API that is newer than the kit's stubs | Update to the kit for your Framework release. If you already have it, send the error and your `KIT_VERSION` line to Revfore |
| A type named in the documentation is missing entirely | Same as above: the kit is behind your release | As above |
| The build reports a missing project, or finds no Revfore types | `rfaRef/` or `rfa_actnExtension_os/` hasn't been downloaded yet, or the project references aren't hooked up | Download both with Developer Studio ([Step 3](#step-3-download-the-assemblies-with-developer-studio)), then ask Claude to hook up the project references |
| The build broke straight after a pull | The pull replaced the project files, removing the references | Ask Claude to hook up the project references again |
| Duplicate OneStream types, or a restore error naming `OneStream.ReferenceAssemblies` | A second OneStream package was added to a project, for example by following Developer Studio's own reference-assembly steps | Remove it. The kit already references `OneStream.Platform.ReferenceAssemblies` through `Directory.Build.props` and `packages/` |
| A handler you pushed never runs | It isn't registered in `ExtensionHandlerDispatcher.cs` | Register it, build, and push again |
| The setup package builder or Setup refuses the package | A placeholder left from the template, a missing sync, the version, or a hand-edited zip | See [Setup Packages troubleshooting](setup-packages.md#troubleshooting) |
| The validator reports errors on JSON Claude wrote | The file was edited by hand, or the kit's skills are older than your release | Ask Claude to fix the reported rules, then validate again |

## Notes

- Keep one kit folder per project or customer. Each has its own `solutions/` and its own copy of the extension assembly, as each OneStream instance does.
- Put the kit folder under source control, but exclude `packages/`, `bin/` and `obj/`. The OneStream package must not be committed to a shared or public repository.
- `samples/GLEntry` is a complete worked example, laid out with the [solution folder file names](#solution-folder-file-names): validated import JSON, a setup package, sample data, and the `JrlEntryHandler` that goes with it. Read it before writing your first handler.
