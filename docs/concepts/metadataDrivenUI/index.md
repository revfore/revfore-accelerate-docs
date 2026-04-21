# Metadata-Driven UI

Revfore Accelerate uses a **metadata-driven UI** approach to expose relational data in flexible, reusable, and user-friendly ways.

Instead of hard-coding separate user experiences for each use case, Revfore Accelerate allows relational structures to be defined once and then rendered in different formats depending on the needs of the solution.

## What is Metadata-Driven UI?

A metadata-driven UI is an interface that is configured through metadata rather than custom development for every page or interaction.

In Revfore Accelerate, this means that Relational Views can be reused across multiple pages and rendered in different user interface formats without redefining the underlying data structure.

This approach helps accelerate solution development, improve consistency, and reduce the effort required to maintain user-facing experiences over time.

## How It Works

Relational Views provide the underlying data structure for metadata-driven pages.

Those views can then be rendered in a variety of user interface formats, including:

- standard grids
- pivot grids
- lists
- summary cards
- interactive BI-viewer dashboards

Because the underlying Relational View is reusable, the same view can support different presentation styles depending on the business use case.

In Genesis-integrated experiences, these layouts are organized through [Content Items](contentItem.md) and [Content Sub Items](contentSubItem.md):

- a **Content Item** defines the overall page layout
- a **Content Sub Item** defines the content rendered within each section or frame of that layout

This allows the same underlying Relational View to be reused in different page structures and presentation formats.

These same Content Items and Content Sub Items can also be used on fully custom dashboards.

## Dynamic Grids

Dynamic grids are one of the primary metadata-driven UI components in Revfore Accelerate.

They support a broad set of standard actions, including:

- add
- edit
- copy
- delete
- import
- export

They can also support custom actions.

See [Actions](actions.md) for more information.

In addition, standard grids support:

- inline entry
- dropdowns
- interactive user maintenance of relational data

This makes them well suited for both day-to-day record maintenance and more guided user experiences.

## Multi-View Page Layouts

Revfore Accelerate also supports placing two views on the same page in different layout configurations.

This is accomplished by assigning multiple [Content Sub Items](contentSubItem.md) to a single [Content Item](contentItem.md).

This allows one view to interact with the other by:

- filtering it
- refreshing it
- narrowing the related data shown on the page

This creates powerful interactive page experiences where users can move between related sets of data without leaving the page.

## Genesis Integration

Pages that include these metadata-driven views can be linked to Genesis pages.

This makes it easier to create menu-driven solutions that combine relational data, workflow, navigation, and user interaction into a cohesive experience.

By linking metadata-driven pages into Genesis navigation, solution builders can create structured application experiences without having to custom build every page from scratch.

Within this model:

- [Content Items](contentItem.md) define the page layout that is linked to Genesis
- [Content Sub Items](contentSubItem.md) define the views, cards, grids, and other content rendered inside that layout

See [Genesis Integration](../../integrations/genesis/index.md) for more information.

## Custom Dashboard Integration

Pages that include these metadata-driven views can be linked to any custom dashboard.

Within this model:

- [Content Items](contentItem.md) define the page layout that will be linked
- [Content Sub Items](contentSubItem.md) define the views, cards, grids, and other content rendered inside that layout

See [Custom Dashboard Integration](../../integrations/customDashboard/index.md) for more information.


## Benefits

A metadata-driven UI approach helps:

- accelerate solution development
- improve consistency across pages and solutions
- reuse existing relational models and views
- reduce custom development effort
- support multiple presentation styles from the same underlying structures
- simplify the creation of menu-driven Genesis solutions

## Typical Use Cases

Examples of metadata-driven UI use cases include:

- data entry and maintenance pages
- reporting and analysis pages
- summary dashboards
- drillable list and card-based pages
- interactive parent-child page layouts
- menu-driven Genesis solutions

## Notes

- Relational Views are the foundation of metadata-driven UI experiences in Revfore Accelerate.
- The same view can be reused across multiple pages and rendered in different formats.
- Page-level filters allow views to be reused without duplicating view definitions.
- Standard grids provide a rich set of built-in actions and interaction capabilities.
- Content Items and Content Sub Items provide the structure for organizing metadata-driven page layouts in Genesis-integrated solutions.
- Genesis integration makes it easy to package metadata-driven pages into a broader solution experience.