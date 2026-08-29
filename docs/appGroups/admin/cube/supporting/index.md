# Dimension Types

[← Back to Admin](../../index.md)

Dimension Types are the **OneStream dimensions available to the Framework**.

A dimension type groups the members data can be written at — Entity, Account, Scenario, Flow, IC, and the user-defined dimensions. Every [dimension member](../dimensions/index.md) belongs to exactly one of them.

!!! note "Read-only"
    Dimension types come from OneStream and are refreshed by **Sync**. They cannot be added or edited here. To change a dimension, change it in OneStream and sync again.

## Overview

Use the Dimension Types page to:

- see which dimensions are available to the Framework
- confirm a dimension exists before working with its members
- refresh the list after dimensions change in OneStream

## Dimension Type Record Fields

The following fields are shown for a Dimension Type record. All are populated from OneStream.

| Field | Data Type| Purpose | Notes |
|---|---|---|---|
| Name | nvarchar | Name of the dimension in OneStream. |
| Display Name | nvarchar | User-friendly display name. |
| Description | nvarchar | Description of the dimension. |
| Type Flag | int | Classifies the kind of dimension. | Identifies which OneStream dimension type the record represents.
| Visibility Flags | int | Controls where the dimension is available for selection. |
| Integration Code | nvarchar | Unique value for the dimension record. | Used for importing data
| Created Date | datetime | Date and time the record was created. |
| Modified Date | datetime | Date and time the record was last modified. |
| Created By | int | User who created the dimension record. |
| Modified By | int | User who last modified the dimension record. |
| Dimension Id | int | Unique identifier for the dimension record. |

## Typical Dimensions

- Entity
- Account
- Scenario
- Flow
- Intercompany
- User-defined dimensions (UD1 through UD8)

## Sync dimension types from OneStream

1. Go to **Admin | Cube | Supporting**
2. Open the **Dimension Types** page
3. Click **Sync**

Syncing brings the current set of dimensions across from OneStream.

## Notes

- A dimension must exist here before its [members](../dimensions/index.md) are available.
- Because the list mirrors OneStream, this page is a quick way to confirm the Framework is seeing the dimensions you expect.
