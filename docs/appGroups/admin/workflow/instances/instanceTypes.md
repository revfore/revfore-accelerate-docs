# Instance Types

[← Back to Workflow Instances Overview](index.md)

The **Workflow Instance Types** page is used to define and manage the kinds of workflow cycle available.

An instance type is the reusable definition — Budget, Forecast, Close — that individual workflow instances are created from.

## Overview

Use the Workflow Instance Types page to:

- define the kinds of cycle the solution supports
- review existing instance types
- enable or disable an instance type

The instance type is more than a label. Workflow units, workflow areas, and member sets are all configured **per instance type**, so it determines which configuration applies when a cycle runs.

## Workflow Instance Type Record Fields

The following fields are used for a Workflow Instance Type record.

| Field | Data Type| Purpose | Notes |
|---|---|---|---|
| Workflow Instance Type Name | nvarchar | Internal name of the workflow instance type. | Must be unique. No spaces or special characters are allowed.
| Workflow Instance Type Display Name | nvarchar | User-friendly display name shown in the application. | Must be unique.
| Workflow Instance Type Description | nvarchar | Description of the instance type and its purpose. |
| Is Enabled | bit | Indicates whether the instance type is enabled for use. |
| Ext Ref Unique Code | nvarchar | Unique value for the instance type record. | This is readonly and provides a unique value for the record that is used for importing data
| Created Date | datetime | Date and time the record was created. |
| Modified Date | datetime | Date and time the record was last modified. |
| Created By | int | User who created the instance type record. |
| Modified By | int | User who last modified the instance type record. |
| Workflow Instance Type Id | int | Unique identifier for the instance type record. | If you leave blank, the system will auto assign

## Typical Use Cases

- Budget
- Forecast
- Close
- Long-range plan

## Create a new Workflow Instance Type

1. Go to **Admin | Workflow | Instances**
2. Open the **Instance Types** page
3. Click on '**Add+**' or '**Enable Inline Adding & Editing**'
4. Enter required fields and click **Save**
5. Configure the [Workflow Units](../units/memberSets.md) and [Workflow Areas](../areas/index.md) that apply to this instance type

**Enable Inline Adding & Editing** allows adding and modifying rows directly in the grid

!!!Note Important Notes
    The Ext Ref Unique Code and Workflow Instance Type Id will be auto-assigned

    See [General Actions](../../../../concepts/metadataDrivenUI/actions.md#general-actions) for more information about adding records

## Notes

- Keep the number of instance types small — one per genuinely different kind of process, not one per cycle.
- Changing an instance type after cycles exist affects which unit member sets and areas apply, so review the configuration before making changes.
- A `Default` instance type is supplied with the platform and can be used where a solution needs only one kind of cycle.
