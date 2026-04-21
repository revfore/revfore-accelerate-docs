# Relational View

A **Relational View** provides a **user-facing representation of data** based on a Relational Model.

It defines how data is presented, filtered, and interacted with in the application.

## Purpose

Relational Views are used to:

- present structured data to users
- support reporting and dashboards
- enable interaction with relational data
- serve as the primary interface between users and the data model

## Relational View Header Record Fields

The following fields are used for a Relational View header record.

| Field | Data Type | Purpose | Notes |
|---|---|---|---|
| Relational View Name | nvarchar | Internal name of the relational view. | This is the name of the actual SQL view in the database. No spaces or special characters are allowed. |
| Relational View Display Name | nvarchar | User-friendly display name shown in the application. | This will be auto-set if a Relational Model is selected and this field is blank or set to 'Auto()'  |
| Relational View Description | nvarchar | Description of the relational view and its purpose. | This will be auto-set if a Relational Model is selected and this field is blank or set to 'Auto()'|
| Relational Model | int | Identifies the relational model that the view is based on. | |
| Schema | int | Schema associated with the relational model. | This will be auto-set if a Relational Model is selected and this field is blank or set to 'Auto()'|
| Is RFA Managed | bit | Indicates whether the view is managed by Revfore Accelerate. |  This will be auto-set if a Relational Model is selected.  See Important Field Notes below|
| Is Custom | bit | Indicates whether the view is custom. | Almost all non-system views will be custom.  This will be auto-set if a Relational Model is selected.|
| Master View | int | Identifies the master view associated with this view. | Used when this view participates in a master-child view structure. |
| Parent View | int | Identifies the parent view associated with this view. | Used when this view is part of a parent-child hierarchy. |
| Child Level | int | Defines the child level of the view within a hierarchy. | Used when this view is part of a parent-child hierarchy. |
| Child Order | int | Defines the display or processing order of child views. | Used to control the order of related child views.  This field is not used at this time. |
| Is Enabled | bit | Indicates whether the view is enabled for use. | Disabled views are not intended for active use. |
| Group By Enabled | bit | Indicates whether grouping is enabled for the view. | Used when the view should support grouped or aggregated output. |
| Distinct Enabled | bit | Indicates whether distinct selection is enabled for the view. | Used when duplicate records should be eliminated from the view output.  This field is not used at this time. |
| Navigate To Enabled | bit | Indicates whether navigation is enabled for the view. | Used when users should be able to navigate from this view to related views or records. |
| Where Clause | nvarchar | Defines additional where clause logic for the view. | Used to restrict which records are included in the view.  Use the **Clause** button to open the expression editor page. |
| Having Clause | nvarchar | Defines additional having clause logic for the view. | Used to restrict grouped or aggregated results. Use the **Clause** button to open the expression editor page. |
| Ext Ref Unique Code | nvarchar | Unique value for the relational view record. | This is readonly and is auto set the same value as the View Name, providing a unique value for the record that is used for importing data. |
| Created Date | datetime | Date and time the record was created. | This is system maintained. |
| Modified Date | datetime | Date and time the record was last modified. | This is system maintained. |
| Created By | int | User who created the relational view record. | This is system maintained. |
| Modified By | int | User who last modified the relational view record. | This is system maintained. |
| Relational View Id | int | Unique identifier for the relational view record. | If you leave blank, the system will auto assign. |

!!!Note Important Field Notes
    **Is RFA Managed** - If Managed, the view definition is maintained in RFA and pushed to the database as a SQL view.  If not managed, the view is created directly in the database by some other method and RFA only references it.  Managed Views give you the full Relational View functionality.  Non-managed Views can only be used as Model Sources.  A good example of a non-managed view is a SQL view that is very complex and can only be created directly in SQL and is required for reporting & analytics.

## Key Concepts

- A view is built on top of a Relational Model
- Views determine how data is displayed and consumed
- Views can be reused across multiple pages, forms, and use cases

## Typical Use Cases

- Reporting and analysis views  
- Data entry and review interfaces  
- Dashboard and page integrations  
- Workflow-driven data interaction  

## Create a new Relational View

1. Go to **Admin | Relational Views**
2. Click on '**Add+**' or '**Inline Entry**'
3. Click on the '**+**' button on the top left of the grid
4. Enter required fields and click **Save**
5. Create new [Relational View Security](security.md)
6. Create new [Relational View Columns](columns.md)
7. Create new [Relational View Actions](actions.md)
8. Create new [Relational View Filters](filters.md)
9. Click on the **Sync** button to sync the view definition with the database

!!!Note Important Notes
    The Ext Ref Unique Code and Relational View Id will be auto-assigned

    The Name, Display Name, Description, Schema, Is RFA Managed and Is Custom fields will auto-populate if a Relational Model is selected and Saved.

    If Lookups need to be created for the underlying relation model, you will want to create a separate Lookup Relational View for it.  See [Tips for Lookup Views](index.md#tips-for-lookup-views)
    
    See [General Actions](../../../concepts/metadataDrivenUI/actions.md#general-actions) for more information about adding records

## Notes

- Views should be designed for clarity and usability
- Keep views focused on a specific purpose
- Views are often the primary way users interact with relational data