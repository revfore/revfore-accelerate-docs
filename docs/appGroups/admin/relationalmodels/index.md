# Relational Models

[← Back to Admin](index.md)

Relational Models define how Relational Tables are **connected and interact with each other**.

They establish relationships between entities and form the structure of your overall data model.

## Overview

Use Relational Models to:

- define relationships between tables
- organize data into a cohesive structure
- enable meaningful data interaction across entities
- create expression columns 
- manage column attributes such as required flags, default values and lookup definitions

Each model definition consists of:

- [Models](models.md) – name, relational table and other model-level settings
- [Sources](sources.md) – a list of relational tables and views that are the sources for the model
- [Columns](columns.md) – a list of relational table columns, expression columns and related attributes
- [Relationships](relationships.md) – relation table relationships that are enabled for the model

## Key Concepts

- Models connect tables and views using defined relationships
- Sources determine tables and views that will be used as sources and how they are connected
- Models provide the structure used by Relational Views

## Typical Use Cases

- Linking Employees to Departments
- Connecting Assets to Locations
- Relating Products to Service Lines

## Notes

- Models should reflect real-world relationships
- Keep relationships simple and well-defined
- A well-designed model improves flexibility and performance