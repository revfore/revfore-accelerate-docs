# Database Tables

[← Back to Database Overview](index.md)

The **Database Tables** page lists the tables that physically exist in the database.

It shows every table, whether or not the Framework has a definition for it, and provides actions to inspect a table, create a definition for it, or drop it.

## Overview

Use the Database Tables page to:

- see which tables exist in the database
- see which of them are linked to a relational object definition
- inspect a table's columns, indexes, relationships and data
- create a Direct SQL definition for a table that has none
- drop a table that is no longer needed

This page is read-only — the rows themselves cannot be edited. Everything is done through the actions.

## Database Table Record Fields

The following fields are shown for each database table.

| Field | Data Type| Purpose | Notes |
|---|---|---|---|
| Schema Name | nvarchar | The database schema the table belongs to. |
| Table Name | nvarchar | The name of the table in the database. |
| Relational Object | int | The relational object definition linked to this table. | Blank where the table has no Framework definition.
| Relational Object Display Name | nvarchar | Display name of the linked relational object. |
| Relational Object Category Name | nvarchar | Category of the linked relational object. |
| Is Table | bit | Indicates the object is a table. |
| Is View | bit | Indicates the object is a view. |
| Is Stored Procedure | bit | Indicates the object is a stored procedure. |
| Is Custom | bit | Indicates the object is custom rather than part of the core product. |
| Table Id | int | Identifier for the object record. |

## Actions

| Action | What it does | Notes |
|---|---|---|
| **Inspect Table** | Inspect columns, indexes, relationships and data. | Read-only. Use this to review a table's structure or check its contents.
| **Add Definition** | Add a Direct SQL relational table definition. | Creates a Framework definition for a table that already exists in the database. Use this to adopt a table created outside the Framework.
| **Drop Table** | Drop the table from the database. | Removes the table and its data. See the warning below.

!!!Warning Drop Table
    Dropping a table removes it from the database along with all of its data. This is not the same as disabling or deleting a relational table definition, and it cannot be undone from within the application.

## Typical Use Cases

- Adopting a table created directly in SQL, so the Framework can use it in models and views
- Confirming a relational table definition has actually been synced to the database
- Reviewing a table's columns or data while troubleshooting
- Removing a table left behind by earlier work

## Inspect a table

1. Go to **Admin | Relational | Database**
2. Open the **Tables** page
3. Select the table
4. Click **Inspect Table**

## Add a definition for an existing table

1. Go to **Admin | Relational | Database**
2. Open the **Tables** page
3. Select a table that has no Relational Object shown
4. Click **Add Definition**
5. Review the created definition under [Relational Tables](../tableDefinitions/index.md)

!!!Note Important Notes
    See [General Actions](../../../../concepts/metadataDrivenUI/actions.md#general-actions) for more information about working with records

## Notes

- A table with no Relational Object is not managed by the Framework and will not appear in models or views until a definition exists.
- A definition added here is a Direct SQL definition: the Framework references the table but does not manage its structure.
- Inspecting never changes anything, so it is always safe to start there.
