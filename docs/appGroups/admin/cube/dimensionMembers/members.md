# Members

[← Back to Dimension Members Overview](index.md)

The **Dimension Members** page is used to define the individual members within each dimension.

A member is the specific value data is written at — a particular entity, account, or scenario.

## Overview

Use the Dimension Members page to:

- register the members within each dimension
- make members available for selection in workflow and cube configuration
- record each member's OneStream identifiers
- limit a member to a period of time using effective dates

## Dimension Member Record Fields

The following fields are used for a Dimension Member record.

| Field | Data Type| Purpose | Notes |
|---|---|---|---|
| Dimension | int | The dimension this member belongs to. | Required. See [Dimensions](dimensions.md).
| Member Name | nvarchar | Name of the member. | Should match the member name in OneStream.
| Member Display Name | nvarchar | User-friendly display name shown in the application. |
| Member Description | nvarchar | Description of the member. |
| OneStream Dimension Type | int | The OneStream dimension type the member belongs to. | Links the record to its OneStream counterpart.
| OneStream Member Id | int | The member's identifier in OneStream. | Links the record to its OneStream counterpart.
| Is Parent | bit | Indicates the member has children in the hierarchy. |
| Is Leaf | bit | Indicates the member is a base-level member with no children. | Data is normally written at leaf members and reported at parents.
| Effective Start Date | date | Date the member becomes available for selection. | Defaults to 1900-01-01.
| Effective End Date | date | Date the member stops being available for selection. | Defaults to 2999-12-31.
| Is Enabled | bit | Indicates whether the member is enabled for use. |
| Integration Code | nvarchar | Unique value for the member record. | This is readonly and provides a unique value for the record that is used for importing data
| Created Date | datetime | Date and time the record was created. |
| Modified Date | datetime | Date and time the record was last modified. |
| Created By | int | User who created the member record. |
| Modified By | int | User who last modified the member record. |
| Dimension Member Id | int | Unique identifier for the member record. | If you leave blank, the system will auto assign

## Where Members are Used

Members are selected when configuring:

- [Workflow Unit Member Sets](../../workflow/units/memberSets.md) – base and scope members per dimension
- [Item Category Member Sets](../../workflow/supporting/itemCategoryMemberSets.md) – base and scope members per dimension
- [Workflow Areas](../../workflow/areas/index.md) – the area's data source member
- [Workflow Instances](../../workflow/instances/instances.md) – the cycle's scenario member

## Create a new Dimension Member

1. Go to **Admin | Cube | Dimension Members**
2. Open the **Members** page
3. Click on '**Add+**' or '**Add & Edit in Grid**'
4. Select the Dimension and enter the remaining required fields
5. Click **Save**

**Add & Edit in Grid** allows adding and modifying rows directly in the grid

!!!Note Important Notes
    The Integration Code and Dimension Member Id will be auto-assigned

    See [General Actions](../../../../concepts/metadataDrivenUI/actions.md#general-actions) for more information about adding records

## Notes

- Data is normally written at leaf members and reported at parent members — check Is Leaf when choosing a base member.
- Member names should match OneStream exactly, since the name is how the member is identified when data is synced.
- Use effective dates rather than deleting members that are no longer used, so historical data stays valid.
