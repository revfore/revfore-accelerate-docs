# Import Files

[← Back to Extending the Framework](../index.md)

A solution's structure — its tables, models, views, lookups and reference data — is defined in **JSON import files** rather than built by hand screen by screen. A single structure file can stand up an entire solution.

This is what makes [AI generation](../../integrations/aiModels/index.md) practical: the deliverable is a file that can be reviewed, versioned and validated before anything touches the database.

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

Structure and data are therefore imported as **two separate steps**, with a sync in between. Importing `Tables`, `Models` and `Views` defines them as metadata; it does not yet create anything in the database. The physical tables and SQL views are created when you **Sync**, and the data import needs them to exist because it writes rows through the views. See [Running an import](#running-an-import).

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

## Running an import

Files are selected from your own machine — there is no need to upload them anywhere first.

Standing up a new solution takes five steps, in this order.

### Step 1: Import the structure file

1. Go to **Admin | Relational | Tables** (or **Admin | Relational | Models**)
2. Click **Import**
3. Click **Select File & Import** and choose the JSON file from a local folder

The action is described in the product as *Import Relational Tables, Models, Views and Data from AI Generated File*.

This defines the tables, models, views and lookups **as metadata**. Nothing exists in the database yet.

### Step 2: Sync the tables

1. Go to **Admin | Relational | Tables**
2. Click **Sync**

This creates the physical tables from the definitions you just imported.

### Step 3: Sync the views

1. Go to **Admin | Relational | Views**
2. Click **Sync**

This creates the SQL views. Both syncs are required before any data can be written.

### Step 4: Import the data file

Same action as Step 1 — **Import**, then **Select File & Import**, choosing the data JSON.

!!!Note Sync before importing data
    Data rows are written through views, so the tables and views have to exist first. Importing a data file before syncing will fail — the view it names is not there yet.

### Step 5: Look at the result

1. Go to **Admin | Relational | Views**
2. Select the view
3. Click **Open View** in the toolbar

This opens the view with its data, which is the quickest confirmation that the structure, the sync and the seed rows all worked.

!!!Note Validate before you import
    A malformed reference — a column code that does not match a real column, a view pointing at a model that is not in the file and not already in the instance — fails at import. When Claude generates a file it validates it against the schema first, which catches most of that class of error before you import it.

## Notes

- Keep structure and data in separate files. Structure is imported and synced first; data comes afterwards.
- A structure file can hold all of the tables, models, views and lookups together, or you can split it, as long as each file's dependencies already exist or appear earlier in the same file.
- Import files are ordinary text. Keep them in source control alongside your extension code — they are the definition of the solution.
- Re-importing is the normal way to apply a change. Edit the file and import it again rather than hand-editing structures in the UI, so the file stays the source of truth.
- The import defines structure and reference data. It does not deploy [extension code](../handlers/index.md), which is saved separately in the assembly.
