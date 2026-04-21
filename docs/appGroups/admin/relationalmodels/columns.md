# Model Columns

[← Back to Relational Models Overview](index.md)

The **Model Columns** page is used to define and manage the fields that belong to a relational model.

Model columns describe the fields that are exposed by the model for use in relational views

## Overview

Use the Columns page to:

- add new columns to a relational model
- define model field names and properties
- expose data from one or more related tables
- control how data is presented and used downstream

## Relational Model Column Fields

The following fields are used for a Relational Model Column record.

| Field | Data Type | Purpose | Notes |
|---|---|---|---|
| Relational Model | int | Identifies the relational model that the column belongs to. | This links the model column record back to its parent relational model. |
| Model Column Name | nvarchar | Internal name of the model column. | If blank or set to 'Auto()' and a 'Relational Column' is selected, this value will auto populate from the Relational Column.  No spaces or special characters are allowed. |
| Model Column Display Name | nvarchar | User-friendly display name shown in the application. | If blank or set to 'Auto()' and a 'Relational Column' is selected, this value will auto populate from the Relational Column |
| Relational Column | int | Identifies the underlying relational column used by the model column. | Typically used when the model column is directly based on a table column. |
| Sequence Number | int | Determines the relative order of the model column. | This is used to control display and processing order. If left blank on a new record, it will be auto-assigned. |
| Model Column Expression | nvarchar | Defines the expression used to derive the model column value. | Used when the column is based on logic or derived from one or more source fields rather than directly from a single relational column. |
| Model Column Description | nvarchar | Description of the model column and its purpose. | Use this to document how the column should be used. |
| Lookup - Dynamic | int | Defines the dynamic lookup associated with the model column. | Used when the column relies on a dynamic RFA lookup definition. |
| Lookup - Standard Parameter | nvarchar | Defines a standard parameter used for lookup behavior. | Used when the column relies on a standard lookup parameter. |
| Primary Model Source | int | Identifies the primary model source used by the column. | This is the main model source the model column is based on. |
| Lookup Model Source | int | Identifies the model source used for dynamic lookups. | Required when a 'Relational View' type of dynamic lookup is used. |
| Expression Model Source 1 | int | Identifies the first model source used in the column expression. | Is required if model column expression references {Alias1} |
| Expression Model Source 2 | int | Identifies the second model source used in the column expression. | Is required if model column expression references {Alias2} |
| Expression Model Source 3 | int | Identifies the third model source used in the column expression. | Is required if model column expression references {Alias3} |
| Expression Model Source 4 | int | Identifies the fourth model source used in the column expression. | Is required if model column expression references {Alias4} |
| Expression Model Source 5 | int | Identifies the fifth model source used in the column expression. | Is required if model column expression references {Alias5} |
| Relational Column Data Type | int | Defines the data type of the model column. | This will auto-populate from the 'Relational Column' if it exists.  Will need to set for expression columns |
| Is Primary Key Column | bit | Indicates whether the model column represents the primary key column. | Useful when the model needs to identify records uniquely. |
| Is Enabled | bit | Indicates whether the model column is enabled for use. | Disabled columns are not intended for active use. |
| Is Visible | bit | Indicates whether the model column is visible to users. | Hidden columns may still be used for logic, filtering, or internal processing. |
| Allow Updates | bit | Indicates whether users are allowed to update the column. | Commonly used for editable data entry models. |
| Data Format String | nvarchar | Defines the format string used to display the column value. | Useful for dates, numbers, percentages, currency, and similar formatted output. |
| Show Search | bit | Indicates whether a lookup column displays a search field. | Is not set, the lookup column is still searchable based on the first few characters a user types |
| Width | int | Defines the display width of the column. | Used to control how wide the column appears in the UI.  Use -1 to enable auto-widths |
| Sub Total Flag | int | Defines subtotal behavior for the column. | Used when the column participates in subtotal or aggregation logic. |
| Required Flag | int | Indicates whether the column is required. | Useful for data entry and validation scenarios. |
| Default Function | int | Defines the function used to calculate a default value. | Used when a default value should be generated automatically. |
| Default Expression | nvarchar | Defines the expression used to calculate the default value. | Used when a static or formula-based default value is needed. |
| Insert Function Flag | int | Indicates whether an insert function is used. | Used to control insert-time behavior for the model column. |
| Insert Function | int | Defines the function used during insert processing. | Used when custom logic should run on insert. |
| Insert Expression | nvarchar | Defines the expression used during insert processing. | Used when insert-time values are derived through an expression. |
| Update Function Flag | int | Indicates whether an update function is used. | Used to control update-time behavior for the model column. |
| Update Function | int | Defines the function used during update processing. | Used when custom logic should run on update. |
| Update Expression | nvarchar | Defines the expression used during update processing. | Used when update-time values are derived through an expression. |
| Copy Function Flag | int | Indicates whether a copy function is used. | Used to control copy behavior for the model column. |
| Copy Function | int | Defines the function used during copy processing. | Used when custom logic should run during copy operations. |
| Copy Expression | nvarchar | Defines the expression used during copy processing. | Used when copied values are derived through an expression. |
| Ext Ref Unique Code | nvarchar | Unique value for the relational model column record. | This is readonly and is typically auto set to provide a stable unique value for importing data. |
| Created Date | datetime | Date and time the record was created. | This is system maintained. |
| Modified Date | datetime | Date and time the record was last modified. | This is system maintained. |
| Created By | int | User who created the relational model column record. | This is system maintained. |
| Modified By | int | User who last modified the relational model column record. | This is system maintained. |
| Relational Model Column | int | Unique identifier for the relational model column record. | If left blank, the system will typically auto assign it. |

## Key Concepts

Each model column represents a field that the relational model exposes for downstream use.

Examples include:

- Employee Name
- Department
- Role
- Status
- Cost
- Effective Date
- Description fields from related foreign key tables
- Derived or expression-based values used for reporting and analysis

Model columns may come from:

- the primary table in the model
- related foreign key tables
- parent-child structures
- master data and transaction data combinations
- derived expressions used for analysis or display

## Design Guidance

When defining model columns:

- use clear and meaningful names
- align columns to the business use case of the model
- expose only the fields needed for downstream use
- keep the structure flexible enough to support future growth
- include descriptive fields from related tables when they improve usability
- use a consistent naming pattern when exposing fields from multiple related tables

## Create a new Model Column

1. Go to **Admin | Relational Models**
2. Select the desired Relational Model and click on **Edit+**
3. In the Child Views list, select **Relational Models : Columns**
4. In the **Relational Models : Columns** section, click on **Add** or **Inline Entry**
5. Click on the **+** button on the top left of the grid
6. Enter required fields and click **Save**

Once all columns are created, create new [Relational Model Relationships](relationships.md)

!!! note Important Notes
    Model columns define the fields that will be available to downstream Relational Views.

    Model columns can represent fields from the primary table, related foreign key tables, and broader analytical joins depending on the model design.

    Lookup and display behavior is often handled at the model level rather than directly in the base relational table.

    All 'Relational View' lookup columns will require a Lookup Model Source and possible multiple Expression Model Sources if they use expression columns for their Display or Ext Ref Unique Column.

    All expression columns will require at least one 'Expression Model Source'.  They will need as many as there are reference in the expression formula.

    If you plan to use this model as a source for a 'Relational View' lookup or as a source for a 'Relational View' that supports importing from excel, spreadsheets or API, an [XRefUniqueCode](#ext-ref-unique-code-column) column is required to be defined as a column in the Relational Table or as an expression column in the Relational Model.  Define it in the Relational Table if you have an external system value, such as an external system primary key value, that needs to be stored in it for integration purposes.  The Model Column Name preferably ends with '_XRefUniqueCode'.  If not, the Model Column Name needs to contain 'XRefUniqueCode'.

    See [General Actions](../../../concepts/metadataDrivenUI/actions.md#general-actions) for more information about adding records.

## Notes

- Model columns should be designed around the intended use of the relational model.
- Well-defined model columns improve data entry, reporting, and analysis.
- Model column design directly impacts Relational Views, forms, and the overall user experience.

---
## Other

## Ext Ref Unique Code column

What is an 'Ext Ref Unique Code' column?
- Used to uniquely identify a record
- Can be set externally before a record is created which can be helpful when needing to match a unique value in an external system and link data before it is saved.
- Can be changed at any time 
- Is not used in foreign keys relationships
- Is required for views that will be used in 'Relational View' lookups
- Is required for importing data
- Can be a table column or an expression model column.  
- if it is a table column, it should have a unique index on it to ensure that it is always unique.  You can set it to default to a New Guid (Get_GuidNew) to ensure its uniqueness as new records are created and then change it later if needed.
- If it is an expression model column, it is derived from other columns, often more than one, and must resolve to a unique record value.