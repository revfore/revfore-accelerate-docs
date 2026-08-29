# Dimension Members

[← Back to Admin](../../index.md)

Dimension Members are the **OneStream dimension members available to the Framework**.

A member is the specific value data is written at or bounded by — a particular entity, account, or scenario. These are what you select when configuring workflow areas, unit member sets, and item types.

!!! note "Synced from OneStream, with some overrides"
    Members come from OneStream and are refreshed by **Sync**. Most fields are read-only, but a few can be overridden here — see [Fields you can override](#fields-you-can-override) below. Everything else is maintained in OneStream.

## Overview

Use the Dimension Members page to:

- see which members are available for selection
- refresh the list after members change in OneStream
- disable a member so it stops appearing in selection lists
- correct how a member's position in the hierarchy is treated

## Fields you can override

These three are yours to set. Everything else on the record comes from OneStream and is read-only.

| Field | Purpose | Why you would change it |
|---|---|---|
| **Is Enabled** | Whether the member is available for selection. | Keep a member out of selection lists without removing it from OneStream. |
| **Is Parent** | Whether the member is treated as having children. | Correct or override how the member's hierarchy position is interpreted. |
| **Is Base** | Whether the member is treated as a base-level member. | Data is normally written at base members, so this affects where a member is valid to select. |

An override applies to how the Framework treats the member; it does not change anything in OneStream.

## Dimension Member Record Fields

| Field | Data Type| Purpose | Notes |
|---|---|---|---|
| Dimension | int | The dimension this member belongs to. | From OneStream. See [Dimension Types](../supporting/index.md).
| Name | nvarchar | Name of the member in OneStream. | From OneStream.
| Display Name | nvarchar | User-friendly display name. | From OneStream.
| Description | nvarchar | Description of the member. | From OneStream.
| Dimension Member | nvarchar | The dimension and member name combined. | Convenience column for selection and display.
| **Is Enabled** | bit | Whether the member is available for selection. | **Can be overridden.**
| **Is Parent** | bit | Whether the member is treated as having children. | **Can be overridden.**
| **Is Base** | bit | Whether the member is treated as a base-level member. | **Can be overridden.** Data is normally written at base members.
| Type Flags | int | Classification flags for the member. | From OneStream.
| Dimension Visibility Flags | int | Controls where the member's dimension is available. | From the dimension type.
| OneStream Dimension Type | int | The member's dimension type in OneStream. | Links the record to its OneStream counterpart.
| OneStream Member Id | int | The member's identifier in OneStream. | Links the record to its OneStream counterpart.
| OneStream Ext Ref Code | nvarchar | The member's external reference code in OneStream. |
| Ext. Ref System | int | The external system the reference code belongs to. |
| Integration Code | nvarchar | Unique value for the member record. | Used for importing data
| Created Date | datetime | Date and time the record was created. |
| Modified Date | datetime | Date and time the record was last modified. |
| Created By | int | User who created the member record. |
| Modified By | int | User who last modified the member record. |
| Dimension Member Id | int | Unique identifier for the member record. |

## Where Members are Used

Members are selected when configuring:

- [Workflow Unit Member Sets](../../workflow/units/memberSets.md) – base and scope members per dimension
- [Item Category Member Sets](../../workflow/supporting/itemCategoryMemberSets.md) – base and scope members per dimension
- [Workflow Areas](../../workflow/areas/index.md) – the area's data source member
- [Workflow Instances](../../workflow/instances/instances.md) – the cycle's scenario member

## Sync members from OneStream

1. Go to **Admin | Cube | Dimensions**
2. Click **Sync**

Syncing brings the current set of members across from OneStream. Run it after members are added, renamed, or restructured there.

## Notes

- A member missing from a selection list is usually either not yet synced, or disabled here.
- **Is Base** matters when choosing a base member for a member set — data is normally written at base members and bounded by scope members.
- Prefer fixing a member in OneStream where you can, so the two stay aligned; use the overrides for cases OneStream cannot express.
