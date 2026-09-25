# Extension Kit (VS Code)

[← Back to Extending the Framework](index.md)

The **Extension Kit** is a VS Code folder where you design and build a Framework solution on your own machine. You can take a solution from the first requirements conversation through to validated import JSON and compiled extension code without connecting to OneStream.

It brings together three things that are otherwise separate:

- the **Claude skills**, already installed in the folder, so Claude Code knows the Framework's schema and conventions as soon as you open it
- a **design area** (`solutions/`) for the design documents, workbooks and import JSON of each solution
- a **buildable copy of the extension assembly**, `rfa_actnExtension_os`, which compiles against the real Revfore and OneStream APIs so mistakes show up in the editor rather than in OneStream

Nothing in the kit runs your code. A clean build proves your code matches the real APIs: names, parameter types and signatures. The logic itself is tested on a OneStream environment after you deploy.

## Requirements

| | |
|---|---|
| **VS Code** | With the **C# Dev Kit** extension, for editing and building the handlers |
| **.NET SDK 10.0** or later | `dotnet build` compiles the extension assembly |
| **OneStream reference assemblies** | `OneStream.Platform.ReferenceAssemblies.<version>.nupkg`, from OneStream. See [Adding the OneStream reference assemblies](#step-2-add-the-onestream-reference-assemblies) |
| **Python 3.9+** | With `jsonschema` and `openpyxl` (`pip install jsonschema openpyxl`), for the validator and the design-workbook scripts |
| **Claude Code** | In the terminal or the VS Code extension. It reads the skills from the kit folder |

## Setting up

### Step 1: Unpack the kit

Revfore supplies the kit as `rfaExtensionKit-<version>.zip`. Extract it to a folder for your work, one per project or customer, and open that folder in VS Code.

```
MyProject/
├── CLAUDE.md                  instructions Claude Code reads on startup
├── README.md
├── KIT_VERSION                the Revfore build the kit was generated from
├── RfaExtension.sln
├── Directory.Build.props      your OneStream version - yours to edit
├── nuget.config               points restore at packages/
├── packages/                  put the OneStream package here
├── rfa_actnExtension_os/      your extension code
├── rfaRef/  rfaOsRef/         Revfore reference stubs - never edit
├── samples/GLEntry/           a complete worked solution
├── solutions/                 your design work
└── .claude/skills/            the Framework skills
```

### Step 2: Add the OneStream reference assemblies

The kit compiles against OneStream's official reference assemblies. **Revfore cannot redistribute this package**, so the kit does not include it. Get it from your OneStream channel under your own licence.

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
    If your organisation already hosts the package on an internal feed, add that feed to `nuget.config` instead of copying the file. Keep the `packageSourceMapping` entry and point it at your feed. Machines that use package source mapping will otherwise fail restore with **NU1100**.

### Step 3: Check the baseline

From a terminal in the kit folder:

```
dotnet build RfaExtension.sln
python .claude/skills/revfore-framework/scripts/validate_framework_json.py samples/GLEntry/glentry-solution.json
```

Both should pass before you change anything. If one fails, fix it first, because every later step assumes this baseline. See [Troubleshooting](#troubleshooting).

## Designing a solution

Start Claude Code in the kit folder. It loads `CLAUDE.md` and the Framework skills on its own; you don't need to upload or configure anything.

The design follows the same stages described in [AI Model Integrations](../integrations/aiModels/index.md#how-a-solution-gets-built). The difference is that every file lands in your project folder, where you can version, diff and review it.

### Step 1: Describe the requirement

Tell Claude what the solution needs to do in plain English: what gets planned or tracked, by whom, how it is approved, and whether and how it posts to the cube. Include anything you know about existing configuration, such as workflow units, cubes, dimensions and accounts.

Claude asks about anything a design needs but you haven't said. Answer those questions rather than letting it guess. A missing prerequisite, such as cube posting or a parent/child review, usually means table changes later.

### Step 2: Review the design and workbook

Claude creates a folder for the solution under `solutions/` and writes:

| File | What it is |
|---|---|
| `<solution>-spec.json` | The design in machine-readable form. Everything else is generated from it |
| `<solution>-design.md` | The same design as prose, for reviewers |
| `<Solution>-<yyyymmdd>.xlsx` | The [design workbook](../integrations/aiModels/index.md#the-design-workbook), for you and your stakeholders to edit |

Edit the workbook directly, or share it. Use the **Review Comment** column for questions and requests that aren't a simple cell change.

### Step 3: Iterate

Save the edited workbook back into the solution folder and ask Claude to read it. It reports only what changed, answers each review comment, updates the spec, and writes the next round of the workbook (`-r2`, `-r3`, ...) next to the previous ones.

Expect several rounds. This is the cheapest stage to change your mind in.

### Step 4: Generate the import JSON

When the design has settled, ask Claude to generate the JSON. It doesn't do this before you ask, because JSON generated early goes stale with the next workbook change.

You'll usually get:

| File | Holds | Load with |
|---|---|---|
| `<solution>-solution.json` | Tables, Models, Views and Lookups | [Structure Definitions](import/structure.md) |
| `<solution>-data-setup.json` | Configuration and reference rows the solution needs, such as workflow areas, item types and rates | [Data Files](import/data.md) |
| `<solution>-data-sample.json` | Optional sample transactions for testing | [Data Files](import/data.md) |

Claude validates each file before handing it over. You can also validate a file yourself at any time:

```
python .claude/skills/revfore-framework/scripts/validate_framework_json.py solutions/<folder>/<file>.json
```

The validator checks the schema and the semantic rules the schema can't express: lookup pairings, model source wiring, sequence numbers, and whether data rows target real view columns. Those mistakes produce valid-looking JSON that fails, or silently misbehaves, at import.

!!! note "Integration codes from your instance"
    A data file that refers to records already in OneStream, such as users, cubes, dimension members, workflow units or instances, needs their real integration codes. Claude asks for any it can't derive. Read them off the relevant admin screen, because a guessed code can load against the wrong record.

### Step 5: Load it into OneStream

Load and sync the files in the order described in [Loading a file](import/index.md#loading-a-file): structure file, sync tables, sync views, then the data files.

To change a solution that already exists in an instance, **extract it first** with **Load/Extract | Extract | Both** on **Admin | Relational | Tables**, and save the file in the solution folder. Claude then works from what's actually in the instance, not from a description of it. See [Extracting a solution](import/index.md#extracting-a-solution).

## Writing the extension code

Once the structure is settled, ask Claude to write the handlers, or write them yourself. The kit's `rfa_actnExtension_os` folder mirrors the assembly in OneStream:

```
rfa_actnExtension_os/
└── DashboardExtenders/
    ├── IExtensionHandler.cs
    ├── ExtensionHandlerDispatcher.cs
    ├── SolutionHelper.cs
    ├── SharedMethods.cs
    └── ExtensionHandlers/
        ├── JrlEntryHandlers/          the sample handler
        └── <Prefix>Handlers/          yours
```

Handlers are added and registered exactly as described in [Adding a Handler](handlers/adding.md): one class per model family implementing `IExtensionHandler`, plus one branch in `ExtensionHandlerDispatcher.cs`.

### The inner loop

1. Edit a handler under `DashboardExtenders/ExtensionHandlers/`
2. Run `dotnet build RfaExtension.sln`, or build from VS Code
3. Fix what the compiler reports: misspelled members, wrong parameter types, missing hooks

The build compiles against **reference stubs**, not the real Revfore engine. `rfaRef` and `rfaOsRef` have the real Revfore signatures, but every method throws; OneStream's own types come from the reference assemblies package. A clean build proves the code will compile in OneStream. It doesn't prove the logic is right.

### Deploying to OneStream

The kit doesn't publish anything. To deploy:

1. In OneStream, go to **Application | Presentation | Workspaces** and select the **Revfore Framework (RFA)** workspace
2. Open **XCP_xRfaDlg_ActnExtension** and go to **Assemblies | rfa_actnExtension_os**
3. Add or update the files you changed, using the same folder structure as the kit. This includes `ExtensionHandlerDispatcher.cs` if you registered a new handler
4. Save. The change takes effect immediately

Then test on a real environment. That is where behaviour, as opposed to shape, gets proven.

!!! warning "Keep one source of truth"
    If someone edits a handler directly in OneStream, copy the change back into the kit before your next deploy, or your next deploy overwrites it. Keeping the kit folder in source control makes that drift easy to see.

## Kit updates

Each Revfore release comes with a new kit. The reference stubs, skills and samples are regenerated to match that release.

- **`solutions/` is never overwritten.** Your design work is safe across updates.
- **`rfa_actnExtension_os/` holds your code.** Keep your handlers and your dispatcher registrations when you take a new kit. Take Revfore's own files, such as `IExtensionHandler.cs`, `SolutionHelper.cs` and `SharedMethods.cs`, from the new kit.
- **`Directory.Build.props` and `packages/` are yours.** Keep them unless you're also moving to a new OneStream version.

The skills describe the schema and conventions of the release they shipped with, so update the kit whenever you upgrade the Framework in OneStream. Otherwise Claude generates against an older schema.

## Troubleshooting

| Symptom | Cause | Fix |
|---|---|---|
| **NU1101** Unable to find package OneStream.Platform.ReferenceAssemblies | The package isn't in `packages/`, or its version doesn't match `Directory.Build.props` | Copy the `.nupkg` in, or correct the version |
| **NU1100** Unable to resolve ... | Package source mapping on your machine blocks the source | Keep the `packageSourceMapping` entry in `nuget.config`, pointed at whichever source holds the package |
| **CS0117** '*Type*' does not contain a definition for '*Member*' | Your code uses an API that is newer than the kit's stubs | Update to the kit for your Framework release. If you already have it, send the error and your `KIT_VERSION` line to Revfore |
| A type named in the documentation is missing entirely | Same as above: the kit is behind your release | As above |
| The validator reports errors on JSON Claude wrote | The file was edited by hand, or the kit's skills are older than your release | Ask Claude to fix the reported rules, then validate again |

## Notes

- Keep one kit folder per project or customer. Each has its own `solutions/` and its own copy of the extension assembly, as each OneStream instance does.
- Put the kit folder under source control, but exclude `packages/`, `bin/` and `obj/`. The OneStream package must not be committed to a shared or public repository.
- `samples/GLEntry` is a complete worked example: validated import JSON, sample data, and the `JrlEntryHandler` that goes with it. Read it before writing your first handler.
