# Data Files

[← Back to Import Files](index.md)

The `Data` section seeds rows — the reference data a solution needs before anyone can use it. Categories, types, statuses, default configuration.

It uses the same file format as [structure definitions](structure.md) and the same **Import** action.

!!!Note Data comes after the sync
    A data file can only be imported once the tables and views it writes to actually exist. That means importing the structure file, then **Sync** on [Relational Tables](../../appGroups/admin/relational/relationalTables/tables.md) and again on [Relational Views](../../appGroups/admin/relational/relationalViews/views.md), and only then the data file. See [Running an import](index.md#running-an-import) for the full sequence.

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

## Re-running an import

Data imports **match on integration code**. A row whose code already exists is updated; a row whose code is new is inserted. Nothing is duplicated.

That makes a data file the maintainable way to manage reference data:

- edit the file and re-import to change a description or add an entry
- keep it in source control, so what is in the file is what is in the instance
- promote the same file between environments

!!!Note Set an integration code on anything you intend to re-import
    Without one there is nothing to match on, and a second import inserts a second row instead of updating the first. For seed data you expect to maintain, give every row a stable code and never change it — the code is the identity, not the name.

## What belongs here

Good candidates:

- **Reference lists** — categories, types, statuses, units of measure
- **Configuration rows** the solution needs to function
- **Starter records** that make a new instance usable immediately

Less suitable:

- **Transactional data.** Use the file import actions on the views themselves, which are built for volume.
- **Anything users maintain.** If people will edit it in the UI, seeding it once then re-importing later will overwrite their changes.

That last point is the important judgement call. A data file is a statement that the file is the source of truth. Where the users are the source of truth, seed the initial rows and then leave the file alone.

## Notes

- Row keys are view column names, with the model alias prefix.
- Rows are validated exactly as user input is — an import failure usually means a genuine data problem, not a format one.
- Order blocks so referenced rows exist first.
- Keep data in its own file. It is imported after the tables and views have been synced, so it belongs to a later step than the structure it depends on.
