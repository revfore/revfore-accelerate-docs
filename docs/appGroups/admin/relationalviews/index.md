# Relational Views

[← Back to Admin](index.md)

Relational Views provide reusable, user-facing representations of relational data in Revfore Accelerate.

Views leverage **Relational Models** as their foundation. A single relational model can support many relational views, each designed for a different use case such as data entry, reporting, analysis, or navigation.

## Overview

Use Relational Views to:

- expose model data for user interaction
- create reusable outputs for reporting and application use
- present a focused subset of model columns
- define which actions can be taken on the view data
- apply filters to the view data
- integrate relational data into dashboards, forms, and pages

Each view definition consists of:

- [Views](views.md) – name, schema, and other view-level settings
- [Columns](columns.md) – the subset of model columns exposed in the view
- [Actions](actions.md) – actions that can be taken on the view data
- [Filters](filters.md) – filters that can be applied to the view data

## Key Concepts

- Views are built on top of Relational Models.
- A single relational model can support multiple views.
- Views allow you to expose only the subset of model columns needed for a specific use case.
- Views define how data is displayed, accessed, and interacted with.
- Views can be used in forms, dashboards, navigation pages, and other user-facing experiences.

## Typical Use Cases

- Data entry views
- Reporting views
- Analysis views
- Planning data outputs
- Data integration into Genesis pages
- User-facing data displays

## Notes

- Views should be designed around a specific user experience or reporting need.
- Keep view definitions focused and maintainable.
- Use multiple targeted views from the same model rather than trying to make one view do everything.
- Views are often the primary way users interact with relational data.