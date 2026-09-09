---
title: Release Notes
---

# Release Notes

What changed in each release of Revfore Framework.

Version labels combine the OneStream platform version the release targets with the Revfore solution version — **PV912-SV200** is solution 2.0.0 on OneStream platform 9.1.2. The documentation site is versioned to match, so the version selector at the top of the page shows the release each set of docs describes.

---

## PV912-SV200

Requires OneStream **9.1.2 or later**. See [Setup and Installation](setup/setup.md) for upgrade steps.

### Release theme

This release is about giving the Framework **context** — about what a solution is being built from, what it posts to, and what process it belongs to — so that less of that context has to be carried in a developer's head or restated in code. It comes together in three themes.

**AI-assisted development using Claude.** A solution can now be taken from a conversation through to the files that build it. The [Claude skill](integrations/aiModels/index.md) teaches Claude the Framework — its schema, conventions, standard columns, extension points and the shape of every file it produces — so what it generates follows the same rules a hand-built solution is held to. [Load and Extract](extending/import/index.md) closes the loop in the other direction: an existing application can be extracted back to JSON and handed to Claude as context before asking it to change anything.

**Cube Intelligence.** The Framework now understands the cube it sits next to. Cubes and dimension members are imported and kept in step by Sync, and new [standard columns](appGroups/admin/relational/supporting/standardColumns.md) let a relational column hold a cube reference, a dimension member or a time period directly. A row can name its own cube intersection, which is what removes the hand-written mapping that used to sit between a relational solution and the cube.

**Workflow Intelligence.** Workflow has been extended on both sides — Revfore workflow and its integration with OneStream workflow — and member sets give a workflow unit or item category a full cube intersection. That is what lets a detail table stay thin: the unit a record belongs to already determines where its data posts, so the context is resolved rather than re-entered.

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

### Standard Columns

Standard columns have grown from column templates into the vocabulary that carries meaning across a solution. The set now covers **cube references, dimension members, time periods and workflow linkage** as well as the familiar keys, names and audit columns — so a column that holds an entity member, a period or a workflow unit is declared as such rather than being an untyped integer the code has to interpret.

The full set of **117 standard columns is now documented**, grouped by purpose, with what each one carries and when to reach for it.

Six time period codes were **renamed** for consistency. The underlying standard columns are unchanged, so existing definitions and data are unaffected, but import files referencing the old codes must be updated: `DimGeneralPeriod_Decimal` and `DimYtdPeriod_Decimal` become `TimePeriodPeriodicAmount_Decimal` and `TimePeriodYtdAmount_Decimal`, and the `*Raw_Int`/`*Os_NVarchar` period and year codes become `TimePeriod_Int`/`TimePeriodName_NVarchar` and `TimeYear_Int`/`TimeYearName_NVarchar`.

See [Standard Columns](appGroups/admin/relational/supporting/standardColumns.md) and the [catalogue](appGroups/admin/relational/supporting/standardColumns.md#standard-column-catalogue).

### Audit Log

Change tracking is now a **property of a table rather than something each solution writes for itself**. Turn it on and inserts, updates and deletes are recorded automatically, with no handler code and nothing to remember at the call site.

Each entry records the action, the record it applied to, who made it and when, together with a **JSON payload of the values themselves** — a full snapshot for an insert or a delete, and the before-and-after values for an update, so a change can be read back long after the fact without reconstructing it from the current row.

- **Logging is configurable per action**, so a table can track updates and deletes while ignoring the noise of inserts
- **Parent and master record keys are captured alongside the record's own**, so a change to a detail row can be traced back to the header it belongs to rather than sitting in isolation
- **The standard audit columns are left out of the payload.** `CreatedAt`, `CreatedBy`, `UpdatedAt` and `UpdatedBy` are already recorded as part of the entry, so repeating them would only pad the log

The setting lives on the table itself — see [Tables](appGroups/admin/relational/relationalTables/tables.md). In an import file, set `EnableAuditLog` on the table definition.

### View Security

View security has grown from a single read/read-write pairing into **multiple configurable levels**, and those levels can be **linked to columns** — which is what makes field-level security possible.

Previously a view was one unit: a user could read it, or read and update it. Now a view can define as many levels as a solution needs, and each column can name the level it requires. The same view then serves users with different rights — a column is visible and editable, read-only, or absent — **without maintaining a near-duplicate view per audience**, and without the divergence that always follows from doing so.

Levels apply to view actions as well as columns, so what a user can *do* on a view is governed by the same mechanism as what they can see.

See [Security](appGroups/admin/relational/relationalViews/security.md).

### View Filters

Filters can now be **defined on a view and then selected when that view is added to a Genesis page**.

A filter is a named, described expression belonging to the view, so the logic is written and tested once, in one place. Where it previously took a separate view per variation to present the same data narrowed different ways, one view can now be placed on several pages, each choosing the filter appropriate to its context.

See [Filters](appGroups/admin/relational/relationalViews/filters.md) and [Genesis](integrations/genesis/index.md).

### Cube Intelligence

The Framework now understands the cube it sits next to.

- **Cubes and dimension members import from OneStream** and are refreshed by Sync, so they are available for selection throughout the application
- **New cube and dimension member standard columns** let a relational column hold a cube reference or a dimension member directly, which is what keeps relational data in step with the cube it posts to

Together these remove most of the hand-written mapping that previously sat between a relational solution and the cube.

See [Cubes](appGroups/admin/cube/cubes/index.md), [Dimension Members](appGroups/admin/cube/dimensions/index.md) and [Dimension Types](appGroups/admin/cube/supporting/index.md).

#### Workflow Unit Member Sets

A dimension member column says *which* member a row points at. A **workflow unit member set** says what that member means in context — it hangs a full cube intersection off a workflow unit, holding the cube together with its base entity, scenario, account, flow, intercompany and UD1–UD8 members, effective-dated so the intersection can change over time without rewriting history.

That context is what lets a detail table stay thin. On detail tables that ultimately post to the cube, the member set should be **assigned automatically from extension code** rather than captured on each row: the workflow unit the record belongs to already determines its intersection, so having the user restate it is both redundant and a source of error.

The resolved context is also serialised into the **`param_RfaSharedWf_MemberSetContextJson`** parameter, which is how cube views pick up their context. A cube view placed next to a relational grid reads the same intersection the workflow unit resolved to, so both halves of the page are looking at the same slice of the cube without any wiring between them.

See [Workflow Unit Member Sets](appGroups/admin/workflow/units/memberSets.md).

#### Item Category Member Sets

An **item category member set** does the same job one level down, against the item category rather than the workflow unit, and is typically used to **derive the account member from the category assigned to a record**.

This is the common case where a user picks something meaningful to them — a category of spend, a type of activity — and the account it posts to follows from that choice. Holding the mapping as an effective-dated member set keeps it configuration rather than code, and keeps it correct when the chart of accounts changes.

See [Item Category Member Sets](appGroups/admin/workflow/supporting/itemCategoryMemberSets.md) and [Member Set Types](appGroups/admin/workflow/supporting/memberSetTypes.md).

#### Cube views alongside relational data

Cube views can now be displayed **alongside a relational data grid**, so users can see cube data and the relational data that drives it on the same page instead of navigating between them.

See [Cube Views](appGroups/admin/relational/supporting/cubeViews.md).

### Workflow capabilities

Workflow has been extended on both sides — **Revfore workflow** (units, instances, areas, item types and categories) and its **integration with OneStream workflow**.

See [Workflow](appGroups/admin/workflow/units/index.md).
