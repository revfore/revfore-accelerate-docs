# Units

[← Back to Workflow Units Overview](index.md)

The **Workflow Units** page is used to define and manage the header fields for a workflow unit.

Workflow units represent the entities a workflow process is performed for, such as a department, cost centre, or region.

## Overview

Use the Workflow Units page to:

- create new workflow unit definitions
- review existing workflow units
- manage unit-level settings such as effective dates and enabled state
- assign the security groups that control access to the unit's data

Each workflow unit should represent a clear business structure that work is divided by.

## Workflow Unit Header Record Fields

The following fields are used for a Workflow Unit header record.

| Field | Data Type| Purpose | Notes |
|---|---|---|---|
| Workflow Unit Name | nvarchar | Internal name of the workflow unit. | Must be unique. No spaces or special characters are allowed.
| Workflow Unit Display Name | nvarchar | User-friendly display name shown in the application. | Must be unique.
| Workflow Unit Description | nvarchar | Description of the workflow unit and its purpose. |
| Security Group | int | Security group granted read-write access to the unit's data. | Users in this group can view and edit data for this unit. Leave blank to grant no read-write access through the unit itself.
| Read Security Group | int | Security group granted read-only access to the unit's data. | Users in this group can view but not edit data for this unit.
| Effective Start Date | date | Date the workflow unit becomes available for use. | Defaults to 1900-01-01, meaning available from the beginning.
| Effective End Date | date | Date the workflow unit stops being available for use. | Defaults to 2999-12-31, meaning no end date. Set this rather than deleting a unit that is no longer in use.
| Is Enabled | bit | Indicates whether the workflow unit is enabled for use. |
| Ext Ref Unique Code | nvarchar | Unique value for the workflow unit record. | This is readonly and provides a unique value for the record that is used for importing data
| Created Date | datetime | Date and time the record was created. |
| Modified Date | datetime | Date and time the record was last modified. |
| Created By | int | User who created the workflow unit record. |
| Modified By | int | User who last modified the workflow unit record. |
| Workflow Unit Id | int | Unique identifier for the workflow unit record. | If you leave blank, the system will auto assign

## Typical Use Cases

Use workflow units to represent the structures a process is performed for, such as:

- Departments
- Cost centres
- Regions or territories
- Legal entities
- Business units

## Create a new Workflow Unit

1. Go to **Admin | Workflow | Units**
2. Click on '**Add+**' or '**Enable Inline Adding & Editing**'
3. Click on the '**+**' button on the top left of the grid
4. Enter required fields and click **Save**
5. Create the unit's [Member Sets](memberSets.md) for each workflow instance type it participates in

**Enable Inline Adding & Editing** allows adding and modifying rows directly in the grid

!!!Note Important Notes
    The Ext Ref Unique Code and Workflow Unit Id will be auto-assigned

    See [General Actions](../../../../concepts/metadataDrivenUI/actions.md#general-actions) for more information about adding records

## Notes

- Workflow Unit Name, Display Name and Ext Ref Unique Code must each be unique.
- Security groups here control access to the unit's data, and are separate from the security applied to relational views.
- A unit is only usable within its effective date range, so check these first if a unit is not appearing where expected.
