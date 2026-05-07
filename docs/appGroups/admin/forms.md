# Forms

[← Back to Admin](index.md)

Forms provide **user-facing interfaces** for entering, editing, and managing data.

They allow users to interact with relational data in a structured and intuitive way.

Custom forms can be selected on [Custom View Actions](../admin/relationalViews/actions.md) and custom code can be added to a custom assembly file to handle custom logic.

## Overview

Use Forms to:

- capture user input
- edit relational data
- create guided data entry experiences
- interact with Relational Tables and Views

## Form Fields

The following fields are used for a Form record.

| Field | Data Type | Purpose | Notes |
|---|---|---|---|
| Form Name | nvarchar | Name of the form. | Spaces are allowed.  This typically matches a dashboard name selected in the 'Dashboard - OneStream' field. |
| Form Description | nvarchar | Description of the form and its purpose. | Use this to document how the form should be used. |
| Form Type | int | Defines the type of form. | Use the 'Dashboard' type.  It is the only one supported right now |
| Dashboard - OneStream | nvarchar | Identifies the associated OneStream dashboard. | Used when the form is tied to a OneStream dashboard-based experience. |
| Ext Ref Unique Code | nvarchar | Unique value for the form record. | This is auto set to Form Name to provide a stable unique value for importing data. Can be changed. |
| Created Date | datetime | Date and time the record was created. | This is system maintained. |
| Modified Date | datetime | Date and time the record was last modified. | This is system maintained. |
| Created By | int | User who created the form record. | This is system maintained. |
| Modified By | int | User who last modified the form record. | This is system maintained. |
| Form | int | Unique identifier for the form record. | If left blank, the system will typically auto assign it. |

## Key Concepts

- Forms are built on top of Relational Tables and Views
- Forms provide structured input interfaces
- Forms can include validation and controlled inputs

## Typical Use Cases

- Data entry for planning models
- Managing operational data
- Updating relational records
- Guided workflows for users

## Create a new Form

1. Go to **Admin | Forms**
2. Click on '**Add**' or '**Enable Inline Adding & Editing**'
3. Click on the '**+**' button on the top left of the grid
4. Enter required fields and click **Save**

**Enable Inline Adding & Editing** allows adding and modifying rows directly in the grid

## Notes

- Forms should be designed for ease of use
- Use Lookups to control input values
- Keep forms focused on a specific task or entity