# Table Relationships

[← Back to Relational Tables Overview](index.md)

The **Table Relationships** page is used to define how relational tables connect to one another.

These relationships allow individual tables to work together as part of a broader relational structure.

## Overview

Use the Relationships page to:

- connect one table to another
- define parent-child or related-entity structures
- support downstream model and view configuration
- enable richer and more connected data solutions

## Relational Table Relationship Fields

The following fields are used for a Relational Table Relationship record.

| Field | Data Type | Purpose | Notes |
|---|---|---|---|
| Relational Table | int | Identifies the relational table that the relationship belongs to. | This links the relationship record back to its parent relational table. |
| Relational Relationship Name | nvarchar | Internal name of the relational relationship. | This should clearly describe the relationship. No spaces or special characters are allowed. |
| Is Foreign Key | bit | Indicates whether the relationship is enforced as a foreign key. | Relationships are primarily used for foreign keys, but they can also be used only for navigation between related records. |
| Is Optional | bit | Indicates whether the relationship is optional. | If not optional, Model Sources will default to Inner joins vs Left joins.  Inner joins are more efficient. |
| Join Alias Suffix | nvarchar | Suffix used when generating join aliases for the relationship. | This helps distinguish joins when the same table is referenced multiple times.  Not required if only joining to a table once. No spaces or special characters are allowed.  A good example is the CreatedById and ModifiedById columns, they both relate to the same 'User' table and require different Join Alias Suffixes |
| Relational Column 1 | int | Defines the primary column in the source table used for the relationship. | This is typically the source column that joins to the related table. |
| Category Flag | int | Defines the category flag associated with the source side of the relationship. | These are used to organize the relationships for end-users |
| Related To Table | int | Identifies the related table on the other side of the relationship. | This is the table that the source table points to or navigates to. |
| Related To Column 1 | int | Defines the primary column in the related table used for the relationship. | This is the target column that the source column joins to. |
| Related To Category Flag | int | Defines the category flag associated with the related side of the relationship. | These are used to organize the relationships for end-users |
| Display Name Flag | int | Identifies the display name behavior for the relationship. | Used to control how related records are labeled or presented in the application. |
| Ext Ref Unique Code | nvarchar | Unique value for the relational relationship record. | This is readonly and will be auto set the Table Name & Relationship Name providing a unique value for the record that is used for importing data |
| Created Date | datetime | Date and time the record was created. | This is system maintained. |
| Modified Date | datetime | Date and time the record was last modified. | This is system maintained. |
| Created By | int | User who created the relational relationship record. | This is system maintained. |
| Modified By | int | User who last modified the relational relationship record. | This is system maintained. |
| Relational Relationship Id | int | Unique identifier for the relational relationship record. | If left blank, the system will typically auto assign it. |

## Typical Use Cases

Examples of common table relationships include:

- Employee to Department
- Asset to Location
- Product to Service Line
- Transaction to Customer

## Design Guidance

When defining relationships:

- model real-world business relationships
- keep links clear and intentional
- avoid unnecessary complexity
- make sure the relationship supports a meaningful business use case

## Create a new Relational Table Relationship

1. Go to **Admin | Relational Tables**
2. Select the desired Relational Table and click on '**Edit+**'
3. In the Child Views list, Select '**Relational Tables : Relationships**'
4. In the '**Relational Tables : Relationships**' section, Click on '**Add**' or '**Inline Entry**' 
5. Click on the '**+**' button on the top left of the grid
6. Enter required fields and click **Save**

!!!Note Important Notes
    The Ext Ref Unique Code and Relational Relationship Id will be auto-assigned

    The Relational Relationship Name will be auto-assigned if blank or has the value 'Auto()' in it
    
    See [General Actions](../../../concepts/metadataDrivenUI/actions.md#general-actions) for more information about adding records

## Notes

- Relationships at the table level form a foundation for higher-level modeling.
- Well-defined relationships improve flexibility and usability across the solution.
- These relationships are often later used in relational models and views.