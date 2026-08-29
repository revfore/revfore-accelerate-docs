# Dimensions

[← Back to Dimension Members Overview](index.md)

The **Dimensions** page is used to define the dimension types the solution uses.

A dimension groups the members data can be written at — Entity, Account, Scenario, Flow, IC, and the user-defined dimensions.

## Overview

Use the Dimensions page to:

- register the dimensions the solution uses
- review the dimensions already available
- control how a dimension is classified and where it is visible

## Dimension Record Fields

The following fields are used for a Dimension record.

| Field | Data Type| Purpose | Notes |
|---|---|---|---|
| Dimension Name | nvarchar | Name of the dimension. | Should match the dimension name in OneStream. Must be unique.
| Dimension Display Name | nvarchar | User-friendly display name shown in the application. |
| Dimension Description | nvarchar | Description of the dimension and its purpose. |
| Type Flag | int | Classifies the kind of dimension. | Identifies which OneStream dimension type the record represents.
| Visibility Flags | int | Controls where the dimension is available for selection. |
| Ext Ref Unique Code | nvarchar | Unique value for the dimension record. | This is readonly and provides a unique value for the record that is used for importing data
| Created Date | datetime | Date and time the record was created. |
| Modified Date | datetime | Date and time the record was last modified. |
| Created By | int | User who created the dimension record. |
| Modified By | int | User who last modified the dimension record. |
| Dimension Id | int | Unique identifier for the dimension record. | If you leave blank, the system will auto assign

## Typical Use Cases

- Entity
- Account
- Scenario
- Flow
- Intercompany
- User-defined dimensions (UD1 through UD8)

## Create a new Dimension

1. Go to **Admin | Cube | Dimension Members**
2. Open the **Dimensions** page
3. Click on '**Add+**' or '**Enable Inline Adding & Editing**'
4. Enter required fields and click **Save**
5. Create the [Members](members.md) that belong to the dimension

**Enable Inline Adding & Editing** allows adding and modifying rows directly in the grid

!!!Note Important Notes
    The Ext Ref Unique Code and Dimension Id will be auto-assigned

    See [General Actions](../../../../concepts/metadataDrivenUI/actions.md#general-actions) for more information about adding records

## Notes

- A dimension must exist before its members can be created.
- Dimension names should match OneStream exactly.
- Register only the dimensions the solution maps data to.
