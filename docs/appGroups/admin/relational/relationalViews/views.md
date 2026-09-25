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
| Is RFA Managed | bit | Indicates whether the view is managed by Revfore Framework. |  This will be auto-set if a Relational Model is selected.  See Important Field Notes below|
| Is Extension | bit | Indicates whether the view is an extension. | Almost all non-system views will be extensions.  This will be auto-set if a Relational Model is selected.|
| Master View | int | Identifies the master view associated with this view. | Used when this view participates in a master-child view structure. |
| Parent View | int | Identifies the parent view associated with this view. | Used when this view is part of a parent-child hierarchy. |
| Child Level | int | Defines the child level of the view within a hierarchy. | Used when this view is part of a parent-child hierarchy. |
| Child Order | int | Defines the display or processing order of child views. | Used to control the order of related child views.  This field is not used at this time. |
| Is Enabled | bit | Indicates whether the view is enabled for use. | Disabled views are not intended for active use. |
| Group By Enabled | bit | Indicates whether grouping is enabled for the view. | Used when the view should support grouped or aggregated output. |
| Distinct Enabled | bit | Indicates whether distinct selection is enabled for the view. | Used when duplicate records should be eliminated from the view output.  This field is not used at this time. |
| Navigate To Enabled | bit | Indicates whether navigation is enabled for the view. | Used when users should be able to navigate from this view to related views or records. |
| Unit Records Shown | int | Which records the view shows, relative to the workflow unit looking at it. | Defaults to Own Records Only. See [Workflow behaviour](#workflow-behaviour). |
| Parent Unit Editability | int | What a Parent unit may do on this view. | Blank means read-only for a Parent. See [Workflow behaviour](#workflow-behaviour). |
| Business Rule Flags | int | Which assembly handles this view's save and delete logic, and in what order. | Defaults to the behaviour that applied before the setting existed. See [Business Rule Flags](#business-rule-flags). |
| Where Clause | nvarchar | Defines additional where clause logic for the view. | Used to restrict which records are included in the view.  Use the **Clause** button to open the expression editor page. |
| Having Clause | nvarchar | Defines additional having clause logic for the view. | Used to restrict grouped or aggregated results. Use the **Clause** button to open the expression editor page. |
| Integration Code | nvarchar | Unique value for the relational view record. | This is readonly and is auto set the same value as the View Name, providing a unique value for the record that is used for importing data. |
| Created Date | datetime | Date and time the record was created. | This is system maintained. |
| Modified Date | datetime | Date and time the record was last modified. | This is system maintained. |
| Created By | int | User who created the relational view record. | This is system maintained. |
| Modified By | int | User who last modified the relational view record. | This is system maintained. |
| Relational View Id | int | Unique identifier for the relational view record. | If you leave blank, the system will auto assign. |

!!!Note Important Field Notes
    **Is RFA Managed** - If Managed, the view definition is maintained in RFA and pushed to the database as a SQL view.  If not managed, the view is created directly in the database by some other method and RFA only references it.  Managed Views give you the full Relational View functionality.  Non-managed Views can only be used as Model Sources.  A good example of a non-managed view is a SQL view that is very complex and can only be created directly in SQL and is required for reporting & analytics.

## Workflow behaviour

Two settings decide how a workflow-enabled view behaves for the unit looking at it. Both live on the view rather than on the unit, because the same unit is usually treated differently on a data entry screen and on the review screen next to it.

### Unit Records Shown

Which rows the view returns, relative to the current workflow unit.

| Setting | The view shows |
|---|---|
| Own Records Only (default) | The current unit's own rows, whatever kind of unit it is. |
| Base Records | The unit's own rows, when it is a **Base** unit. |
| Parent Records | The unit's own rows, when it is a **Parent** unit. |
| Descendant Records | Rows belonging to the units **below** it in the [unit hierarchy](../../workflow/units/hierarchies.md). |
| Base & Descendant Records | Both of the above. |
| Parent & Descendant Records | Both of the above. |
| Base, Parent & Descendant Records | Everything the unit can reach. |

Own Records Only is what every view did before this setting existed, so leaving it alone changes nothing.

The combinations exist because Base and Parent units usually want different screens. A submission grid might show Base records only; the review grid beside it shows Parent and Descendant records, so a reviewer sees their own rows together with everything underneath them.

Note that **Parent Records** means a Parent unit's *own* rows, not the rows of the units below it. A Parent unit can hold data like any other — the two are separate settings precisely so a view can show one, the other, or both.

!!!Note
    Descendant records are resolved through the unit hierarchy that governs the current cycle. That hierarchy has to exist, have been [processed](../../workflow/units/hierarchies.md#process), and be named on the [instance type's Time](../../workflow/instances/instanceTypes.md#time-which-hierarchy-applies) grid for the cycle's start date — otherwise there is nothing to resolve and no descendant rows appear.

### Parent Unit Editability

What a **Parent** unit is allowed to do here, using the same choices as Editability Mode.

- **Left blank**, a Parent unit gets read-only. This is the safe default: a parent looking across other units' data is reviewing it, not typing into it.
- **Set**, it *replaces* Editability Mode for a Parent unit — it does not narrow it. A view that is read-only for everyone can still be editable for a Parent, which is how a review screen lets an approver record a decision on a grid nobody else can change.

The workflow status check still applies on top of either. A closed cycle is read-only for a Parent as much as for anyone else.

### The unit column

Where a view shows more than one unit's rows, the workflow unit column appears by itself. Nothing needs configuring — it follows from Unit Records Shown including descendants and the current unit being a Parent.

On every other view it stays hidden, because every row belongs to the same unit and the column would repeat a single value down the page.

## Business Rule Flags

**Business Rule Flags** decides which assembly handles the view's save and delete logic.

| Setting | Behaviour |
|---|---|
| Default (by area) | What applied before this setting existed: core handles core views, the extension assembly handles extension views. |
| Core | Core logic only. |
| Extension | Extension logic only — it **replaces** the core behaviour rather than adding to it. |
| Core, then Extension | Both, core first. |
| Extension, then Core | Both, extension first. |

The common reason to set it is to add validation to a **core** view without losing what the core already does — *Core, then Extension*. Choosing *Extension* on its own is how you deliberately take the core behaviour out.

See [How Dispatch Works](../../../../extending/handlers/index.md) for what runs where.

## Key Concepts

- A view is built on top of a Relational Model
- Views determine how data is displayed and consumed
- Views can be reused across multiple pages, forms, and use cases

## Typical Use Cases

- Reporting and analysis views  
- Data entry and review interfaces  
- Dashboard and page integrations  
- Workflow-driven data interaction  
- Review and approval screens that look across the units beneath a parent  

## Create a new Relational View

1. Go to **Admin | Relational | Relational Views**
2. Click on '**Add+**' or '**Add & Edit in Grid**'
3. Click on the '**+**' button on the top left of the grid
4. Enter required fields and click **Save**
5. Create new [Relational View Security](security.md)
6. Create new [Relational View Columns](columns.md)
7. Create new [Relational View Actions](actions.md)
8. Create new [Relational View Filters](filters.md)
9. Click on the **Sync** button to sync the view definition with the database

**Add & Edit in Grid** allows adding and modifying rows directly in the grid

!!!Note Important Notes
    The Integration Code and Relational View Id will be auto-assigned

    The Name, Display Name, Description, Schema, Is RFA Managed and Is Extension fields will auto-populate if a Relational Model is selected and Saved.

    If Lookups need to be created for the underlying relation model, you will want to create a separate Lookup Relational View for it.  See [Tips for Lookup Views](index.md#tips-for-lookup-views)
    
    See [General Actions](../../../../concepts/metadataDrivenUI/actions.md#general-actions) for more information about adding records

## Notes

- Views should be designed for clarity and usability
- Keep views focused on a specific purpose
- Views are often the primary way users interact with relational data