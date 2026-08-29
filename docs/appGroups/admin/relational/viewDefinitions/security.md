# View Security

[← Back to Relational Views Overview](index.md)

The **View Security** page is used to define and manage data access security for a Relational View.

Security controls which users or groups can read data and which users or groups can both read and update data within the view.

## Overview

Use the Security page to:

- define read access for a relational view
- define read and write access for a relational view
- control which users can view data
- control which users can update data in the view

## Relational View Security Fields

The following fields are used for a Relational View Security record.

| Field | Data Type | Purpose | Notes |
|---|---|---|---|
| Relational View Name | nvarchar | Internal name of the relational view. | This is the name of the actual SQL view in the database. No spaces or special characters are allowed. |
| Relational View Display Name | nvarchar | User-friendly display name shown in the application. | |
| Read Data Group | uniqueidentifier | Identifies the security group that has read-only access to the view data. | Users in this group can view the data but cannot update it. |
| Read and Write Data Group | uniqueidentifier | Identifies the security group that has both read and write access to the view data. | Users in this group can view and update the data, depending on the view configuration. |
| Ext Ref Unique Code | nvarchar | Unique value for the relational view security record. | This is readonly and is typically auto set to provide a stable unique value for importing data. |
| Created Date | datetime | Date and time the record was created. | This is system maintained. |
| Modified Date | datetime | Date and time the record was last modified. | This is system maintained. |
| Created By | int | User who created the relational view security record. | This is system maintained. |
| Modified By | int | User who last modified the relational view security record. | This is system maintained. |
| Relational View Id | int | Unique identifier for the relational view record. | This links the security settings back to the associated relational view. |

## Typical Use Cases

Examples include assigning:

- a read-only group for reporting users
- a read and write group for administrators or data stewards
- separate access groups for review users and maintenance users
- controlled edit access for operational teams

## Design Guidance

When defining security:

- assign read-only access to users who only need to review data
- assign read and write access only to users who need to maintain data
- keep group assignments clear and consistent
- align security with the intended use of the view

## Configure Relational View Security

1. Go to **Admin | Relational Views**
2. Select the **Security** page
3. Select the desired Relational View
4. Click on '**Edit**' or '**Enable Inline Adding & Editing**' 
5. Make any needed changes and click **Save**

**Enable Inline Adding & Editing** allows adding and modifying rows directly in the grid

## Notes

- Security should be reviewed carefully for any view that allows updates.
- Read-only and read/write access should be separated where appropriate.
- Security settings help ensure that users only have the level of access required for their role.
- A user’s ability to update data also depends on whether the view and its columns are configured to allow updates.