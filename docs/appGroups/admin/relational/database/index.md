# Database

[← Back to Admin](../../index.md)

The Database pages show **what actually exists in the database**, as opposed to what is defined in the Framework.

Everywhere else under Relational you work with definitions. These two pages look at the database itself and let you compare the two — useful when a definition and the physical object have drifted apart, or when adopting a table that already exists.

## Overview

Use the Database pages to:

- see the tables and views that physically exist in the database
- see which of them are already linked to a Framework definition
- inspect an object's structure and data without leaving the application
- create a Framework definition for an object that does not have one
- drop an object that is no longer needed

The Database pages consist of:

- [Tables](tables.md) – the physical tables in the database
- [Views](views.md) – the physical views in the database

## Key Concepts

- These pages list the database's own objects, so an object appears here whether or not the Framework knows about it.
- Each row shows whether the object is linked to a relational object definition, and which one.
- **Add Definition** creates a Direct SQL definition for an object that exists in the database but has no Framework definition — this is how an externally created table is adopted.
- **Inspect** opens the object's structure and data for review, without changing anything.
- **Drop** removes the object from the database itself, not just its definition.

## Typical Use Cases

- Adopting a table created outside the Framework
- Checking whether a definition has actually been synced to the database
- Reviewing the columns or data of an object while troubleshooting
- Removing objects left behind by earlier work

!!!Warning Dropping objects
    Drop removes the object from the database, along with any data it holds. It is not the same as disabling or deleting a definition.

## Notes

- An object listed here with no relational object shown is not managed by the Framework.
- Use [Tables](tables.md) and [Views](views.md) to review before changing anything — inspecting is always safe.
