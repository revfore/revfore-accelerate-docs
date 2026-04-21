# Content Items

[← Metadata-Driver Overview](index.md)
[← Genesis Integration](../../integrations/genesis/index.md)
[← Custom Dashboard Integration](../../integrations/customDashboard/index.md)

A **Content Item** is a configurable page layout that can be linked to a Genesis or Custom page.

It defines the overall page structure and determines how content is arranged on the page.

## Overview

Use Content Items to:

- define the layout of a Genesis or Custom linked page
- control how many content sections or frames appear on the page
- support reusable page structures
- provide the container for one or more Content Sub Items

## Page Layout Role

A Content Item represents the page-level layout rather than the detailed content shown inside each section.

For example, a Content Item may define a layout with:

- a single full-page section
- two vertically stacked sections
- two horizontally arranged sections

Revfore Accelerate currently supports **three Content Item layouts**, with many more planned for future release.

## Content Frames and Sections

Each Content Item contains one or more sections or frames.

Each section or frame is assigned a [Content Sub Item](contentSubItem/index.md), which determines what is actually rendered inside that portion of the page.

This means the Content Item controls the layout, while the Content Sub Items control the content and behavior inside the layout.

## Typical Use Cases

Examples of Content Item use cases include:

- a single-view maintenance page
- a two-panel page with summary data on one side and detail data on the other
- a vertically stacked page with a parent view on top and a related child view below

## Benefits

Using Content Items helps:

- standardize page layouts
- reduce custom UI development
- reuse page patterns across solutions
- make Genesis pages easier to configure and maintain

## Notes

- Content Items define the overall page layout.
- Content Items do not define the detailed behavior of the content itself.
- Content Sub Items are assigned to each section or frame within a Content Item.
- Additional Content Item layouts are planned for future release.

## Related Concepts

- [Genesis Integration](../../integrations/genesis/index.md)
- [Content Sub Items](contentSubItem/index.md)