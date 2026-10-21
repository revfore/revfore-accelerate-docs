# Data Files

[← Back to Import Files](index.md)

The `Data` section seeds rows — the reference data a solution needs before anyone can use it. Categories, types, statuses, default configuration.

It uses the same file format as [structure definitions](structure.md) and the same **Load** action, and is what an [extract](index.md#extracting-a-solution) in Data mode produces.

!!!Note Data comes after the sync
    A data file can only be loaded once the tables and views it writes to actually exist. That means loading the structure file, then **Sync** on [Relational Tables](../../appGroups/admin/relational/relationalTables/tables.md) and again on [Relational Views](../../appGroups/admin/relational/relationalViews/views.md), and only then the data file. See [Loading a file](index.md#loading-a-file) for the full sequence.

    This is why structure and data are normally kept in separate files — they are imported at different points in the sequence.

## Shape

```json
{
  "Data": [
    {
      "ViewId_IntegrationCode": "{RfaExtension}.CpxCategory_mE",
      "Rows": [
        { "Cat_Name": "Equipment", "Cat_Description": "Machinery and equipment", "Cat_DefaultUsefulLifeInMonths": 60, "Cat_IsActive": true },
        { "Cat_Name": "Software",  "Cat_Description": "Software licenses and subscriptions", "Cat_DefaultUsefulLifeInMonths": 36, "Cat_IsActive": true }
      ]
    }
  ]
}
```

Each block names one view and the rows to write through it. Both properties are required.

## Rows go through a view, not a table

This is the part that catches people out.

Rows are written **through a view**, so the keys in each row are **view column names** — the aliased form, `Cat_Name` rather than `Name`. That is deliberate, and it buys three things:

- **Validation applies.** Required fields, lengths and formats are enforced exactly as they would be for a user typing into the screen.
- **Lookups resolve.** A column backed by a lookup can take the readable value and resolve it, rather than needing a raw id.
- **Defaults and audit fields fill themselves.** Anything the view defaults on insert is applied, and created/modified fields are maintained by the framework.

The practical consequence: **you can only seed what the view exposes.** If a column is not on the view, it cannot be set from a data file. Pick a view that carries every column you need — commonly the data entry view.

## Referring to other rows

Reference data usually has relationships — a category belonging to a type, an item type mapped to an area. Point at the other row by its integration code rather than an id, exactly as structure definitions do:

```json
{ "Itm_Name": "Laptop", "Itm_CategoryId_IntegrationCode": "Equipment" }
```

This is what makes a data file portable. The same file seeds a development instance and a production one, even though the underlying ids differ.

Order your blocks so referenced rows are created first — categories before the items that point at them.

A code that names a relational object — a view, a table, a schema — takes the same schema tokens as a structure file, and they are resolved before rows are matched:

```json
{ "RelVw_IntegrationCode": "{RfaExtension}.PrtTopSales_mG", "RelVw_NavigateToEnabled": false }
```

Never write the literal schema name (`rfa01x`) — another environment's schema can be named differently. A bare `{RfaExtension}` is the schema itself. Ordinary codes like `Equipment` have no token and are left as they are.

## Re-running an import

Data imports **match on integration code**. A row whose code already exists is updated; a row whose code is new is inserted. Nothing is duplicated.

That makes a data file the maintainable way to manage reference data:

- edit the file and re-import to change a description or add an entry
- keep it in source control, so what is in the file is what is in the instance
- promote the same file between environments

!!!Note Set an integration code on anything you intend to re-import
    Without one there is nothing to match on, and a second import inserts a second row instead of updating the first. For seed data you expect to maintain, give every row a stable code and never change it — the code is the identity, not the name.

### Update only, or insert only

An entry's `Mode` changes how its rows meet the ones already there:

| `Mode` | A row whose code exists | A row whose code is new |
|---|---|---|
| `Upsert` (the default) | is updated | is inserted |
| `Update` | is updated | stops the import, naming the code |
| `Insert` | is left as it is, and reported | is inserted |

In every mode a row sets only the columns it gives — an existing row keeps its other values.

**`Update`** is for changing a few columns on rows that already exist, such as switching a setting on a core view. Each row needs only its integration code and the columns that change, and a mistyped code is reported instead of attempting a half-filled insert.

```json
{
  "ViewId_IntegrationCode": "{RfaCore}.RelView_mE",
  "Mode": "Update",
  "Rows": [
    { "RelVw_IntegrationCode": "{RfaExtension}.PrtTopSales_mG", "RelVw_NavigateToEnabled": false }
  ]
}
```

**`Insert`** is for starter rows that the customer may change afterwards — default categories, initial settings. Setup re-runs data files on every upgrade; with `Insert` the rows are added once and the customer's later edits are never overwritten.

## What belongs here

Good candidates:

- **Reference lists** — categories, types, statuses, units of measure
- **Configuration rows** the solution needs to function
- **Starter records** that make a new instance usable immediately

Less suitable:

- **Transactional data.** Plans, journals, actuals and other high-volume files are a recurring business process, not part of setting up a solution. Users load them with **File Import** on the views themselves, which takes this same format, or — for the largest recurring files — with extension code. See [High-volume transactional files](#high-volume-transactional-files) below.
- **Anything users maintain.** If people will edit it in the UI, re-importing it later overwrites their changes. Seed it with `"Mode": "Insert"`, which adds the rows once and never touches them again, or leave it out of the file.

That last point is the important judgement call. A data file is a statement that the file is the source of truth. Where the users are the source of truth, seed the initial rows and then leave them alone.

### High-volume transactional files

Grid **File Import** handles volume reasonably well: it compares the file with the existing rows first, then inserts and updates in batches. That first compare — matching every row, checking it against the grid's scope, validating it and listing it for review — is what makes it a little slower, and it is worth having when a user is checking what a file will change. For a file of thousands of transactions loaded every period, the compare and review are overhead nobody uses.

For those, write a **custom action** in the extension assembly:

1. **Ask for the file** with the shared **Select File** form. The action calls `SharedMethods.InitializeFileSelect` and names the form in its `ShaFormId_IntegrationCode`. When the user uploads a file and presses Proceed, the form calls the same action again, and `SharedMethods.GetFileSelectFilePath` returns the file.
2. **Read it** with `SharedMethods.ReadFileText`, and pass the whole document to SQL Server as **one parameter**.
3. **Check it** with a `select` over `openjson(@Json, '$.Data[0].Rows')`: missing keys, duplicates, and lookup codes that match no row. Refuse the file with counts if anything is wrong.
4. **Load it** with one `insert … select … from openjson(…)` — or a delete and an insert, for a file that replaces a period's rows — in one transaction.

The file can be in the [data file](#shape) format, so it is the same file a grid exports. However many rows it holds, the load is one round trip and one statement.

!!! warning "Your code takes over what the grid did"
    A set-based load writes straight to the table. It skips the row-by-row validation, save handlers, default values and audit stamping that a grid save, File Import or Load provides, so the action has to do what it needs itself: resolve each `_IntegrationCode` to its id by joining to the lookup's table, set the audit columns, and check the rows before inserting them. Keep small master tables — customers, classes, statuses — on the ordinary path, where all of that comes for free.

The [Extension Kit](../extension-kit.md)'s reference has the worked code: **Ask the user, then run your code** in `docs/actions.md`, and loading JSON in one statement in `docs/sql.md`.

## From a grid: File Export and File Import

A grid's **File Export** can write this same format, and **File Import** reads it back — see [File Export and Import as JSON](../../concepts/metadataDrivenUI/actions.md#file-export-and-import-as-json).

The file holds one block per view: the grid's view first, with the rows its filters select, then each level of child views with the rows that belong to them. Child rows point at their parent by its integration code, so a new order and its new lines import together.

Because it is the same format, a grid export can also be loaded with **Load**. The difference is scope: from a grid, a file can only touch the rows that grid shows and its child records; **Load** is an administrator's tool and writes wherever the file says.

## Notes

- Row keys are view column names, with the model alias prefix.
- Rows are validated exactly as user input is — an import failure usually means a genuine data problem, not a format one.
- Order blocks so referenced rows exist first.
- Keep data in its own file. It is imported after the tables and views have been synced, so it belongs to a later step than the structure it depends on.
