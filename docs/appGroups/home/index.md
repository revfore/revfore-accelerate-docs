# Home Application Group

The **Home Application Group** is used to create and manage **custom navigation groups and pages** within a Genesis workspace.

It provides a flexible way to build user-facing navigation and integrate Relational Views created in Revfore Accelerate.

## Overview

Use the Home Application Group to:

- create custom navigation groups
- add custom pages
- integrate Relational Views into user-facing pages
- extend existing Genesis workspaces with additional functionality

You can also add custom navigation groups and pages to **any Genesis workspace**, not just the Home Application Group.

---

## When to Use the Home Application Group

The Home Application Group is best suited for:

- adding a **limited number of navigation groups and pages**
- extending an existing solution with a small number of custom pages
- quickly exposing Relational Views to end users

---

## When to Use a Separate Instance

If you need to:

- create a large number of navigation groups and pages
- build a full solution experience
- leverage additional application groups such as:
  - Config
  - Admin
  - Help

👉 then you should create a **separate Genesis instance**

This approach is recommended when building a **complete standalone solution**.

---

## Integrating Relational Views into Genesis Pages

You can add a Revfore Accelerate Relational View to any page in any Genesis instance.

This allows you to surface relational data directly within your application’s navigation.

---

## Steps: Add a Relational View to a Genesis Page

!!! example "Add a Revfore Accelerate Relational View"
    1. Go to your desired **Genesis Instance Designer**.

    2. Add a page to any **Navigation Group**.

    3. Select **"Link an Existing Content Item from a Shared Workspace"**.

    4. Select the **"Revfore Accelerate (RFA)"** workspace.

    5. In the filter field, type:
       
       `ContentItem`

    6. Select one of the following dashboards:

       - `XCP_RfaConItm100_Set1000` – Single Content Sub Item  
       - `XCP_RfaConItm101_Set1000` – Two Vertical Content Sub Items  
       - `XCP_RfaConItm102_Set1000` – Two Horizontal Content Sub Items  

    7. Select the **Content** tab.

    8. Click **Configure** to set up the desired Relational View.

---

## Notes

- These dashboards are prebuilt to host Relational Views within Genesis pages.
- The layout you choose determines how the content is displayed (single, vertical split, or horizontal split).
- Relational Views must already be defined in Revfore Accelerate before they can be configured in a page.

---

## Related Application Groups

- Use the **Admin** application group to create and manage Relational Tables, Models, and Views.
- Use the **Config** application group for setup, upgrades, and system-level configuration.
- Use the **Help** application group for documentation and support resources.