# Cube Views

[← Back to Relational Supporting Overview](index.md)

The **Cube Views** page defines the view records that surface relational data in dashboards.

A cube view links a relational object to a view definition that can be presented in the application, so data defined in the relational model can be displayed to users.

## Overview

Use the Cube Views page to:

- review the view records available for presentation
- see which relational object each one surfaces
- set the view type
- enable or disable a view without removing it

## Cube View Record Fields

The following fields are used for a Cube View record.

| Field | Data Type| Purpose | Notes |
|---|---|---|---|
| View Name | nvarchar | Internal name of the view. | No spaces or special characters are allowed.
| View Display Name | nvarchar | User-friendly display name shown in the application. |
| View Description | nvarchar | Description of the view and its purpose. |
| Type | int | The kind of view. | Determines how the view is presented.
| Relational Object | int | The relational object this view surfaces. | Links the view to the relational view or table it presents.
| View Ext Ref Code | nvarchar | External reference code for the view. |
| View Ext Ref System | int | The external system the reference code belongs to. |
| Is Enabled | bit | Indicates whether the view is enabled for use. |
| Ext Ref Unique Code | nvarchar | Unique value for the view record. | This is readonly and provides a unique value for the record that is used for importing data
| Created Date | datetime | Date and time the record was created. |
| Modified Date | datetime | Date and time the record was last modified. |
| Created By | int | User who created the view record. |
| Modified By | int | User who last modified the view record. |
| View Id | int | Unique identifier for the view record. | If you leave blank, the system will auto assign

## Typical Use Cases

- Surfacing a relational view in a dashboard
- Presenting the same relational data in more than one way
- Disabling a view that is no longer used without deleting its definition

## Create a new Cube View

1. Go to **Admin | Relational | Supporting**
2. Open the **Cube Views** page
3. Click on '**Add+**' or '**Enable Inline Adding & Editing**'
4. Enter the name and select the Relational Object and Type
5. Click **Save**

**Enable Inline Adding & Editing** allows adding and modifying rows directly in the grid

!!!Note Important Notes
    The Ext Ref Unique Code and View Id will be auto-assigned

    See [General Actions](../../../../concepts/metadataDrivenUI/actions.md#general-actions) for more information about adding records

## Notes

- A cube view presents an existing relational object — define the [relational view](../relationalViews/index.md) first.
- Disabling a view removes it from presentation but leaves the definition in place.
