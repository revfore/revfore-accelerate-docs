# Workflow Supporting

[← Back to Admin](../../index.md)

Workflow Supporting holds the **reference data the rest of the workflow configuration is built from**.

These records are defined once and reused across workflow units, instances, and areas. They rarely change after a solution is set up.

## Overview

Use Workflow Supporting to:

- define the kinds of member set assignment available
- define the categories of work an item can belong to
- define the types of workflow item the solution supports
- map item types to the areas and categories they apply to

Workflow Supporting consists of:

- [Member Set Types](memberSetTypes.md) – the kinds of dimension-member assignment, such as base or summary scope
- [Item Categories](itemCategories.md) – the categories of work an item belongs to, with their own [Member Sets](itemCategoryMemberSets.md)
- [Item Types](itemTypes.md) – the types of workflow item, with their [Areas](itemTypeAreas.md) and [Item Categories](itemTypeItemCategories.md)

## Key Concepts

- Supporting records are reference data: define them during setup, then reuse them.
- Item types are mapped to areas and to categories through child records, so one item type can serve several areas.
- A mapping can be marked primary, which identifies the default where an item type has more than one.
- Item category member sets serve the same purpose as unit member sets, but scoped to a category rather than a unit.
- Mappings are effective-dated, so configuration can change over time without losing history.

## Typical Use Cases

- Defining the item types a solution captures, such as Capital Request or Headcount Change
- Grouping items into categories for reporting or approval routing
- Mapping an item type to the workflow areas it appears in
- Assigning dimension members at category level rather than per unit

## Setup Order

1. Create the [Member Set Types](memberSetTypes.md)
2. Create the [Item Categories](itemCategories.md) and their [Member Sets](itemCategoryMemberSets.md)
3. Create the [Item Types](itemTypes.md)
4. Map each item type to its [Areas](itemTypeAreas.md) and [Item Categories](itemTypeItemCategories.md)

!!!Note Important Notes
    Ext Ref Unique Codes and record Ids are auto-assigned throughout

    See [General Actions](../../../../concepts/metadataDrivenUI/actions.md#general-actions) for more information about adding records

## Notes

- Set these up before creating workflow instances, since instances depend on them.
- Keep the number of item types and categories small; they multiply through the mapping tables.
