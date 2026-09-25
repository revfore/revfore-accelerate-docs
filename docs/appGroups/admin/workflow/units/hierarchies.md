# Unit Hierarchies

[← Back to Workflow Units Overview](index.md)

A **Unit Hierarchy** arranges workflow units into a parent and child structure, so that a unit higher up the structure can see and review the work of the units beneath it.

Without a hierarchy, every workflow unit is an island: it has its own data and nobody has a view across it. A hierarchy is what makes a review or roll-up level possible.

## Overview

Use Unit Hierarchies to:

- define which units report to which, for a given purpose
- give a **Parent** unit a view across the units beneath it
- drive which records a view shows, through the view's [Unit Records Shown](../../relational/relationalViews/views.md#workflow-behaviour) setting
- roll a submission process up through review and approval levels

A unit can appear in more than one hierarchy. Reporting structure and approval structure are often different, and each is its own hierarchy rather than a compromise between the two.

## The four parts

A hierarchy is made up of one record you create, one grid you maintain, and two tables the system builds for you.

| Part | What it is | Maintained by |
|---|---|---|
| **Unit Hierarchies** | The hierarchy itself — its name, effective dates and processing state. | You |
| **Relationships** | One row per parent-and-child pair. This is the structure. | You |
| **Relationships By Row** | Every ancestor-to-descendant pair, at any depth. | **Process** |
| **Relationships By Column** | One row per unit with its ancestors spread across columns. | **Process** |

The last two exist so that a question like *"every unit below this one, however deep"* is a single join rather than a recursive walk. They are rebuilt in full each time the hierarchy is processed, so they are never edited directly and carry no add, edit or delete actions.

!!!Note Important
    **The derived tables stay empty until you run Process.** Creating the hierarchy and entering its relationships is only half the job — nothing that reads the hierarchy will see anything until it has been processed.

## Unit Hierarchy Header Record Fields

The following fields are used for a Unit Hierarchy header record.

| Field | Data Type | Purpose | Notes |
|---|---|---|---|
| Name | nvarchar | Internal name of the hierarchy. | Must be unique. No spaces or special characters are allowed. |
| Display Name | nvarchar | User-friendly display name shown in the application. | Must be unique. |
| Description | nvarchar | Description of the hierarchy and its purpose. | Worth completing where more than one hierarchy exists, so it is clear which is which. |
| Effective Start Date | date | Date the hierarchy becomes available for use. | Defaults to 1900-01-01, meaning available from the beginning. |
| Effective End Date | date | Date the hierarchy stops being available for use. | Defaults to 2999-12-31, meaning no end date. |
| State Flags | int | Whether the hierarchy is open for change or frozen. | A frozen hierarchy declines to process, which is how a structure is locked once a cycle is under way. |
| Is Enabled | bit | Indicates whether the hierarchy is enabled for use. | |
| By Column Status | int | Whether the Relationships By Column table is current, and whether it fits. | Set by Process. See [Depth and By Column](#depth-and-by-column). |
| Data Flags | int | What Process found in the structure. | Set by Process. Reports duplicates, cycles and unreachable units. |
| Height | int | The deepest level in the hierarchy. | Set by Process. |
| Node Count | int | How many units the hierarchy contains. | Set by Process. |
| Last Processed At | datetime | When Process last ran. | Set by Process. A blank value means it has never been processed. |
| Integration Code | nvarchar | Unique value for the hierarchy record. | This is readonly and provides a unique value for the record that is used for importing data |
| Created Date | datetime | Date and time the record was created. | |
| Modified Date | datetime | Date and time the record was last modified. | |
| Created By | int | User who created the record. | |
| Modified By | int | User who last modified the record. | |
| Workflow Unit Hierarchy Id | int | Unique identifier for the hierarchy record. | If you leave blank, the system will auto assign |

## Relationship Record Fields

Each row says *this unit hangs under that unit*. This is the only part of a hierarchy you build by hand.

| Field | Data Type | Purpose | Notes |
|---|---|---|---|
| Hierarchy | int | The hierarchy this relationship belongs to. | Required. |
| Unit | int | The workflow unit being placed. | Required. The unit must already exist. |
| Parent Unit | int | The unit it hangs under. | **Leave blank for a top-level unit.** A blank parent is what makes a unit a root. |
| Sibling Sequence Number | bigint | The order this unit appears among the others under the same parent. | Yours to set, but see [Sibling order](#sibling-order) — Process tidies the numbering. |
| Level | int | How deep the unit sits. Roots are level 0. | Set by Process. |
| Is Leaf | bit | Whether the unit has anything beneath it. | Set by Process. |
| Path | nvarchar | The unit's line of ancestry. | Set by Process. |
| Tree Sequence Number | int | The unit's position when the whole hierarchy is read top to bottom. | Set by Process. See [Sorting into tree order](#sorting-into-tree-order). |
| Integration Code | nvarchar | Unique value for the relationship record. | This is readonly. |
| Is Enabled | bit | Indicates whether the relationship is enabled. | A disabled relationship is ignored by Process, which is how a unit is taken out of a structure without losing the row. |
| Created Date | datetime | Date and time the record was created. | |
| Modified Date | datetime | Date and time the record was last modified. | |
| Created By | int | User who created the record. | |
| Modified By | int | User who last modified the record. | |
| Workflow Unit Hierarchy Relationship Id | int | Unique identifier for the record. | If you leave blank, the system will auto assign |

!!!Note Important Field Notes
    **Level**, **Is Leaf**, **Path** and **Tree Sequence Number** are all worked out by Process. They are shown so you can see what the structure resolved to, but entering a value by hand achieves nothing — the next Process run overwrites it.

## Sibling order

**Sibling Sequence Number** is the one derived-looking field that is genuinely yours: it decides the order units appear under the same parent.

Process reads the order you asked for and then writes it back tidied up — renumbered 1, 2, 3 and so on, restarting under each parent. Two things follow from that:

- **A new row added with no sequence number sorts to the end of its group**, not to the top, and is given the next number. Adding a unit puts it where you would expect rather than at the front of the list.
- **You do not have to renumber by hand** to make room. Give a row any number that puts it in the right place relative to its siblings, and Process normalises the rest.

## Sorting into tree order

**Tree Sequence Number** is the unit's position when the hierarchy is read from the top down, the way it would appear in a tree view. Sorting a flat list by it reproduces that order.

Nothing else does the same job:

- **Level** sorts a whole level at a time, so all the roots come first, then everything at level 1.
- **Sibling Sequence Number** only orders within one parent, so it repeats across the hierarchy.
- **Path** sorts by identifier rather than by display position.

## Depth and By Column

**Relationships By Column** spreads a unit's ancestors across a fixed set of columns — Ancestor 01, Ancestor 02 and so on — which is what makes a hierarchy easy to pivot or report on by level.

Because the columns are fixed, the table has a maximum depth. If a hierarchy grows deeper than that, **By Column Status** says so after Process runs, and the other two tables are unaffected — a hierarchy too deep to pivot still works everywhere else.

## Ragged hierarchies

A hierarchy does not have to be even. Units can sit at different depths, and the same unit can appear under more than one parent.

Where a unit appears twice, it genuinely appears twice — once for each place it sits — exactly as it would in a tree view. That is intended, and the derived tables are built per relationship rather than per unit so it works.

## Process

**Process** rebuilds the two derived tables for the hierarchy from its relationships.

Run it when:

- you have finished entering or changing relationships
- you have enabled or disabled a relationship
- a hierarchy has been loaded from a data file

Process reports what it found. As well as the counts on the header, it will tell you about:

- **duplicate relationships** — the same unit under the same parent twice
- **cycles** — a unit that ends up beneath itself
- **unreachable units** — a unit whose parent has no place in the hierarchy

These are reported rather than blocked. The hierarchy still processes, and the parts that are sound still work.

!!!Note
    Saving changes in the Relationships grid processes the hierarchy for you, so the derived tables stay in step as you work. The **Process** button is for when you want to run it deliberately — after a data load, or to see the summary.

## Create a new Unit Hierarchy

1. Go to **Admin | Workflow | Units**
2. Select **Unit Hierarchies**
3. Click on '**Add+**' or '**Add & Edit in Grid**'
4. Enter the name and display name and click **Save**
5. With the hierarchy selected, open **Relationships** and add a row for each unit, leaving **Parent Unit** blank for the top-level units
6. Click **Process**

**Add & Edit in Grid** allows adding and modifying rows directly in the grid

!!!Note Important Notes
    The Integration Code and Workflow Unit Hierarchy Id will be auto-assigned

    The workflow units must exist before they can be placed in a hierarchy

    See [General Actions](../../../../concepts/metadataDrivenUI/actions.md#general-actions) for more information about adding records

## Notes

- Build one hierarchy per purpose. A reporting structure and an approval structure that nearly agree are still two hierarchies.
- A unit's [Profile Type](units.md#unit-profile-type) and its place in the hierarchy are expected to agree: a **Base** unit sits at the bottom with nothing beneath it, a **Parent** unit has units under it.
- Disable a relationship rather than deleting it when a unit leaves a structure, so the history of the arrangement is kept.
- The derived tables are rebuilt in full on each Process run, so there is no cost to reprocessing and no state to clean up first.
