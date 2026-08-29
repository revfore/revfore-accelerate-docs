# Users

[← Back to Admin](../../index.md)

Users are the **people who use the solution**.

A user record identifies an individual and carries the settings that control how they access the application and what they are licensed for.

## Overview

Use Users to:

- register the people who use the solution
- control which application modes a user can work in
- limit access to a period of time using effective dates
- enable or disable a user without removing them

Access to data is granted through [Security Groups](../Groups/index.md) rather than directly on the user record.

## User Record Fields

The following fields are used for a User record.

| Field | Data Type| Purpose | Notes |
|---|---|---|---|
| User Number | nvarchar | Identifying number for the user. | Required.
| User Name | nvarchar | Name of the user. | Required.
| User Description | nvarchar | Description or additional detail about the user. |
| Person | int | The person record this user is associated with. |
| License Type | int | The licence the user is assigned. | Determines what the user is entitled to use.
| Default Application Mode | int | The mode the application opens in for this user. |
| Application Mode Flags | int | The modes this user is permitted to work in. |
| Comments | nvarchar | Free-text comments about the user. |
| Effective Start Date | date | Date the user becomes active. | Defaults to 1900-01-01.
| Effective End Date | date | Date the user stops being active. | Defaults to 2999-12-31. Use this rather than deleting a user who has left.
| Is Enabled | bit | Indicates whether the user is enabled. |
| Ext Ref Unique Code | nvarchar | Unique value for the user record. | This is readonly and provides a unique value for the record that is used for importing data
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

## Create a new User

1. Go to **Admin | Security | Users**
2. Click on '**Add+**' or '**Enable Inline Adding & Editing**'
3. Click on the '**+**' button on the top left of the grid
4. Enter required fields and click **Save**
5. Add the user to the [Security Groups](../Groups/index.md) that grant the access they need

**Enable Inline Adding & Editing** allows adding and modifying rows directly in the grid

!!!Note Important Notes
    The Ext Ref Unique Code and User Id will be auto-assigned

    See [General Actions](../../../../concepts/metadataDrivenUI/actions.md#general-actions) for more information about adding records

## Notes

- A user with no security group membership can sign in but will not see data that is secured.
- Use effective dates to retire a user rather than deleting the record, so the audit trail on existing data stays intact.
