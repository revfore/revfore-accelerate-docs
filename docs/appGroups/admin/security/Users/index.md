# Users

[← Back to Admin](../../index.md)

Users are the **people who use the solution**.

A user record identifies an individual and carries the settings that control how they access the application and what they are licensed for.

!!! note "Read-only"
    Users come from OneStream and are refreshed by **Sync**. They cannot be added or edited here. To change a user, change it in OneStream and sync again.

## Overview

Use the Users page to:

- see which users the Framework knows about
- refresh the list after users change in OneStream
- re-establish the link to a OneStream record where it has been lost

Access to data is granted through [Security Groups](../Groups/index.md) rather than directly on the user record.

## Actions

| Action | What it does | Notes |
|---|---|---|
| **Sync** | Sync with OneStream Users and Security Groups. | Brings the current set of users and groups across from OneStream. Run it after users are added, removed, or changed there.
| **Relink** | Relink to OneStream records. | Re-establishes the link between a Framework record and its OneStream counterpart, for cases where the two have become disconnected.

## What the sync brings across

Sync does more than list users. It also pulls in each user's **security group assignments**, and flattens them so access can be checked with a simple join rather than by walking the group hierarchy.

Both are visible on the **View+** screen:

| View | Holds | Shape |
|---|---|---|
| **Users : Security Groups** | The groups a user is assigned to directly. | One row per direct assignment, as configured in OneStream. |
| **Users : Security Groups By Row** | Every group a user has access through — direct assignments **and** those inherited through the group hierarchy. | Flattened vertically: one row per user and group, at any depth. |

**This is what makes row-level user security straightforward.** Because inheritance is already resolved into rows, a view can be filtered per user with a simple join to the By Row view — no recursion through parent groups, and no working out inherited access at query time. A user sees only the rows their groups grant them.

## User Record Fields

The following fields are shown for a User record. All are populated from OneStream.

| Field | Data Type| Purpose | Notes |
|---|---|---|---|
| User Number | nvarchar | Identifying number for the user. | Required.
| User Name | nvarchar | Name of the user. | Required.
| User Description | nvarchar | Description or additional detail about the user. |
| Comments | nvarchar | Free-text comments about the user. |
| Effective Start Date | date | Date the user becomes active. | Defaults to 1900-01-01.
| Effective End Date | date | Date the user stops being active. | Defaults to 2999-12-31. Use this rather than deleting a user who has left.
| Is Enabled | bit | Indicates whether the user is enabled. |
| Integration Code | nvarchar | Unique value for the user record. | This is readonly and provides a unique value for the record that is used for importing data
| Created Date | datetime | Date and time the record was created. |
| Modified Date | datetime | Date and time the record was last modified. |
| Created By | int | User who created the record. |
| Modified By | int | User who last modified the record. |
| User Id | int | Unique identifier for the user record. | If you leave blank, the system will auto assign

## Where Users are Used

The user record is referenced throughout the solution:

- as the Created By and Modified By on every record
- as the owner, approver or other business contact on solution tables
- as a member of one or more [Security Groups](../Groups/index.md), which is what grants access to data

## Sync users from OneStream

1. Go to **Admin | Security | Users**
2. Click **Sync**

Syncing brings the current set of users and security groups across from OneStream.

## Notes

- A user with no security group membership can sign in but will not see data that is secured.
- A user missing here has usually been added in OneStream but not yet synced.
- Because users mirror OneStream, retire a user there rather than trying to remove them here.
