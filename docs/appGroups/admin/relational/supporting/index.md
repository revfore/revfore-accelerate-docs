# Relational Supporting

[← Back to Admin](../../index.md)

Relational Supporting holds the **shared definitions the rest of the relational model draws on**.

These are defined once and reused across tables, models, and views. Standard columns in particular are the vocabulary every table column is built from.

## Overview

Relational Supporting consists of:

- [Standard Columns](standardColumns.md) – the reusable column definitions every table column inherits from
- [Lookups](lookups.md) – the controlled lists and pickers that turn stored ids into readable values
- [Cube Views](cubeViews.md) – the view definitions that surface data in dashboards
- [Forms](forms.md) – the user-facing interfaces for entering and editing data

## Key Concepts

- A standard column carries the data type, key flags, nullability and defaults that a table column inherits, so columns of the same kind behave the same way everywhere.
- A lookup turns a stored foreign-key id into a value a user can choose from and read.
- Cube views and forms are the presentation layer — they surface what the relational model defines.
- Because these are shared, changing one affects everywhere it is used. Review before editing.

## Typical Use Cases

- Checking which standard column to use when adding a table column
- Defining a controlled list for a new foreign key
- Reviewing the views that surface a solution's data
- Creating a form for a custom action

## Notes

- Standard columns are supplied with the platform; most solutions consume them rather than adding new ones.
- Lookups and forms are commonly created per solution.
- These definitions are shared, so a change here can affect more than the solution you are working on.
