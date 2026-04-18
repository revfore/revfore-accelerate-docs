# Columns

[← Back to Relational Models Overview](index.md)

The **Columns** page is used to define the columns exposed by a relational model.

These columns determine what data is included in the model and made available for downstream use.

## Overview

Use the Columns page to:

- select or define model-level columns
- organize the data structure presented by the model
- prepare the model for views, reporting, and interaction
- control how source data is surfaced in a reusable way

## Key Concepts

Model columns typically build on data from the underlying sources.

They may represent:

- direct fields from source tables
- model-specific combinations or arrangements of source data
- standardized structures for downstream use

## Design Guidance

When defining model columns:

- include only the fields needed for the use case
- use clear and consistent naming
- align columns with how users will consume the model
- keep the structure maintainable and reusable

## Notes

- Model columns shape how the model is understood and used.
- Clear column design improves reporting, views, and user experience.
- Model columns often become the basis for Relational Views.