# Standard Columns

[← Back to Relational Supporting Overview](index.md)

The **Relational Standard Columns** page defines the reusable column definitions that table columns are built from.

A standard column carries the data type, key behaviour, nullability, default value, and display settings for a kind of column. When a table column is created from a standard column it inherits all of that, so columns of the same kind behave consistently everywhere.

## Overview

Use the Relational Standard Columns page to:

- review the standard columns available when defining table columns
- see the data type, key flags and defaults each one carries
- see which settings a table column will inherit if it does not override them

Standard columns are supplied with the platform. Most solutions consume them rather than adding new ones.

## Relational Standard Column Record Fields

The following fields are used for a Relational Standard Column record.

| Field | Data Type| Purpose | Notes |
|---|---|---|---|
| Standard Column Name | nvarchar | Name of the standard column. | This is the value selected when defining a table column.
| Standard Column Description | nvarchar | Description of the standard column and what it is for. |
| Table/View Column Name | nvarchar | The default column name applied when this standard column is used. |
| Table/View Column Display Name | nvarchar | The default display name applied when this standard column is used. |
| Standard Base Column | int | The base column definition this standard column derives from. | Groups related standard columns that share underlying behaviour.
| Key Flags | int | How the column participates in keys. | Identifies primary, foreign, integration and parent key behaviour, which drives how the column is treated across models and views.
| Data Type | int | The column's data type. |
| Is Nullable | bit | Whether columns created from this standard column allow nulls by default. | A table column can override this.
| Max Length | int | Default maximum length for text columns. |
| Precision | int | Default precision for numeric columns. |
| Scale | int | Default scale for numeric columns. |
| Auto Increment | bit | Whether the column is an identity column. | Only applies to a table's own identity primary key.
| Default Value | nvarchar | Default value applied to the physical column. |
| Bulk Update Flags | int | Whether and how the column can be bulk updated. |
| Is Display Label | bit | Whether the column is used as the display label for its record. |
| Filter Flags | int | How the column can be filtered. |
| Filter Expression | nvarchar | Expression used when filtering on the column. |

## Key Concepts

- **Key Flags is the most consequential setting.** It determines whether a column is treated as a primary key, a foreign key needing a lookup, or the integration code a record is identified by.
- A table column inherits everything from its standard column unless it explicitly overrides it, so leaving a setting unset is usually correct.
- Choosing the right standard column matters more than the column's own settings — it is what makes columns of the same kind behave alike.

## Typical Use Cases

- Checking which standard column to use for a new table column
- Confirming what nullability or default a column will inherit
- Understanding why a column is being treated as a foreign key
- Reviewing the data type and length a standard column applies

## Notes

- Standard columns are shared across every solution in the application; treat them as read-only unless there is a clear reason to change one.
- If no standard column fits a requirement, review the list again before adding one — the intended column often exists under a different name.
- See [Table Columns](../tableDefinitions/columns.md) for how a standard column is applied when defining a column.
