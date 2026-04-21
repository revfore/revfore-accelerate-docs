# Table Columns

[← Back to Relational Tables Overview](index.md)

The **Table Columns** page is used to define and manage the fields that belong to a relational table.

Columns describe the attributes of each record stored in a table.

## Overview

Use the Columns page to:

- add new columns to a relational table
- define field names and properties
- manage the structure of table data
- control how data is stored and displayed

## Relational Table Column Fields

The following fields are used for a Relational Table Column record.

| Field | Data Type | Purpose | Notes |
|---|---|---|---|
| Relational Table | int | Identifies the relational table that the column belongs to. | This links the column record back to its parent relational table. |
| Column Name | nvarchar | Internal name of the column. | This is the name of the actual column in the database. No spaces or special characters are allowed. |
| Column Display Name | nvarchar | User-friendly display name shown in the application. | This is typically the label users will see. |
| Column Description | nvarchar | Description of the column and its purpose. | Use this to document how the column should be used. |
| Data Type | int | Defines the data type of the column. | This controls how the value is stored and interpreted in the database. |
| Sequence Number | int | Determines the relative order of the column. | This is typically used to control the display and processing order of columns. |
| Is Nullable | bit | Indicates whether the column allows null values. | If enabled, the column is not required. |
| Max Length | int | Defines the maximum allowed length for the column value. | Typically used for text-based fields such as nvarchar. |
| Precision | int | Defines the total number of digits allowed for numeric values. | Used with numeric data types such as decimal that support precision and scale. |
| Scale | int | Defines the number of digits allowed to the right of the decimal place. | Used with numeric data types that support decimal values. |
| Is Identity | bit | Indicates whether the column is an identity column. | Identity columns are typically auto-incrementing numeric keys and are assigned to the primary key column |
| Is Primary Key | bit | Indicates whether the column is part of the primary key. | Primary key columns uniquely identify records in the table. |
| Default Value | nvarchar | Defines the default value for the column. | This value is used when no value is explicitly provided. |
| Bulk Update Flag | int | Controls how the column behaves during bulk update processes. | Use this to manage whether and how the column participates in bulk updates. |
| Lookup - Standard Parameter | nvarchar | Defines a standard parameter used for lookup behavior. | Used when the column relies on a standard lookup parameter. |
| Lookup - Dynamic | int | Defines the dynamic lookup associated with the column. | Used when the column relies on a dynamic RFA lookup definition. |
| Is Custom | bit | Indicates whether the column is custom. | Most user-defined columns will be custom. |
| Is Enabled | bit | Indicates whether the column is enabled for use. | Disabled columns are not intended for active use. |
| Ext Ref Unique Code | nvarchar | Unique value for the relational table column record. | This is readonly and will be auto set the Table Name & Column Name providing a unique value for the record that is used for importing data |
| Created Date | datetime | Date and time the record was created. | This is system maintained. |
| Modified Date | datetime | Date and time the record was last modified. | This is system maintained. |
| Created By | int | User who created the relational table column record. | This is system maintained. |
| Modified By | int | User who last modified the relational table column record. | This is system maintained. |
| Relational Table Column Id | int | Unique identifier for the relational table column record. | If left blank, the system will typically auto assign it. |

## Key Concepts

Each column represents a single attribute of a business entity.

Examples include:

- Name
- Department
- Role
- Status
- Cost
- Effective Date

## Design Guidance

When defining columns:

- use clear and meaningful names
- align fields to real business attributes
- avoid adding unnecessary columns too early
- keep the structure flexible enough to support future growth

## Create a new Relational Column

1. Go to **Admin | Relational Tables**
2. Select the desired Relational Table and click on '**Edit+**'
3. In the Child Views list, Select '**Relational Tables : Columns**'
4. In the '**Relational Tables : Columns**' section, Click on '**Add**' or '**Inline Entry**' 
5. Click on the '**+**' button on the top left of the grid
6. Enter required fields and click **Save**

Once all columns are created, create new [Relational Indexes](indexes.md)

!!!Note Important Notes
    The Ext Ref Unique Code and Relational Column Id will be auto-assigned

    The Sequence Number will be auto-assigned if blank on a new record

    The first column should be the **Primary Key Column**. It should be not nullable and an Int. It is typically an Identity column.  All tables need a primary key column.

    Text based columns will require a Max Length

    Decimal values will require a Precision

    Lookup values do not need to be specified here.  They will need to be specified on the Relational Model.

    If you plan to use this model as a source for a 'Relational View' lookup or as a source for a 'Relational View' that supports importing from excel, spreadsheets or API, an [XRefUniqueCode](#ext-ref-unique-code-column) column is required to be defined as a column in the Relational Table or as an expression column in the Relational Model.  Define it in the Relational Table if you have an external system value, such as an external system primary key value, that needs to be stored in it for integration purposes.

    See [General Actions](../../../concepts/metadataDrivenUI/actions.md#general-actions) for more information about adding records

## Notes

- Columns should be specific to the purpose of the table.
- Well-defined columns improve usability, reporting, and downstream configuration.
- Column design directly impacts forms, models, and views.

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