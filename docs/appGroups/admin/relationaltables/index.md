# Indexes

[← Back to Relational Tables Overview](index.md)

The **Indexes** page is used to define and manage indexes for relational tables.

Indexes help improve performance and support efficient data access.

## Overview

Use the Indexes page to:

- define indexes on relational tables
- improve query and retrieval performance
- support efficient filtering and lookups
- optimize how data is accessed in the database

## Key Concepts

Indexes are used to improve performance for common access patterns.

They are especially useful when:

- tables contain large amounts of data
- certain fields are frequently searched or filtered
- joins or relational lookups depend on specific columns

## Design Guidance

When working with indexes:

- focus on fields that are commonly used in filtering, searching, or joining
- avoid creating unnecessary indexes
- balance performance benefits with maintenance overhead

## Notes

- Indexes are a technical optimization feature and may not be needed for every table.
- Index strategy should align with expected usage patterns.
- Use indexes thoughtfully to support performance without overcomplicating the design.