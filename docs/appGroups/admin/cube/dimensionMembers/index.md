# Dimension Members

[← Back to Admin](../../index.md)

Dimension Members are the **OneStream dimensions and members the Framework maps relational data to**.

Every place the Framework writes data to a cube, it does so at a specific member of each dimension. These pages define which dimensions and members are available to select.

## Overview

Use Dimension Members to:

- register the dimensions the solution uses
- register the members within each dimension
- make members available for selection in workflow and cube configuration
- limit a member to a period of time using effective dates

Dimension Members consists of:

- [Dimensions](dimensions.md) – the dimension types, such as Entity, Account, Scenario or a user-defined dimension
- [Members](members.md) – the individual members within each dimension

## Key Concepts

- A dimension groups members; a member is the individual value data is written at.
- Every member belongs to exactly one dimension.
- Members are referenced throughout workflow configuration — base and summary members, scenario members, and data source members all select from here.
- Members carry their OneStream identifiers, so a record here corresponds to a member that exists in OneStream.
- Members can be marked as parent or leaf, reflecting their position in the OneStream hierarchy.

## Typical Use Cases

- Registering the Entity members workflow units post to
- Registering the Scenario members used by budget and forecast cycles
- Registering the Account members a solution writes amounts to
- Registering user-defined dimension members used for analysis

## Setup Order

1. Create the [Dimensions](dimensions.md) the solution uses
2. Create the [Members](members.md) within each dimension
3. Select those members when configuring [workflow areas](../../workflow/areas/index.md), [unit member sets](../../workflow/units/memberSets.md) and [item types](../../workflow/supporting/itemTypes.md)

!!!Note Important Notes
    Integration Codes and record Ids are auto-assigned

    See [General Actions](../../../../concepts/metadataDrivenUI/actions.md#general-actions) for more information about adding records

## Notes

- Register only the dimensions and members the solution actually uses, rather than mirroring the whole OneStream application.
- Names should match OneStream exactly, since they are how members are identified when data is synced.
