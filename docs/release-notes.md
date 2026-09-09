---
title: Release Notes
---

# Release Notes

What changed in each release of Revfore Framework.

Version labels combine the OneStream platform version the release targets with the Revfore solution version — **PV912-SV200** is solution 2.0.0 on OneStream platform 9.1.2. The documentation site is versioned to match, so the version selector at the top of the page shows the release each set of docs describes.

---

## PV912-SV200

Requires OneStream **9.1.2 or later**. See [Setup and Installation](setup/setup.md) for upgrade steps.

### Users and security in your own tables

Users and security groups can now be integrated into extension tables directly, rather than being something the Framework only used internally.

You can:

- define **table relationships to the user and security tables**, so a record can own a genuine foreign key to a user or a group
- surface **user and security group dropdowns** on your own views, populated from the synced OneStream lists
- **join to the flattened user-security tables** to drive row-level user security

The flattening is what makes the last point practical. Because [Users : Security Groups By Row](appGroups/admin/security/Users/index.md) and [Security Groups : Relationships By Row](appGroups/admin/security/Groups/index.md) already resolve inherited access into rows, filtering a view to the current user is a simple join — no recursion through the group hierarchy, and no per-solution security code to write and maintain.

### Database Tables and Views lists

Two new pages under **Admin | Relational | Database** show what physically exists in the database, as opposed to what the Framework has defined.

From them you can:

- inspect an existing table or view — its **columns, relationships and underlying data** — without leaving the application
- see which objects are already linked to a Framework definition and which are not
- **generate Table, Model and View definitions for an object that already exists**, which is how you adopt a table the Framework did not create
- **drop** a table or view that is no longer needed

This closes the gap for applications with existing relational structures: they no longer have to be rebuilt to be managed by the Framework.

See [Database](appGroups/admin/relational/database/index.md), [Tables](appGroups/admin/relational/database/tables.md) and [Views](appGroups/admin/relational/database/views.md).

### Load and Extract JSON

A solution's structure and data now move in and out of an application as JSON files, in both directions.

- **Load** reads a structure or data file in — including the files [Claude generates](integrations/aiModels/index.md) from a design workbook
- **Extract** writes an existing solution back out to the same format

Extract is the more quietly useful half. It gives you a way to **migrate a solution between instances**, to recover the definition of something that was built before any file existed, and to **hand Claude the current state of an application** as context before asking it to change anything.

See [Load & Extract Files](extending/import/index.md).

### Cube View support

Cube views can now be displayed **alongside a relational data grid**, so users can see cube data and the relational data that drives it on the same page instead of navigating between them.

See [Cube Views](appGroups/admin/relational/supporting/cubeViews.md).

### Cube Intelligence

The Framework now understands the cube it sits next to.

- **Cubes and dimension members import from OneStream** and are refreshed by Sync, so they are available for selection throughout the application
- **New cube and dimension member standard columns** let a relational column hold a cube reference or a dimension member directly, which is what keeps relational data in step with the cube it posts to

Together these remove most of the hand-written mapping that previously sat between a relational solution and the cube.

See [Cubes](appGroups/admin/cube/cubes/index.md), [Dimension Members](appGroups/admin/cube/dimensions/index.md) and [Dimension Types](appGroups/admin/cube/supporting/index.md).

### Workflow capabilities

Workflow has been extended on both sides — **Revfore workflow** (units, instances, areas, item types and categories) and its **integration with OneStream workflow**.

See [Workflow](appGroups/admin/workflow/units/index.md).
