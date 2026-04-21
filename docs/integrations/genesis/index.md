# Genesis Integration

Revfore Accelerate supports integration with **Genesis** to make it easier to build structured, menu-driven solutions that expose relational data through configurable page layouts.

By combining Revfore Accelerate with Genesis, solution builders can create reusable application pages without having to custom build each page from scratch.

## Overview

Genesis integration allows you to:

- link configurable page layouts to Genesis pages
- expose relational data through reusable content components
- create menu-driven solution experiences
- combine multiple relational views and UI components on the same page
- manage how content areas interact with one another

A key part of this approach is the use of [Content Items](../../concepts/metadataDrivenUI/contentItem.md) and [Content Sub Items](../../concepts/metadataDrivenUI/contentSubItem.md).

## How Genesis and Revfore Accelerate Work Together

Genesis provides the broader page and navigation framework, while Revfore Accelerate provides the relational structures and metadata-driven content that can be rendered within that framework.

Together, they make it possible to create flexible application experiences that are:

- menu-driven
- relationally aware
- reusable
- easier to maintain

## Typical Use Cases

Examples of Genesis integration use cases include:

- linking relational views into Genesis pages
- building menu-driven operational applications
- creating pages with one or two coordinated relational views
- presenting summary content alongside detailed relational data
- supporting drill, filter, and refresh interactions across page sections

## Benefits

Using Genesis integration with Revfore Accelerate helps:

- accelerate page and solution development
- reduce custom UI development effort
- reuse existing relational models and views
- create more interactive and structured application experiences
- support consistent page patterns across solutions

## Add a Relational View to a Genesis Page

!!! example "Add a Revfore Accelerate Relational View"
    1. Go to your desired **Genesis Instance Designer**.

    2. Add a page to any **Navigation Group**.

    3. Select **"Link an Existing Content Item from a Shared Workspace"**.

    4. Select the **"Revfore Accelerate (RFA)"** workspace.

    5. In the filter field, type: `ContentItem`

    6. Select one of the following dashboards:

       - `XCP_RfaConItm100_Set1000` – Single Content Sub Item  
       - `XCP_RfaConItm101_Set1000` – Two Vertical Content Sub Items  
       - `XCP_RfaConItm102_Set1000` – Two Horizontal Content Sub Items  

    7. Select the **Content** tab.

    8. Click **Configure** to set up the desired Relational View.

---

## Related Concepts

- [Content Items](../../concepts/metadataDrivenUI/contentItem.md)
- [Content Sub Items](../../concepts/metadataDrivenUI/contentSubItem.md)