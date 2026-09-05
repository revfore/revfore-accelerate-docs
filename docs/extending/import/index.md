# Load & Extract Files

[← Back to Extending the Framework](../index.md)

A solution's structure — its tables, models, views, lookups and reference data — is defined in **JSON import files** rather than built by hand screen by screen. A single structure file can stand up an entire solution.

This is what makes [AI generation](../../integrations/aiModels/index.md) practical: the deliverable is a file that can be reviewed, versioned and validated before anything touches the database.

The same format works in both directions. An existing solution can be **extracted** back to JSON — see [Extracting a solution](#extracting-a-solution) — which is how you move one between applications, or recover the definition of something built before the file existed.

The two actions are **Load** (read a file in) and **Extract** (write one out).

## Overview

An import file is a single JSON document with up to five top-level sections, processed in this order:

| Section | Defines | Covered in |
|---|---|---|
| **`Tables`** | Physical tables — columns, indexes, relationships | [Structure Definitions](structure.md#tables) |
| **`Models`** | How tables are joined, which columns — stored and expression — are available, and how each behaves | [Structure Definitions](structure.md#models) |
| **`Views`** | The access layer — which columns are exposed, with what security, behaviour and filters | [Structure Definitions](structure.md#views) |
| **`Lookups`** | Reusable dropdown definitions | [Structure Definitions](structure.md#lookups) |
| **`Data`** | Reference and seed rows | [Data Files](data.md) |

Every section is optional. A file that only adds a column contains a single `Tables` entry; a file that only seeds reference data contains only `Data`.

The order matters — a model cannot resolve until its table exists, a view until its model exists, and data until its view exists.

Structure and data are therefore loaded as **two separate steps**, with a sync in between. Loading `Tables`, `Models` and `Views` defines them as metadata; it does not yet create anything in the database. The physical tables and SQL views are created when you **Sync**, and the data load needs them to exist because it writes rows through the views. See [Loading a file](#loading-a-file).

## Everything links by Integration Code

This is the idea to understand first, because it runs through the whole format.

Nothing in an import file refers to a database id. Objects refer to each other by **integration code** — a stable text identifier — so the same file works against any instance regardless of what ids happen to exist there.

Any property ending `_IntegrationCode` is a reference to something else:

```json
{
  "Name": "{RfaExtension}.CpxDepartment_mE",
  "RelModelId_IntegrationCode": "{RfaExtension}.CpxDepartment",
  "ObjectCategoryId_IntegrationCode": "RfaDataEntryView"
}
```

Read that as: *a view named `CpxDepartment_mE`, built over the model `CpxDepartment`, categorised as a data entry view.*

The codes follow predictable shapes:

| Reference to | Shape | Example |
|---|---|---|
| A table or model | `{schema}.Name` | `{RfaExtension}.CpxDepartment` |
| A table column | `{schema}.Table\|Column` | `{RfaExtension}.CpxDepartment\|CpxDepartmentId` |
| A view | `{schema}.ViewName` | `{RfaExtension}.CpxDepartment_mE` |
| A standard column | Its code | `Key_Int` |
| An object category | Its code | `RfaTable`, `RfaDataEntryView` |

Because references are by code, an import is **re-runnable**. Running the same file twice updates what it defined the first time rather than creating duplicates.

## Schema tokens

Names are prefixed with a schema token rather than a literal schema name:

- **`{RfaExtension}`** — your solution's schema. Almost everything you define uses this.
- **`{RfaCore}`** — the framework's own schema, for referring to built-in objects.

The tokens resolve to the real schema names at import time, which is what lets the same file be imported into any instance. Never write a literal schema name.

## Loading a file

Files are selected from your own machine — there is no need to upload them anywhere first.

The action is on all three admin screens, named for what each one offers:

| Screen | Action | Offers |
|---|---|---|
| **Admin \| Relational \| Tables** | **Load/Extract** | Loading a file, and extracting one |
| **Admin \| Relational \| Models** | **Load** | Loading a file |
| **Admin \| Relational \| Views** | **Load** | Loading a file |

Clicking it opens a page where you choose what to do. Loading works the same from any of the three,
so use whichever screen you happen to be on. [Extracting](#extracting-a-solution) is on Tables only.

Standing up a new solution takes five steps, in this order.

### Step 1: Load the structure file

1. Go to **Admin | Relational | Tables**
2. Click **Load/Extract**
3. Choose **Load**, and select the JSON file from a local folder

This defines the tables, models, views and lookups **as metadata**. Nothing exists in the database yet.

### Step 2: Sync the tables

1. Go to **Admin | Relational | Tables**
2. Click **Sync**

This creates the physical tables from the definitions you just imported.

### Step 3: Sync the views

1. Go to **Admin | Relational | Views**
2. Click **Sync**

This creates the SQL views. Both syncs are required before any data can be written.

### Step 4: Load the data file

Same action as Step 1 — **Load/Extract**, then **Load**, choosing the data JSON.

!!!Note Sync before loading data
    Data rows are written through views, so the tables and views have to exist first. Loading a data file before syncing will fail — the view it names is not there yet.

### Step 5: Look at the result

1. Go to **Admin | Relational | Views**
2. Select the view
3. Click **Open View** in the toolbar

This opens the view with its data, which is the quickest confirmation that the structure, the sync and the seed rows all worked.

!!!Note Validate before you load
    A malformed reference — a column code that does not match a real column, a view pointing at a model that is not in the file and not already in the instance — fails on load. When Claude generates a file it validates it against the schema first, which catches most of that class of error beforehand.

## Extracting a solution

Extract is the reverse of load: you select the base tables, and the framework writes a JSON file
holding their definitions, their rows, or both — in the same format a load reads.

Because it is the same format, an extracted file can be loaded straight into another application.
That makes extract the way to promote a solution from development to test to production, to hand a
working solution to a partner, and to capture the definition of something that was built in the UI
before anyone wrote a file for it.

### Running an extract

1. Go to **Admin | Relational | Tables**
2. Select the tables you want to extract
3. Click **Load/Extract**, choose **Extract**, and pick what the file should contain

Extract is on the **Tables** screen only. The Models and Views screens offer **Load** alone, because
the selection an extract works from is base tables.

Everything built on a selected table comes with it — its models, its views, and the lookups defined
over those views — so you pick the tables and the rest follows.

### What the file contains

| Mode | Contains |
|---|---|
| **Structure** | `Tables`, `Models`, `Views` and `Lookups` — the definitions |
| **Data** | `Data` — the rows |
| **Both** | All five sections in one file |

Those are the three options on the extract side of the page.

The file is written to your **ExportAndImport** folder, timestamped, so repeated extracts sit
alongside each other rather than overwriting — the previous one is usually still wanted, to diff
the new one against.

!!!Note Structure and data still load as two steps
    A "Both" extract is one file, but loading it is still the two-step sequence above: load it,
    sync the tables and views, then load it again for the data. Extracting them separately is
    usually simpler.

### What is and is not carried across

Everything in an extracted file is written as an **integration code** with a
[schema token](#schema-tokens) — never a database id, and never a literal schema name. That is what
lets the file be loaded into an application whose ids and schema name are entirely different.

Data rows carry two deliberate omissions:

- **Identity primary keys** are left out. The loading application assigns its own, and a row is
  matched on its integration code instead.
- **Audit columns** — created/updated timestamps and users — are left out. The loading application
  sets them as it writes the rows.

Each table's rows are read through its **maintenance view** (the one whose name ends `_mE`), because
that is the view exposing the full editable column set a load needs to write back. Two consequences
worth knowing:

- A table with **no** `_mE` view extracts its structure but no data, and the extract says so.
- A table with **more than one** `_mE` view is reported rather than guessed at, and its data is
  skipped — extract those views' tables separately.

Every extracted row also needs a value in whatever column the view flags as its integration code,
since that is the key a load upserts against. Rows without one are left out and counted in the
message.

## Notes

- Keep structure and data in separate files. Structure is loaded and synced first; data comes afterwards.
- A structure file can hold all of the tables, models, views and lookups together, or you can split it, as long as each file's dependencies already exist or appear earlier in the same file.
- These files are ordinary text. Keep them in source control alongside your extension code — they are the definition of the solution.
- Re-loading is the normal way to apply a change. Edit the file and load it again rather than hand-editing structures in the UI, so the file stays the source of truth.
- A load defines structure and reference data. It does not deploy [extension code](../handlers/index.md), which is saved separately in the assembly. An extract does not include it either — the assembly moves separately.
- An extracted file is ordinary JSON and can be edited before it is loaded. Extract, edit and load back is a reasonable way to rename or restructure something that already exists.
