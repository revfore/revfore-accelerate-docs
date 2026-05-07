# Relational Tables

[← Back to Admin](index.md)

Relational Tables are the **foundation of the Revfore Accelerate data model**.

They define the core business entities and store data at its natural level of detail.

## Overview

Use Relational Tables to:

- define business entities and objects (e.g., Employees, Assets, Products, Rates)
- store detailed operational and planning data
- structure data in a flexible, relational format

Each table definition consists of:

- [Tables](tables.md) – name, alias, schema, and other table-level settings
- [Columns](columns.md) – fields that define the attributes of the business entity or object
- [Indexes](indexes.md) – indexes used to improve query performance, support efficient data access, and enforce uniqueness when required
- [Relationships](relationships.md) – relationships to related tables, primarily through foreign keys, but also for navigation between related records

## Key Concepts

- Tables are defined using metadata, so relational structures can be configured without writing SQL.
- Tables represent real-world business entities, objects, and hierarchical structures.
- Tables can be reused across models, views, and forms.
- Tables include both business fields and table-level configuration such as naming, schema, and status.
- Tables can be related to other tables through foreign keys or through navigation-oriented relationships.
- Tables can use indexes to improve performance and enforce uniqueness where needed.

## Typical Use Cases

- Employee planning
- Asset tracking
- Product and service modeling
- Operational data capture

## Create a new Relational Table

1. Go to **Admin | Relational Tables**
2. Click on '**Add+**' or '**Enable Inline Adding & Editing**'
3. Click on the '**+**' button on the top left of the grid
4. Enter required fields and click **Save**
5. Create new [Relational Columns](columns.md)
6. Create new [Relational Index](indexes.md)
7. Create new [Table Relationships](relationships.md)
8. Click on the **Sync** button to sync the table definition with the database

**Enable Inline Adding & Editing** allows adding and modifying rows directly in the grid

!!!Note Important Notes
    The Schema, Ext Ref Unique Code and Relational Table Id will be auto-assigned
    
    See [General Actions](../../../concepts/metadataDrivenUI/actions.md#general-actions) for more information about adding records

## Notes

- Tables should be designed around clear business concepts
- Avoid combining unrelated data into a single table
- Keep naming consistent and meaningful