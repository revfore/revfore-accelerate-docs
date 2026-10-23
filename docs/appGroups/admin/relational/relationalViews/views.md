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
| Top Count | int | Shows at most this many rows - the first by the view's own sort. | Optional; blank shows all rows. Needs a sort on the view's columns. See [Top Count](#top-count). |
| Unit Records Shown | int | Which records the view shows, relative to the workflow unit looking at it. | Defaults to Own Records Only. See [Workflow behavior](#workflow-behavior). |
| Parent Unit Editability | int | What a Parent unit may do on this view. | Blank means read-only for a Parent. See [Workflow behavior](#workflow-behavior). |
| Business Rule Flags | int | Which assembly handles this view's save and delete logic, and in what order. | Defaults to the behavior that applied before the setting existed. See [Business Rule Flags](#business-rule-flags). |
| Where Clause | nvarchar | Defines additional where clause logic for the view. | Used to restrict which records are included in the view. Its **Flags** decide whether it is built into the view or applied per user - see [Where Clause](#where-clause). Use the **Clause** button to open the expression editor page. |
| Having Clause | nvarchar | Intended to restrict grouped or aggregated results. | **Not applied yet** - it is stored, but not written into the view, so it filters nothing. Filter on a model column with the Where Clause (Flags 5) instead, or rank with Top Count. |
| Integration Code | nvarchar | Unique value for the relational view record. | This is readonly and is auto set the same value as the View Name, providing a unique value for the record that is used for importing data. |
| Created Date | datetime | Date and time the record was created. | This is system maintained. |
| Modified Date | datetime | Date and time the record was last modified. | This is system maintained. |
| Created By | int | User who created the relational view record. | This is system maintained. |
| Modified By | int | User who last modified the relational view record. | This is system maintained. |
| Relational View Id | int | Unique identifier for the relational view record. | If you leave blank, the system will auto assign. |

!!!Note Important Field Notes
    **Is RFA Managed** - If Managed, the view definition is maintained in RFA and pushed to the database as a SQL view.  If not managed, the view is created directly in the database by some other method and RFA only references it.  Managed Views give you the full Relational View functionality.  Non-managed Views can only be used as Model Sources.  A good example of a non-managed view is a SQL view that is very complex and can only be created directly in SQL and is required for reporting & analytics.

## Workflow behavior

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

## Where Clause

A where clause can be applied at two different moments, and its **Flags** say which:

| Flags | Applied | `{Name}` refers to | Use it for |
|---|---|---|---|
| **1** (the default) | **When the view is built** - written into the SQL view | a column of this view | fixed filters on columns the view shows |
| **2** | **Every time the view is read** - the grid, lookups, Top Count and File Export | a column of this view | anything that depends on who is looking or which workflow they are in |
| **5** | **When the view is built** | a column of the view's **model** | fixed filters on columns the view does not show |

**Built into the view (1 and 5).** The clause is resolved once, when the view's SQL is created by Sync or Setup. There is no user and no workflow at that moment, so a clause that uses `{CurrentUserId}`, `{WfUnitId}`, `{WfInstanceYear}` or any other user or workflow value is refused on import - it would hold the value of whoever synced, for everyone. Use Flags 2 for those.

**Flags 5** filters on a model column without adding it to the view. That matters most on a summary view: every column there is either grouped or totalled, so a column added only to filter on would also have to be grouped by - which splits the totals if its values vary. With Flags 5 the view groups only by the columns you chose.

```json
{ "Clause": "{PrtMsr_Code} = {P1} and {PrtPln_IsInstanceYear} = 1",
  "Parameters": [ { "Name": "P1", "Type": "String", "Value": "Sales" } ],
  "Flags": 5 }
```

**Applied every time (2).** The clause is added to each query on the view, with the user's own workflow - so `{PrtPln_Year} = {WfInstanceYear}` shows each user the rows for the instance they are in. You can give a view one built-in clause and any number of run-time ones as a list; they are combined with AND.

**Other tables.** A clause can look at a table the view doesn't show, for example in an `exists` subquery. Name the table with a schema token, as import files do: `{RfaCore}.WfInstance`, `{RfaExtension}.PrtPlanLine` or `{OsCore}.XFC_Something`. The token resolves to the application's own schema, so the clause works in any environment. Never write the schema's name itself (`rfa01`).

```json
{ "Clause": "exists (select 1 from {RfaCore}.WfInstance_Unit iu where iu.WfInstanceId = {WfInstanceId} and iu.WfUnitId = {PrtPln_WfUnitId})",
  "Flags": 2 }
```

!!!Note Workflow columns still belong on the view
    A workflow-enabled view keeps its instance and unit columns, even if nobody looks at them: the Framework's own workflow filtering runs when the view is read, against those columns.

## Top Count

**Top Count** limits a view to its first rows — a top 10 by sales, the five largest variances. Which rows are "first" is decided by the view's own sort: the **Order By** sequence and direction on its [columns](columns.md). Sort descending on the value to get the highest.

The rows are chosen **after** every filter, workflow included, so each unit sees its own top 10 rather than whichever of the overall top 10 happen to be theirs. Everything on the screen uses the same rows:

- clicking a column heading re-sorts the top rows; it does not pick different ones
- the record count and the footer totals and averages cover only the rows shown
- **File Export** writes the same rows, to Excel or JSON, and a JSON import only updates rows among them

!!!Note A Top Count needs a sort
    Without one there is no "top", so set an Order By on the column that ranks the rows. Loading a view with a Top Count and no sorted column is refused.

## Business Rule Flags

**Business Rule Flags** decides which assembly handles the view's save and delete logic.

| Setting | Behavior |
|---|---|
| Default (by area) | What applied before this setting existed: core handles core views, the extension assembly handles extension views. |
| Core | Core logic only. |
| Extension | Extension logic only — it **replaces** the core behavior rather than adding to it. |
| Core, then Extension | Both, core first. |
| Extension, then Core | Both, extension first. |

The common reason to set it is to add validation to a **core** view without losing what the core already does — *Core, then Extension*. Choosing *Extension* on its own is how you deliberately take the core behavior out.

See [Writing Extension Code](../../../../extending/writing-code.md) for what runs where.

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