# Lookups

[← Back to Admin](index.md)

Lookups are used to define **standardized values and controlled lists** used throughout the solution.

They help ensure consistency and improve data quality. Two types of lookups are supported.  The first type is a 'Relational View' type that leverages a relational view to populate the lookup.  The second type is a 'Delimited List' type that leverages a json object to populate the lookup.

## Overview

Use Lookups to:

- define controlled value lists
- standardize input values across tables and forms
- improve data consistency and governance

## Lookup Fields

The following fields are used for a Lookup record.

| Field | Data Type | Purpose | Notes |
|---|---|---|---|
| Lookup Name | nvarchar | Name of the lookup. | Spaces are allowed. |
| Lookup Type | int | Defines the type of lookup. Relational View or Delimited List. | Used to determine how the lookup is structured and resolved. |
| Relational View | int | Identifies the relational view associated with the lookup. | Required when lookup type is 'Relational View'. |
| Key/Value View Column | int | Identifies the view column that provides the key or value stored by the lookup. | This is the underlying value used by the application.  Typically the Primary Key Column. Required when lookup type is 'Relational View' and can auto-populate. |
| Display View Column | int | Identifies the view column that provides the display value for the lookup. | This is the user-friendly value shown in the application. Required when lookup type is 'Relational View' and can auto-populate. |
| Is Enabled View Column | int | Identifies the view column that provides whether or not records are enabled. | Used when the lookup values support enabled or disabled states.  Can auto-populate |
| Ext Ref Unique Code View Column | int | Identifies the view column that provides the external reference unique code. | Used when the lookup is tied to a stable unique external reference value. Required when lookup type is 'Relational View' and can auto-populate. |
| Delimited List Json Expression | nvarchar | Defines the JSON expression used for a delimited list lookup. | Required when the lookup type is 'Delimited List'. |
| Is Enabled | bit | Indicates whether the lookup is enabled for use. | Disabled lookups are not intended for active use. |
| Ext Ref Unique Code | nvarchar | Unique value for the lookup record. | This will default to a Guid but can be changed. |
| Created Date | datetime | Date and time the record was created. | This is system maintained. |
| Modified Date | datetime | Date and time the record was last modified. | This is system maintained. |
| Created By | int | User who created the lookup record. | This is system maintained. |
| Modified By | int | User who last modified the lookup record. | This is system maintained. |
| Lookup Id | int | Unique identifier for the lookup record. | If left blank, the system will typically auto assign it. |

!!!Note Important Field Notes
    The Key/Value View Column, Display View Column, Is Enabled View Column and Ext Ref Unique Code View Column fields can auto-populate once a Relational View is selected and Saved.  

    The **Key/Value View Column** will be set to the 'Primary Key' column if found

    The **Display View Column** will be set to the first column set as a 'Key Display Column; if not found, it will then look for the first column with 'Name' in the name; if not found, it will look for the first column with 'DisplayName'  in the name.

    The **Is Enabled View Column** will be set to the 'Primary Key' column if found

    The **Ext Ref Unique Code View Column** will be set to the 'Ext Ref Unique Code' column if found

    The **Delimited List Json Expression** column expects a json object that has the Key first and the Display text second. 
    Here are two examples:

    {"0":" ","1":"Sum","2":"Min","4":"Max","8":"Count","16":"Avg"}

    {"0":"None","14":"Insert, Update, Copy","15":"Default, Insert, Update, Copy"}


## Key Concepts

- Lookups provide predefined values
- Lookups are reusable across tables and forms
- Lookups help prevent inconsistent or invalid data entry

## Typical Use Cases

- Status values (Active, Inactive)
- Categories or classifications
- Dropdown selections in forms
- Standardized codes and labels

## Notes

- Use Lookups wherever consistent values are required
- Keep lookup lists simple and well-defined
- Reuse existing lookups when possible