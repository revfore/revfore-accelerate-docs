# Columns

[← Back to Relational Views Overview](index.md)

The **Columns** page is used to define which columns appear in a Relational View and how they are presented.

These columns determine the structure of the data users see and interact with.

## Overview

Use the Columns page to:

- select the columns included in a view
- define how data is displayed to users
- organize output for reporting, forms, or dashboards
- tailor the view to a specific use case

## Relational View Column Fields

The following fields are used for a Relational View Column record.

| Field | Data Type | Purpose | Notes |
|---|---|---|---|
| Relational Model | int | Identifies the relational model associated with the view column. | This is readonly and can be ignored when creating new records |
| Relational View | int | Identifies the relational view that the column belongs to. | This links the view column record back to its parent relational view. |
| Column Name | nvarchar | Internal name of the view column. | No spaces or special characters are allowed.  Can auto-populate. See Important Field Notes below. |
| Column Display Name | nvarchar | User-friendly display name shown in the application. | This is typically the label users will see.  Can auto-populate. See Important Field Notes below. |
| Column Description | nvarchar | Description of the view column and its purpose. | Use this to document how the column should be used.  Can auto-populate. See Important Field Notes below. |
| Relational Model Column | int | Identifies the relational model column used by the view column. | A view column is typically built on top of a model column. |
| Sequence Number | int | Determines the relative order of the view column. | This is used to control display and processing order.  Will auto-populate on new records. |
| Column Data Type | int | Defines the data type of the view column. | This controls how the value is interpreted and used in the view. |
| Precision | int | Defines the total number of digits allowed for numeric values. | Used with numeric data types such as decimal that support precision and scale. |
| Scale | int | Defines the number of digits allowed to the right of the decimal place. | Used with numeric data types that support decimal values. |
| Is Primary Key | bit | Indicates whether the view column represents a primary key field. | Useful when the view needs to uniquely identify records. |
| Bulk Update Flag | int | Controls how the view column behaves during bulk update processes. | Use this to manage whether and how the column participates in bulk updates. |
| Model Lookup - Standard Parameter | nvarchar | Defines the standard lookup parameter inherited from the model column. | Used when lookup behavior is defined at the model level. |
| Lookup - Standard Parameter | nvarchar | Defines the standard lookup parameter used by the view column. | Used when lookup behavior is overridden or defined at the view level. |
| Model Lookup - Dynamic | int | Defines the dynamic lookup inherited from the model column. | Used when dynamic lookup behavior is defined at the model level. |
| Lookup - Dynamic | int | Defines the dynamic lookup used by the view column. | Used when lookup behavior is overridden or defined at the view level. |
| Is Enabled | bit | Indicates whether the view column is enabled for use. | Can auto-populate. See Important Field Notes below. |
| Is Visible | bit | Indicates whether the view column is visible to users. | Hidden columns may still be used for logic, filtering, or internal processing.  Can auto-populate. See Important Field Notes below. |
| Model Allow Updates | bit | Indicates whether updates are allowed at the model column level. | Used to inherit or understand update behavior from the model.  This is Readonly. |
| Allow Updates | bit | Indicates whether users are allowed to update the view column. | Commonly used for editable data entry views.  Can auto-populate. See Important Field Notes below. |
| Key Display Column | bit | Indicates whether the column should be treated as a key display column. | Certain lists will need to know which columns to shown to label the record.  At least once column should be marked as a Key Display Column. |
| Filter Flag | int | Defines filter behavior for the view column. | Used to control whether and how the column participates in filtering. |
| Width | int | Defines the display width of the column. | Used to control how wide the column appears in the UI. |
| Sub Total Flag | int | Defines subtotal behavior for the column. | Used when the column participates in subtotal or aggregation logic. |
| Group By Flag | int | Defines whether the column participates in group by behavior. | Used when the View's 'Group By Enabled' field is On. |
| Ext Ref Unique Code | nvarchar | Unique value for the relational view column record. | This is readonly and is typically auto set to provide a stable unique value for importing data. |
| Created Date | datetime | Date and time the record was created. | This is system maintained. |
| Modified Date | datetime | Date and time the record was last modified. | This is system maintained. |
| Created By | int | User who created the relational view column record. | This is system maintained. |
| Modified By | int | User who last modified the relational view column record. | This is system maintained. |
| Relational Column Id | int | Unique identifier for the relational view column record. | If left blank, the system will typically auto assign it. |

!!!Note Important Field Notes
    The Name, Display Name, Column Data Type, Is Enabled, Allow Updates and Is Visible fields will auto-populate once a Relational Model Column is selected and Saved.

## Key Concepts

View columns are based on the underlying relational model and define the final output structure presented by the view.

A good view should expose the right data in a clear and usable format.

## Design Guidance

When defining view columns:

- include only the fields needed by the user or use case
- keep output clear and focused
- use consistent naming and ordering
- align the view structure with how it will be consumed

## Import new Relational View Columns

1. Go to **Admin | Relational Views**
2. Select the desired Relational View and click on '**Edit+**'
3. In the Child Views list, Select '**Relational Views : Actions**'
4. Click on the **Import** button
5. Check all desired **Columns** from the Columns tab and click **OK**

## Create a new Relational View Column

1. Go to **Admin | Relational Views**
2. Select the desired Relational Table and click on '**Edit+**'
3. In the Child Views list, Select '**Relational Views : Columns**'
4. In the '**Relational Views : Columns**' section, Click on '**Add**' or '**Inline Entry**' 
5. Click on the '**+**' button on the top left of the grid
6. Enter required fields and click **Save**

Once all columns are created, create new [Relational Actions](actions.md)

!!!Note Important Notes
    The Name, Display Name, Column Data Type, Is Enabled, Allow Updates and Is Visible fields will auto-populate once a Relational Model Column is selected and Saved.

## Notes

- View columns directly affect usability.
- A well-designed set of columns makes a view easier to understand and use.
- View columns are often what users interact with most directly.