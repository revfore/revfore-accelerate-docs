# Relational Models

[← Back to Relational Models Overview](index.md)

The **Relational Models** page is used to define and manage the header fields for a relational model.

Relational models define how relational data is organized for data entry, reporting, and analysis. A model can represent a single primary table along with related foreign key tables, or it can represent a broader read-only analytical structure across multiple related tables.

## Overview

Use the Relational Models page to:

- create new relational model definitions
- review existing relational model definitions
- manage model-level settings
- organize data structures for downstream views

Each relational model should represent a clear business use case, such as a workforce model, asset model, product model, customer transaction model, or reporting model.

## Relational Model Header Record Fields

The following fields are used for a Relational Model header record.

| Field | Data Type | Purpose | Notes |
|---|---|---|---|
| Relational Model Name | nvarchar | Internal name of the relational model. | No spaces or special characters are allowed.  If blank or set to 'Auto()' and a 'Relational Table' is selected, this value will auto populate from the Relational Column. |
| Relational Model Display Name | nvarchar | User-friendly display name shown in the application. | If blank or set to 'Auto()' and a 'Relational Table' is selected, this value will auto populate from the Relational Column. |
| Relational Model Description | nvarchar | Description of the relational model and its purpose. | |
| Relational Table | int | Identifies the primary relational table associated with the model. | This is typically the main business entity or object that the model is centered on. |
| Is Custom | bit | Indicates whether the model is custom. | Almost all non-system models will be custom. |
| Is Enabled | bit | Indicates whether the model is enabled for use. | |
| Ext Ref Unique Code | nvarchar | Unique value for the relational model record. | This is readonly and will be auto set the same value as the Model Name, providing a unique value for the record that is used for importing data. |
| Created Date | datetime | Date and time the record was created. | |
| Modified Date | datetime | Date and time the record was last modified. | |
| Created By | int | User who created the relational model record. | |
| Modified By | int | User who last modified the relational model record. | |
| Relational Model Id | int | Unique identifier for the relational model record. | If you leave blank, the system will auto assign. |

## Typical Use Cases

Use relational models to define data structures such as:

- Workforce models
- Asset models
- Product and service models
- Customer and transaction models
- Reporting and analysis models

## Create a new Relational Model

1. Go to **Admin | Relational Models**
2. Click on **Add+** or **Inline Entry**
3. Click on the **+** button on the top left of the grid
4. Enter required fields and click **Save**
5. Create new [Model Sources](sources.md)
6. Create new [Model Columns](columns.md)
7. Create new [Model Relationships](relationships.md)

!!! note "Important Notes"
    The Ext Ref Unique Code and Relational Model Id will be auto-assigned.

    See [General Actions](../../../concepts/metadataDrivenUI/actions.md#general-actions) for more information about adding records.

## Notes

- Models should be designed around a clear business use case.
- Some models are centered around a single primary table and related foreign key tables for data entry and maintenance.
- Other models are broader read-only structures used for reporting and analysis across related tables.
- Models provide the foundation for Relational Views.