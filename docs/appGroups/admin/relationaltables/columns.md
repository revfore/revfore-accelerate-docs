# Columns

[← Back to Relational Tables Overview](index.md)

The **Columns** page is used to define and manage the fields that belong to a relational table.

Columns describe the attributes of each record stored in a table.

## Overview

Use the Columns page to:

- add new columns to a relational table
- define field names and properties
- manage the structure of table data
- control how data is stored and displayed

## Key Concepts

Each column represents a single attribute of a business entity.

Examples include:

- Name
- Department
- Role
- Status
- Cost
- Effective Date

## Design Guidance

When defining columns:

- use clear and meaningful names
- align fields to real business attributes
- avoid adding unnecessary columns too early
- keep the structure flexible enough to support future growth

## Create a new Relational Column

1. Go to **Admin | Relational Tables**
2. Select the desired Relational Table and click on '**Edit+**'
3. In the Child Views list, Select '**Relational Tables : Columns**'
4. In the '**Relational Tables : Columns**' section, Click on '**Add**' or '**Inline Entry**' 
5. Click on the '**+**' button on the top left of the grid
6. Enter required fields and click **Save**
7. Create new [Relational Index](indexes.md)

## Notes

- Columns should be specific to the purpose of the table.
- Well-defined columns improve usability, reporting, and downstream configuration.
- Column design directly impacts forms, models, and views.