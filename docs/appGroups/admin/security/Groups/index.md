# Security Groups

[← Back to Admin](../../index.md)

Security Groups are **how access to data is granted**.

Users are not given access directly. They are placed in groups, and groups are assigned to the things that need securing — relational views, workflow units, and workflow instances.

!!! note "Read-only"
    Security groups come from OneStream and are refreshed by **Sync**. They cannot be added or edited here, and group membership is maintained in OneStream. To change a group, change it in OneStream and sync again.

## Overview

Use the Security Groups page to:

- see which groups are available to assign
- refresh the list after groups change in OneStream
- re-establish the link to a OneStream record where it has been lost

## Actions

| Action | What it does | Notes |
|---|---|---|
| **Sync** | Sync with OneStream Users and Security Groups. | Brings the current set of users and groups across from OneStream. Run it after groups are added, removed, or changed there.
| **Relink** | Relink to OneStream records. | Re-establishes the link between a Framework record and its OneStream counterpart, for cases where the two have become disconnected.

## Security Group Record Fields

The following fields are shown for a Security Group record. All are populated from OneStream.

| Field | Data Type| Purpose | Notes |
|---|---|---|---|
| Security Group Name | nvarchar | Internal name of the security group. | Must be unique.
| Security Group Description | nvarchar | Description of the group and what access it grants. | Required.
| Is Enabled | bit | Indicates whether the security group is enabled for use. |
| Integration Code | nvarchar | Unique value for the security group record. | This is readonly and provides a unique value for the record that is used for importing data
| Created Date | datetime | Date and time the record was created. |
| Modified Date | datetime | Date and time the record was last modified. |
| Created By | int | User who created the record. |
| Modified By | int | User who last modified the record. |
| Security Group Id | int | Unique identifier for the security group record. | If you leave blank, the system will auto assign

## Group Hierarchy and Membership

Two relationships sit alongside the group record, both maintained in OneStream:

- **Parent groups** – a group can be nested within another group, so access granted to a parent flows down to its children.
- **User membership** – users are assigned to groups, which is what actually grants them access.

Access through a nested hierarchy is resolved automatically: a user who is a member of a parent group has the access of its child groups, without needing to be added to each one.

## What the sync brings across

Sync pulls in the **group hierarchy** as well as the groups themselves, and flattens it so access can be checked with a simple join rather than by walking the hierarchy.

Both are visible on the **View+** screen:

| View | Holds | Shape |
|---|---|---|
| **Security Groups : Relationships** | The direct parent/child edges between groups. | One row per edge, as configured in OneStream. |
| **Security Groups : Relationships By Row** | Every ancestor/descendant pair, at any depth. | Flattened vertically: one row per pair, so nesting is already resolved. |

The **By Row** view is the one to join against when checking what a group reaches, and together with [Users : Security Groups By Row](../Users/index.md#what-the-sync-brings-across) it is what makes row-level user security straightforward - a view can be filtered per user with a simple join, because inherited access is already resolved into rows.

## Where Security Groups are Used

A security group is selected when configuring:

- [Relational Views](../../relational/relationalViews/security.md) – who can see and edit a view
- [Workflow Units](../../workflow/units/units.md) – read-write and read-only access to a unit's data
- [Workflow Instances](../../workflow/instances/instances.md) – read-write and read-only access to a cycle's data

Read-write and read-only access are granted through **separate groups**, so a user can be given visibility of data without the ability to change it.

## Sync security groups from OneStream

1. Go to **Admin | Security | Groups**
2. Click **Sync**

Syncing brings the current set of users and security groups across from OneStream.

## Notes

- Name groups after the access they grant rather than after the team that happens to hold it — a naming decision made in OneStream.
- Prefer a shallow hierarchy; deeply nested groups make effective access hard to reason about.
- A group missing here has usually been added in OneStream but not yet synced.
