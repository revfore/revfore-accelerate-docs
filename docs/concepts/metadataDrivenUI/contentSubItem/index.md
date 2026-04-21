# Content Sub Items

[← Metadata-Driver Overview](../index.md)
[← Genesis Integration](../../../integrations/genesis/index.md)
[← Custom Dashboard Integration](../../../integrations/customDashboard/index.md)

A **Content Sub Item** is the configured content placed into a section or frame within a Content Item.

Each section or frame in a Content Item layout is assigned a Content Sub Item, which determines what is rendered in that section of the page and how it behaves.

## Overview

Use Content Sub Items to:

- define what content is shown in a page section
- control how that content is rendered
- configure user interface options for the content frame
- manage view columns, actions, and interactions
- define how one content section interacts with another

## Content Types

Content Sub Items can be configured with content types such as:

- dynamic grid
- list
- pivot grid
- summary card
- relational view
- interactive BI-viewer dashboard

This allows the same overall page layout to support different presentation styles depending on the use case.

## Content Sub Item Configuration

In addition to content type, Content Sub Items can be configured with settings such as:

- frame options
- font sizes
- view column settings
- action settings
- other display and interaction options

These settings allow each section of a page to be tailored to the intended user experience.

## Interaction Between Content Sub Items

Content Sub Items can also be configured to interact with one another.

This makes it possible for one section of a page to affect another by:

- filtering related content
- refreshing related content
- narrowing the data shown in another section

This supports more interactive page experiences and allows users to move through related sets of data without leaving the page.

## Typical Use Cases

Examples of Content Sub Item use cases include:

- rendering a relational view as a standard grid
- showing summary information in a card layout
- displaying data in a pivot grid for analysis
- using one view to filter another on the same page
- combining detail and summary content in a coordinated page experience

## Benefits

Using Content Sub Items helps:

- make page sections highly reusable
- support different content formats from the same underlying relational structures
- reduce the need for custom page development
- enable richer interactions across page sections

## Notes

- Content Sub Items define the content rendered inside each Content Item section.
- Content type, display settings, and interaction behavior are all configured at the Content Sub Item level.
- Content Sub Items can be configured to interact with one another on the same page.

## Related Concepts

- [Genesis Integration](../../../integrations/genesis/index.md)
- [Content Items](../contentItem.md)