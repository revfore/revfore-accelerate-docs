# Model Relationships

[← Back to Relational Models Overview](index.md)

The **Model Relationships** page is used to define which Table Relationships will be exposed for downstream use in Navigation pages

## Overview

Use the Relationships page to:

- import table relationships for the model that will be exposed for downstream use
- disable table relationships on the model

## Relational Model Relationship Fields

The following fields are used for a Relational Model Relationship record.

| Field | Data Type | Purpose | Notes |
|---|---|---|---|
| Relational Model | int | Identifies the relational model that the relationship belongs to. | This links the model relationship record back to its parent relational model. |
| Relational Relationship Id | int | Identifies the underlying relational table relationship used by the model. | This is the table-level relationship being referenced within the model. |
| Category Flag | int | Defines the category flag used for the relationship within the model. | This is readonly and defined on the table relationship. |
| Related To Relational Model | int | Identifies the related relational model connected to this relationship. | Used when the model relationship points to or interacts with another relational model. |
| Related To Category Flag | int | Defines the category flag used for the related side of the relationship. | This is readonly and defined on the table relationship. |
| Is Enabled | bit | Indicates whether the model relationship is enabled for use. | Disabled relationships are not intended for active use. |
| Ext Ref Unique Code | nvarchar | Unique value for the relational model relationship record. | This is readonly and is auto set to provide a stable unique value for importing data. |
| Created Date | datetime | Date and time the record was created. | This is system maintained. |
| Modified Date | datetime | Date and time the record was last modified. | This is system maintained. |
| Created By | int | User who created the relational model relationship record. | This is system maintained. |
| Modified By | int | User who last modified the relational model relationship record. | This is system maintained. |
| Relational Model Relationship Id | int | Unique identifier for the relational model relationship record. | If left blank, the system will typically auto assign it. |

## Typical Use Cases

Examples include:

- linking employees to departments
- linking assets to locations
- linking products to categories or service lines

## Design Guidance

When defining model relationships:

- make sure each relationship supports a valid business use case

## Import a new Model Relationship (Recommended)

1. Go to **Admin | Relational Models**
2. Select the desired Relational Model and click on **Edit+**
3. In the Child Views list, select **Relational Models : Relationships**
4. In the **Relational Models : Relationships** section, click on **Import**
5. Select the desired Relationships and click on **OK**


## Create a new Model Relationship

1. Go to **Admin | Relational Models**
2. Select the desired Relational Model and click on **Edit+**
3. In the Child Views list, select **Relational Models : Sources**
4. In the **Relational Models : Sources** section, click on **Add** or **Enable Inline Adding & Editing**
5. Click on the **+** button on the top left of the grid
6. Enter required fields and click **Save**

**Enable Inline Adding & Editing** allows adding and modifying rows directly in the grid

## Notes

- Relationships are central to the usefulness of a relational model.
- Strong relationship design improves downstream view creation and user experience.
- Model relationships should build on well-defined source structures.