# Table Indexes

[← Back to Relational Tables Overview](index.md)

The **Table Indexes** page is used to define and manage indexes for relational tables.

Indexes help improve performance and support efficient data access.

## Overview

Use the Indexes page to:

- define indexes on relational tables
- improve query and retrieval performance
- ensure uniqueness of specific column values such as names
- support efficient filtering and lookups
- optimize how data is accessed in the database

## Relational Table Index Fields

The following fields are used for a Relational Table Index record.

| Field | Data Type | Purpose | Notes |
|---|---|---|---|
| Relational Table | int | Identifies the relational table that the index belongs to. | This links the index record back to its parent relational table. |
| Relational Index Name | nvarchar | Internal name of the relational index. | This should be clear and unique within the table. No spaces or special characters are allowed. |
| Is Unique | bit | Indicates whether the index enforces unique values. | Use this when the indexed column combination must be unique across records. |
| Relational Column 1 | int | Defines the first column included in the index. | This is typically the primary column used by the index. |
| Relational Column 1 Is Ascending | bit | Indicates whether the first indexed column is sorted in ascending order. | If not enabled, the column is sorted in descending order. |
| Relational Column 2 | int | Defines the second column included in the index. | Use when the index includes multiple columns. |
| Relational Column 2 Is Ascending | bit | Indicates whether the second indexed column is sorted in ascending order. | If not enabled, the column is sorted in descending order. |
| Relational Column 3 | int | Defines the third column included in the index. | Use when the index includes multiple columns. |
| Relational Column 3 Is Ascending | bit | Indicates whether the third indexed column is sorted in ascending order. | If not enabled, the column is sorted in descending order. |
| Relational Column 4 | int | Defines the fourth column included in the index. | Use when the index includes multiple columns. |
| Relational Column 4 Is Ascending | bit | Indicates whether the fourth indexed column is sorted in ascending order. | If not enabled, the column is sorted in descending order. |
| Relational Column 5 | int | Defines the fifth column included in the index. | Use when the index includes multiple columns. |
| Relational Column 5 Is Ascending | bit | Indicates whether the fifth indexed column is sorted in ascending order. | If not enabled, the column is sorted in descending order. |
| Is Enabled | bit | Indicates whether the index is enabled for use. | Disabled indexes are not intended for active use. |
| Ext Ref Unique Code | nvarchar | Unique value for the relational index record. | This is readonly and will be auto set the Table Name & Index Name providing a unique value for the record that is used for importing data |
| Created Date | datetime | Date and time the record was created. | This is system maintained. |
| Modified Date | datetime | Date and time the record was last modified. | This is system maintained. |
| Created By | int | User who created the relational index record. | This is system maintained. |
| Modified By | int | User who last modified the relational index record. | This is system maintained. |
| Relational Index Id | int | Unique identifier for the relational index record. | If left blank, the system will typically auto assign it. |

## Key Concepts

Indexes are used to improve performance for common access patterns.

They are especially useful when:

- tables contain large amounts of data
- certain fields are frequently searched or filtered
- joins or relational lookups depend on specific columns
- they are also helpful to ensure uniqueness of specific column values such as a name

## Design Guidance

When working with indexes:

- focus on fields that are commonly used in filtering, searching, or joining
- avoid creating unnecessary indexes
- balance performance benefits with maintenance overhead

## Create a new Relational Index

1. Go to **Admin | Relational Tables**
2. Select the desired Relational Table and click on '**Edit+**'
3. In the Child Views list, Select '**Relational Tables : Indexes**'
4. In the '**Relational Tables : Indexes**' section, Click on '**Add**' or '**Inline Entry**' 
5. Click on the '**+**' button on the top left of the grid
6. Enter required fields and click **Save**
7. Create new [Table Relationships](relationships.md)

!!!Note Important Notes
    The Ext Ref Unique Code and Relational Index Id will be auto-assigned

    The Relational Index Name will be auto-assigned if blank or has the value 'Auto()' in it

    See [General Actions](../../../concepts/metadataDrivenUI/actions.md#general-actions) for more information about adding records

## Notes

- Indexes are a technical optimization feature and may not be needed for every table.
- Index strategy should align with expected usage patterns.
- Use indexes thoughtfully to support performance without overcomplicating the design.