# Relational Views

Relational Views define how relational model data is exposed for user interaction, reporting, and analysis in Revfore Accelerate.

They are built on top of Relational Models and are used to create reusable **SQL views** in the database that can then be consumed by the application.

* * *

## What is a Relational View?

A Relational View is a configurable representation of data based on a Relational Model.  It can represent all columns in the model or a subset.

It determines what data is exposed, how it is labeled, what actions are available, and how users can filter or interact with the data.

Each Relational View creates a corresponding **SQL view** in the database.

Examples of common views:

- Workforce Planning View
- Asset Maintenance View
- Product Analysis View
- Customer Transaction View

Each view is designed around a specific user experience or reporting need.

* * *

## Types of Relational Views

Relational Views can support different types of use cases depending on the type of model they are built on.

The standard types of views are:

- Data Entry (_mE)
- Lookup (_mL)
- Summary (_mS)
- General (_mG)

### Data Entry Views (_mE)

Data entry views are built on models centered around a **single primary table** along with related foreign key tables.

These views are commonly used for:

- data entry
- record maintenance
- operational workflows
- reporting on a primary business entity

Examples include:

- an Employee view that allows users to maintain employee records while showing related department and location information
- an Asset view that allows users to manage asset setup data while showing related type or location details

These views are typically focused on a primary entity and are often used in editable or user-managed experiences.

These require a Primary Key, Name and Ext Ref Unique Code column

These require Actions to be defined

### Lookup Views (_mL)

Lookup views are used exclusively on Lookup records are of type 'Relational View'

These require a Primary Key, Name and Ext Ref Unique Code column

These do not require Actions


### Summary Views (_mS)

Summary views use SQL Group by functionality to summarize values with Min, Max, Sum, Count and Avg aggregate functions.  These can used as Model Sources, allowing the summarized values to be included on Relational Views

These require a Primary Key column, at least one column that is Group By Enabled, and at least one column that has a Summary Flag assigned to it.

These do not require Actions

### General Views (_mS)
General views are built on broader read-only models that join multiple related tables.

These are commonly used for:

- reporting
- analysis
- inquiry
- reviewing master and transaction data together

These views may be based on structures such as:

- parent-child hierarchies
- master data and transaction data
- multi-table analytical models

Examples include:

- a customer transaction analysis view
- a product hierarchy reporting view
- an organizational rollup analysis view

These views are generally designed for user consumption and analysis rather than direct maintenance of the underlying records.

These views only require a Primary Key column

These do not require Actions

* * *

## View Structure

A Relational View consists of:

### Columns

Columns determine which fields are included in the SQL view and shown to users.

Examples of view columns:

- Employee Name
- Department
- Asset Type
- Location
- Transaction Amount

* * *

### Actions

Actions define the interactions available from the view.  Primary used only on Data Entry views.

Examples of actions:

- Add / Edit / Copy / Delete
- Bulk Update
- Spreadsheet
- File Export / Import

* * *

### Filters

Filters define how users can narrow or focus the data shown in the view.

Examples of filters:

- Open Transactions
- West Region Locations
- Heavy Equipment Assets
- Entered this Month

* * *

## ⚙️ How Relational Views Work in Revfore Accelerate

Relational Views in Revfore Accelerate are:

- defined through metadata
- built on top of Relational Models
- used to generate SQL views in the database
- reusable across dashboards, forms, and navigation pages
- designed for both user interaction and downstream reporting

Once a Relational View is configured, its definition can be synced so that the corresponding SQL view is created or updated in the database.

* * *

## Views and SQL Views

A key concept in Revfore Accelerate is that a Relational View is not just an application definition. It also drives the creation of a corresponding **SQL view**.

This provides several benefits:

- consistent database-level access to the defined view structure
- reuse of the same logic across application pages and reporting
- alignment between application-facing and database-facing representations
- easier integration with other reporting and technical processes

This means a Relational View serves both as an application configuration object and as a generator of reusable SQL-based output.

* * *

## Views and User Experience

Relational Views translate structured model data into a usable interface.

They allow users to:

- review data
- filter and analyze data
- take action on related records
- interact with business information in a focused way

A well-designed view makes complex relational data easier to understand and use.

* * *

## Designing Relational Views (Best Practices)

### Design for the User

Build the view around what the user needs to see and do.

✔️ Good:

- a planning view focused on key editable fields
- a reporting view focused on summary and analysis
- an admin view focused on configuration and maintenance

❌ Avoid:

- views with too many columns, filters, or actions
- views that try to serve too many unrelated purposes

* * *

### Match the View to the Model Type

If the underlying model is centered around a primary table for maintenance, the view should typically support focused data entry or record management.

If the underlying model is broader and read-only, the view should typically focus on analysis, inquiry, or reporting.

* * *

### Keep Views Focused

Each view should support one primary use case.

If needed, create multiple views from the same model instead of overloading one view.

* * *

### Show Only What Matters

Include the columns, filters, and actions that are most relevant to the user.

This keeps the experience cleaner and easier to use.

* * *

### Reuse Models Thoughtfully

Views should build on strong relational models rather than duplicating structure at the view level.

This improves consistency and maintainability.

* * *

## Example Use Cases

### Example 1: Data Entry View

You have a workforce model based on:

- Employee Table
- Department Table
- Location Table

You might create a view that:

- allows users to maintain employee records
- displays related department and location information
- supports filtering by department or location

This view also creates a SQL view representing that same structure in the database.

### Example 2: General View

You have a reporting model based on:

- Customer Table
- Transaction Table
- Product Table
- Organization Hierarchy Table

You might create a view that:

- presents customer transaction data
- supports analysis across multiple related entities
- provides filters and actions for inquiry and reporting

This view also creates a SQL view that can be used for reporting and technical integration.

* * *

## Tips

- start with the intended user experience
- use focused views for data entry and maintenance
- use broader views for reporting and analysis
- keep columns, filters, and actions easy to understand
- remember that each Relational View also creates a SQL view

* * *

## Next Steps
- Learn about [Relational Views](../appGroups/admin/relationalViews/index.md)
- Review your relational model to make sure it supports the intended view
- Configure columns, actions, and filters based on the desired user experience
- Sync the view definition to create or update the corresponding SQL view