# Member Set Types

[← Back to Workflow Supporting Overview](index.md)

The **Workflow Member Set Types** page is used to define the kinds of dimension-member assignment available.

A member set type classifies what a member set represents — for example whether it selects members at base level or at summary level.

## Overview

Use the Workflow Member Set Types page to:

- define the kinds of member set assignment the solution uses
- review existing member set types
- set the data source member associated with a type
- limit a type to a period of time using effective dates

## Workflow Member Set Type Record Fields

The following fields are used for a Workflow Member Set Type record.

| Field | Data Type| Purpose | Notes |
|---|---|---|---|
| Workflow Member Set Type Name | nvarchar | Internal name of the member set type. | Must be unique. No spaces or special characters are allowed.
| Workflow Member Set Type Display Name | nvarchar | User-friendly display name shown in the application. | Must be unique.
| Workflow Member Set Type Description | nvarchar | Description of the member set type and its purpose. |
| Number Prefix | nvarchar | Prefix applied to numbers generated for this type. |
| Data Source Member | int | The data source dimension member associated with this type. |
| Effective Start Date | date | Date the member set type becomes available. | Defaults to 1900-01-01.
| Effective End Date | date | Date the member set type stops being available. | Defaults to 2999-12-31.
| Is Enabled | bit | Indicates whether the member set type is enabled for use. |
| Ext Ref Unique Code | nvarchar | Unique value for the member set type record. | This is readonly and provides a unique value for the record that is used for importing data
| Created Date | datetime | Date and time the record was created. |
| Modified Date | datetime | Date and time the record was last modified. |
| Created By | int | User who created the member set type record. |
| Modified By | int | User who last modified the member set type record. |
| Workflow Member Set Type Id | int | Unique identifier for the member set type record. | If you leave blank, the system will auto assign

## Create a new Workflow Member Set Type

1. Go to **Admin | Workflow | Supporting**
2. Open the **Member Set Types** page
3. Click on '**Add+**' or '**Enable Inline Adding & Editing**'
4. Enter required fields and click **Save**

**Enable Inline Adding & Editing** allows adding and modifying rows directly in the grid

!!!Note Important Notes
    The Ext Ref Unique Code and Workflow Member Set Type Id will be auto-assigned

    See [General Actions](../../../../concepts/metadataDrivenUI/actions.md#general-actions) for more information about adding records

## Notes

- Member set types are referenced by [Item Category Member Sets](itemCategoryMemberSets.md), so create them first.
- Keep the set small — these classify assignments, they are not the assignments themselves.
