# Workflow Areas

[← Back to Admin](../../index.md)

Workflow Areas divide a workflow instance type into **distinct areas of work**.

An area groups a part of the process — capital, headcount, revenue, operating expense — so a cycle can be split into sections that are configured and completed separately.

## Overview

Use Workflow Areas to:

- divide a workflow instance type into separate areas of work
- set the cube each area's data belongs to
- set the data source dimension and member used for the area
- control whether saving data in the area triggers a cube update automatically

Each area belongs to one workflow instance type, so the same solution can present different areas for a Budget cycle than for a Forecast cycle.

## Workflow Area Record Fields

The following fields are used for a Workflow Area record.

| Field | Data Type| Purpose | Notes |
|---|---|---|---|
| Workflow Area Name | nvarchar | Internal name of the workflow area. | Must be unique. No spaces or special characters are allowed.
| Workflow Area Display Name | nvarchar | User-friendly display name shown in the application. | Must be unique.
| Workflow Area Description | nvarchar | Description of the workflow area and its purpose. |
| Workflow Instance Type | int | The instance type this area belongs to. | Required. Determines which kind of cycle the area appears in.
| Cube | int | The cube the area's data belongs to. | Leave blank where the area does not post to a cube.
| Data Source Dimension | int | The dimension the area's data source member is selected from. |
| Data Source Member | int | The data source dimension member used for the area's data. |
| Auto Trigger Update Of Cube | bit | Whether saving data in this area updates the cube automatically. | When off, the cube is updated by an explicit action instead.
| Is Enabled | bit | Indicates whether the workflow area is enabled for use. |
| Integration Code | nvarchar | Unique value for the workflow area record. | This is readonly and provides a unique value for the record that is used for importing data
| Created Date | datetime | Date and time the record was created. |
| Modified Date | datetime | Date and time the record was last modified. |
| Created By | int | User who created the workflow area record. |
| Modified By | int | User who last modified the workflow area record. |
| Workflow Area Id | int | Unique identifier for the workflow area record. | If you leave blank, the system will auto assign

## Typical Use Cases

- Capital expenditure
- Headcount and compensation
- Revenue planning
- Operating expense
- Allocations

## Create a new Workflow Area

1. Go to **Admin | Workflow | Areas**
2. Click on '**Add+**' or '**Add & Edit in Grid**'
3. Click on the '**+**' button on the top left of the grid
4. Select the Workflow Instance Type and enter the remaining required fields
5. Click **Save**
6. Assign the area to the [Workflow Item Types](../supporting/itemTypeAreas.md) that use it

**Add & Edit in Grid** allows adding and modifying rows directly in the grid

!!!Note Important Notes
    The Integration Code and Workflow Area Id will be auto-assigned

    See [General Actions](../../../../concepts/metadataDrivenUI/actions.md#general-actions) for more information about adding records

## Notes

- Areas are defined per instance type, so an area used by both Budget and Forecast needs a record for each.
- Turn on Auto Trigger Update Of Cube only where the data should reach the cube on every save; otherwise drive the update from an action.
- An area with no cube can still be used for work that stays entirely in relational tables.
