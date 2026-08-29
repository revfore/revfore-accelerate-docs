# Security Groups

[← Back to Admin](../../index.md)

Security Groups are **how access to data is granted**.

Users are not given access directly. They are placed in groups, and groups are assigned to the things that need securing — relational views, workflow units, and workflow instances.

## Overview

Use Security Groups to:

- define the groups the solution uses to control access
- build a hierarchy by nesting groups within parent groups
- assign users to the groups that grant them access
- enable or disable a group without removing it

## Security Group Record Fields

The following fields are used for a Security Group record.

| Field | Data Type| Purpose | Notes |
|---|---|---|---|
| Security Group Name | nvarchar | Internal name of the security group. | Must be unique.
| Security Group Description | nvarchar | Description of the group and what access it grants. | Required.
| Is Enabled | bit | Indicates whether the security group is enabled for use. |
| Ext Ref Unique Code | nvarchar | Unique value for the security group record. | This is readonly and provides a unique value for the record that is used for importing data
| Created Date | datetime | Date and time the record was created. |
| Modified Date | datetime | Date and time the record was last modified. |
| Created By | int | User who created the record. |
| Modified By | int | User who last modified the record. |
| Security Group Id | int | Unique identifier for the security group record. | If you leave blank, the system will auto assign

## Group Hierarchy and Membership

Two relationships sit alongside the group record:

- **Parent groups** – a group can be nested within another group, so access granted to a parent flows down to its children.
- **User membership** – users are assigned to groups, which is what actually grants them access.

Access through a nested hierarchy is resolved automatically: a user who is a member of a parent group has the access of its child groups, without needing to be added to each one.

## Where Security Groups are Used

A security group is selected when configuring:

- [Relational Views](../../relational/relationalViews/security.md) – who can see and edit a view
- [Workflow Units](../../workflow/units/units.md) – read-write and read-only access to a unit's data
- [Workflow Instances](../../workflow/instances/instances.md) – read-write and read-only access to a cycle's data

Read-write and read-only access are granted through **separate groups**, so a user can be given visibility of data without the ability to change it.

## Create a new Security Group

1. Go to **Admin | Security | Groups**
2. Click on '**Add+**' or '**Enable Inline Adding & Editing**'
3. Click on the '**+**' button on the top left of the grid
4. Enter the name and description, then click **Save**
5. Assign [Users](../Users/index.md) to the group
6. Assign the group where access is needed, such as on a view, unit or instance

**Enable Inline Adding & Editing** allows adding and modifying rows directly in the grid

!!!Note Important Notes
    The Ext Ref Unique Code and Security Group Id will be auto-assigned

    See [General Actions](../../../../concepts/metadataDrivenUI/actions.md#general-actions) for more information about adding records

## Notes

- Name groups after the access they grant rather than after the team that happens to hold it.
- Prefer a shallow hierarchy; deeply nested groups make effective access hard to reason about.
- Description is required on a security group — use it to record what the group is for.
