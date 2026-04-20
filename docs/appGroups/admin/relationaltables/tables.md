# Tables

[← Back to Relational Tables Overview](index.md)

The **Relational Tables** page is used to define and manage the header fields for a relational table

Relational tables represent the primary business entities or objects in your solution and provide the foundation for the broader relational framework.

## Overview

Use the Relational Tables page to:

- create new relational table definitions
- review existing table definitions
- manage table-level settings
- sync the table definitions with the database

Each relational table should represent a clear business concept, such as a product, employee, asset, service, rates or location.

## Relational Table Header Record Fields

The following fields are used for a Relational Table header record.

| Field | Data Type| Purpose | Notes |
|---|---|---|---|
| Relational Table Name | nvarchar | Internal name of the relational table. | This is the name of the actual table in the database.  No spaces or special characters are allowed.
| Relational Table Display Name | nvarchar | User-friendly display name shown in the application. |
| Relational Table Description | nvarchar | Description of the relational table and its purpose. |
| Alias | nvarchar | Alternate short name or alias for the relational table. | This will be used for joining tables and views, and as the prefix for model and view column names. No spaces or special characters are allowed.
| Is RFA Managed | bit | Indicates whether the table is managed by Revfore Accelerate. | If Managed, the table header, columns, indexes and relationships are defined in RFA and created from RFA using the Sync process.  If not managed, the table is create directly in the database by some other method and RFA only references it.  Both RFA managed and non-managed tables can be used in Relational Models and Views.
| Is Custom | bit | Indicates whether the table is custom. | Almost all non-system tables will be custom.
| Schema | int | Schema associated with the relational table.  Schemas provide a unique namespace for tables and views. | This will be auto-assigned based on the Is Custom value.  Custom tables should be assigned to the custom schema which is the one with 'x' in the name.
| Is Enabled | bit | Indicates whether the table is enabled for use. |
| Ext Ref Unique Code | nvarchar | Unique value for the relational table record. | This is readonly and will be auto set the same value as the Table Name providing a unique value for the record that is used for importing data
| Created Date | datetime | Date and time the record was created. |
| Modified Date | datetime | Date and time the record was last modified. |
| Created By | int | User who created the relational table record. |
| Modified By | int | User who last modified the relational table record. |
| Relational Table Id | int | Unique identifier for the relational table record. | If you leave blank, the system will auto assign

## Typical Use Cases

Use relational tables to define business entities and objects such as:

- Employees
- Assets
- Products
- Services
- Locations
- Rates
  
## Create a new Relational Table

1. Go to **Admin | Relational Tables**
2. Click on '**Add+**' or '**Inline Entry**'
3. Click on the '**+**' button on the top left of the grid
4. Enter required fields and click **Save**
5. Create new [Relational Columns](columns.md)
6. Create new [Relational Index](indexes.md)
7. Create new [Table Relationships](relationships.md)
8. Click on the **Sync** button to sync the table definition with the database

!!!Note Important Notes
    The Schema, Ext Ref Unique Code and Relational Table Id will be auto-assigned
    See [Adding New Records](addingNewRecords.md) for general information about adding records

## Notes

- Tables should be designed around business entities and objects rather than technical groupings.
- Keep table names clear and consistent.
- Tables are the starting point for columns, relationships, models, views, and forms.