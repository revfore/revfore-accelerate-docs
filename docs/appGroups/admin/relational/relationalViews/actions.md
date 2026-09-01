# View Actions

[← Back to Relational Views Overview](index.md)

The **View Actions** page is used to define the actions available within a Relational View.

Actions allow users to interact with view data in meaningful ways.

## Overview

Use the Actions page to:

- define user actions available in a view
- support workflows and interactions
- enable users to act on data presented in the view
- extend the functionality of a view beyond simple display

## Relational View Action Fields

The following fields are used for a Relational View Action record.

| Field | Data Type | Purpose | Notes |
|---|---|---|---|
| Relational Model | int | Identifies the relational model associated with the action. | This is readonly and can be ignored when creating new records |
| Relational View | int | Identifies the relational view that the action belongs to. | This links the action record back to its parent relational view. |
| Action Name | nvarchar | Internal name of the action. | This should be clear and unique within the view. No spaces or special characters are recommended. |
| Action Display Name | nvarchar | User-friendly display name shown in the application. | This is the label users will see for the action. |
| Sequence Number | decimal | Determines the relative order in which the action appears. | Lower sequence numbers are shown first. |
| Tool Tip | nvarchar | Provides additional help text for the action. | This is shown when a user hovers over the action. |
| Component Width | int | Defines the display width of the action component. | Used to control the width of the button |
| User Interface Action Flag | int | Defines the type of user interface action. | Used to determine how the action behaves in the application. |
| Business Rule Flag | int | Defines the business rule behavior associated with the action. | Select 'Custom' for to trigger custom assembly logic |
| Form | int | Identifies the form associated with the action. | Used when the action opens or works with a specific form. |
| Is Enabled | bit | Indicates whether the action is enabled for use. | Disabled actions are not intended for active use. |
| Integration Code | nvarchar | Unique value for the relational view action record. | This is readonly and will be auto set the View Name & Action Name providing a unique value for the record that is used for importing data |
| Created Date | datetime | Date and time the record was created. | This is system maintained. |
| Modified Date | datetime | Date and time the record was last modified. | This is system maintained. |
| Created By | int | User who created the relational view action record. | This is system maintained. |
| Modified By | int | User who last modified the relational view action record. | This is system maintained. |
| Relational Action Id | int | Unique identifier for the relational view action record. | If left blank, the system will auto assign it. |

## Typical Use Cases

Examples of view actions may include:

- opening related records
- launching follow-up workflows
- performing guided user tasks
- navigating to related pages or forms

## Design Guidance

When defining actions:

- keep actions aligned with the purpose of the view
- expose only the actions users truly need
- make the intent of each action clear
- avoid cluttering the interface with unnecessary options

## General Actions

General actions can be added.

1. Go to **Admin | Relational | Relational Views**
2. Select the desired Relational View and click on '**Edit+**'
3. In the Child Views list, Select '**Relational Views : Actions**'
4. Click on the **Import** button
5. Check all desired **Actions** from the Actions tab and click **OK**

See [General Actions](../../../../concepts/metadataDrivenUI/actions.md#general-actions) for more information

## Custom Actions

Custom actions can also be added.

1. Go to **Admin | Relational | Relational Views**
2. Select the desired Relational View and click on '**Edit+**'
3. In the Child Views list, Select '**Relational Views : Actions**'
4. In the '**Relational Views : Actions**' section, Click on '**Add**' or '**Add & Edit in Grid**' 
5. Click on the '**+**' button on the top left of the grid
6. Enter required fields, select **Custom** for the **Business Rule Flag** column and click **Save**

**Add & Edit in Grid** allows adding and modifying rows directly in the grid

That defines the action. Nothing happens when a user clicks it until the code side is implemented, which is done in the extension assembly rather than here.

The **Action Name** you entered above is the value your code matches on, so it is the contract between the two halves — pick something stable and descriptive.

See [View Actions in Code](../../../../extending/actions.md) for implementing it: where `ActionHandler` fits, what it receives, and how to open another view from an action. If the model family has no handler yet, start with [Adding a Handler](../../../../extending/handlers/adding.md).

This allows developers to extend the standard action set with solution-specific functionality.

Once all actions are created, create new [Relational Filters](filters.md)

## Notes

- Actions help turn a view into an interactive experience.
- Good action design improves workflow efficiency and usability.
- Actions should support real user tasks rather than add complexity.
