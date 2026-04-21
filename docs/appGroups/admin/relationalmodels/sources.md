# Model Sources

[← Back to Relational Models Overview](index.md)

The **Model Sources** page is used to define the source tables and source structures used within a relational model.

Sources establish the underlying data inputs that the model is built on.

## Overview

Use the Sources page to:

- select the tables used in a model
- define the primary data sources for the model
- organize the model structure around relevant business entities
- prepare the model for downstream columns and relationships

## Relational Model Source Fields

The following fields are used for a Relational Model Source record.

| Field | Data Type | Purpose | Notes |
|---|---|---|---|
| Relational Model | int | Identifies the relational model that the source belongs to. | This links the model source record back to its parent relational model. |
| Relational Source Object | int | Identifies the relational source object used by the model source. | This is typically the table or object being added as a source in the model. |
| Table Relationship | int | Identifies the table relationship used by the model source. | Highly recommended for all sources other than the first source.  Once set many of the other fields will auto populate |
| Sequence Number | int | Determines the relative order of the model source. | This is used to control the order in which sources are processed.  The first source should not join with another source.  All subsequent sources do need to join with another source.  This field will auto-populate if left blank |
| Alias | nvarchar | Alternate short name or alias for the model source. | This is used in joins and expressions to distinguish sources.  Will auto-populate if a Table Relationship is specified, specifically from from the 'Source Relational Table' 'Alias' and the 'Table Relationship' 'Join Alias Suffix'. No spaces or special characters are allowed. |
| Join Type | int | Defines the type of join used for the model source. | Inner and Left Joins are the two options.  Inner joins require that the joined table has matching records.  Left joins do not have this requirement but do not perform as well. |
| Join With Relational Model Source 1 | int | Identifies the first model source that this source joins to. | Used when defining the 1st join condition. |
| Join Relational Column 1 | int | Identifies the first relational column from this source used in the join. | This is the source-side column for the 1st join condition. |
| Join With Relational Column 1 | int | Identifies the first relational column from the joined source used in the join. | This is the target-side column for the 1st join condition. |
| Join With Relational Model Source 2 | int | Identifies the second model source that this source joins to. | Used when defining an 2nd join condition. |
| Join Relational Column 2 | int | Identifies the second relational column from this source used in the join. | This is the source-side column for the 2nd join condition. |
| Join With Relational Column 2 | int | Identifies the second relational column from the joined source used in the join. | This is the target-side column for the 2nd join condition. |
| Join With Relational Model Source 3 | int | Identifies the third model source that this source joins to. | Used when defining an 3rd join condition. |
| Join Relational Column 3 | int | Identifies the third relational column from this source used in the join. | This is the source-side column for the 3rd join condition. |
| Join With Relational Column 3 | int | Identifies the third relational column from the joined source used in the join. | This is the target-side column for the 3rd join condition. |
| Join With Relational Model Source 4 | int | Identifies the fourth model source that this source joins to. | Used when defining an 4th join condition. |
| Join Relational Column 4 | int | Identifies the fourth relational column from this source used in the join. | This is the source-side column for the 4th join condition. |
| Join With Relational Column 4 | int | Identifies the fourth relational column from the joined source used in the join. | This is the target-side column for the 4th join condition. |
| Join With Relational Model Source 5 | int | Identifies the fifth model source that this source joins to. | Used when defining an 5th join condition. |
| Join Relational Column 5 | int | Identifies the fifth relational column from this source used in the join. | This is the source-side column for the 5th join condition. |
| Join With Relational Column 5 | int | Identifies the fifth relational column from the joined source used in the join. | This is the target-side column for the 5th join condition. |
| Additional Join Clause | nvarchar | Defines any additional SQL join logic required for the model source. | Used when the relationship cannot be fully defined through the standard join fields alone.  Use the **Clause** button to open the expression editor page |
| Ext Ref Unique Code | nvarchar | Unique value for the relational model source record. | This is readonly and is auto set to provide a stable unique value for importing data. |
| Created Date | datetime | Date and time the record was created. | This is system maintained. |
| Modified Date | datetime | Date and time the record was last modified. | This is system maintained. |
| Created By | int | User who created the relational model source record. | This is system maintained. |
| Modified By | int | User who last modified the relational model source record. | This is system maintained. |
| Relational Model Source | int | Unique identifier for the relational model source record. | If left blank, the system will typically auto assign it. |

## Key Concepts

A relational model typically combines one or more source tables into a meaningful structure.

Sources determine what data is available within the model and how the model is grounded in underlying relational tables.

## Typical Use Cases

Examples include:

- using Employee and Department tables in a workforce model
- combining Product and Service Line tables in a planning model
- structuring Asset and Location data in an operational model

## Create a new Model Source

1. Go to **Admin | Relational Models**
2. Select the desired Relational Model and click on **Edit+**
3. In the Child Views list, select **Relational Models : Sources**
4. In the **Relational Models : Sources** section, click on **Add** or **Inline Entry**
5. Click on the **+** button on the top left of the grid
6. Enter required fields and click **Save**

Once all sources are created, create new [Relational Model Columns](columns.md)

!!!Note Important Notes
    The Ext Ref Unique Code and Relational Model Source Id will be auto-assigned

    The first source is the primary source and should not join to another source

    Table Relationships are highly recommended on all sources other than the first source

    Many fields will auto-populate if a Table Relationship is specified.

    The Sequence Number will be auto-assigned if blank on a new record

    Sources are needed for lookup columns.  Typically only one source if the lookup pulls from columns that share a single source.  If the lookup pulls from columns from any sources, all of those sources will need to be added.  This often happens if Lookups use expression columns for their Display or Ext Ref Unique Code column.

## Notes

- Sources should reflect the purpose of the model.
- A model should include only the sources needed for the intended use case.
- Good source selection simplifies downstream model design.