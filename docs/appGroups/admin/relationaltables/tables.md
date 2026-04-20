# Tables

[← Back to Relational Tables Overview](index.md)

The **Relational Tables** page is used to define and manage the header fields for a relational table

Relational tables represent the primary business entities or objects in your solution and provide the foundation for the broader relational framework.

## Overview

Use the Relational Tables page to:

- create new relational table definition
- review existing table definitions
- manage table-level settings
- sync the table definitions with the database

Each relational table should represent a clear business concept, such as a product, employee, asset, service, rates or location.

## Relational Table Header Record Fields

The following fields are used for a Relational Table header record.

| Field | Data Type | Allow Updates | Is Lookup Type | Purpose | Notes |
|---|---|---|---|---|---|
| Relational Table Id | int | Yes | - | Unique identifier for the relational table record. |
| Relational Table Name | nvarchar | Yes | - | Internal name of the relational table. | No spaces or special characters
| Relational Table Display Name | nvarchar | Yes | - | User-friendly display name shown in the application. |
| Relational Table Description | nvarchar | Yes | - | Description of the relational table and its purpose. |
| Alias | nvarchar | Yes | - | Alternate short name or alias for the relational table. | No spaces or special characters
| Type | int | - | Yes | Type classification of the relational table. |
| Is RFA Managed | bit | Yes | - | Indicates whether the table is managed by Revfore Accelerate. |
| Is Custom | bit | Yes | - | Indicates whether the table is custom. |
| Schema | int | Yes | Yes | Schema associated with the relational table. | 
| Is Enabled | bit | Yes | - | Indicates whether the table is enabled for use. |
| Created Date | datetime | - | - | Date and time the record was created. |
| Modified Date | datetime | - | - | Date and time the record was last modified. |
| Created By | int | - | Yes | User who created the relational table record. |
| Modified By | int | - | Yes | User who last modified the relational table record. |

## Typical Use Cases

Use relational tables to define entities such as:

- Employees
- Assets
- Products
- Services
- Locations
- Rates

## Notes

- Tables should be designed around business entities and objects rather than technical groupings.
- Keep table names clear and consistent.
- Tables are the starting point for columns, relationships, models, views, and forms.