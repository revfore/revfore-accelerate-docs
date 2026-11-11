---
title: Release Notes
---

# Release Notes

What changed in each release of Revfore Framework.

Version labels combine the OneStream platform version the release targets with the Revfore solution version — **PV921-SV200** is solution 2.0.0 on OneStream platform 9.2.1. The documentation site is versioned to match, so the version selector at the top of the page shows the release each set of docs describes.

---

## PV921-SV200

Requires OneStream **9.2.1 or later**. See [Setup and Installation](setup/setup.md) for upgrade steps.

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

### Where clauses that know who is looking

A view's where clause can now be applied **every time the view is read**, with the user's own workflow - not only built into the view. Set its Flags to 2 and `{WfCycleYear}`, `{CurrentUserId}` and the other user and workflow values resolve per user, across the grid, lookups, Top Count and File Export. A built-in clause can also filter on **model columns the view doesn't show** (Flags 5), so a summary view no longer needs hidden columns - and the extra grouping they bring - just to filter on.

A built-in clause that uses a user or workflow value is now refused, because it would hold the value of whoever synced the view.

See [Where Clause](appGroups/admin/relational/relationalViews/views.md#where-clause).

### Update-only and insert-only data

A data file entry can now say how its rows meet the existing ones. **Update** changes only rows that are already there, so a file can switch one setting on core rows without restating them — and a mistyped code is reported, not inserted. **Insert** adds only new rows and leaves existing ones alone, so starter data the customer has since edited survives every upgrade.

See [Update only, or insert only](extending/import/data.md#update-only-or-insert-only).

### Removing things through a load

A structure file can now remove model columns, sources and relationships, and view columns, actions, filters and security levels — mark the entry `"Remove": true`. Removing a model column takes its view columns with it, in every view. Nothing is removed while something still uses it, and the message says what does. Upgrades no longer need a list of things to delete by hand.

See [Removing columns and other items](extending/import/structure.md#removing-columns-and-other-items).

### Top N views

A view can now show just its first rows — a top 10 by sales, the five largest variances — with **Top Count**. The rows are ranked by the view's own sort and chosen after every filter, workflow included, so each unit gets its own top 10. Counts, footer totals and File Export all cover the same rows.

See [Top Count](appGroups/admin/relational/relationalViews/views.md#top-count).

### Grid data as JSON

**File Export** and **File Import** on a grid now handle JSON as well as Excel. The file is the same [data file](extending/import/data.md) format Load takes, so it moves data between environments — lookups travel as integration codes, not ids.

An export takes the rows the grid's filters select and **every level of their child records**, so an order goes out with its lines. An import is reviewed and saved like an Excel import, with the same save logic, and stays within what the grid shows: a row its filters hide is refused rather than updated, and child rows must belong to a parent in the grid or the file.

See [File Export and Import as JSON](concepts/metadataDrivenUI/actions.md#file-export-and-import-as-json).

### Standard Columns

Standard columns have grown from column templates into the vocabulary that carries meaning across a solution. The set now covers **cube references, dimension members, time periods and workflow linkage** as well as the familiar keys, names and audit columns — so a column that holds an entity member, a period or a workflow unit is declared as such rather than being an untyped integer the code has to interpret.

The full set of **117 standard columns is now documented**, grouped by purpose, with what each one carries and when to reach for it.

Six time period codes were **renamed** for consistency. The underlying standard columns are unchanged, so existing definitions and data are unaffected, but import files referencing the old codes must be updated: `DimGeneralPeriod_Decimal` and `DimYtdPeriod_Decimal` become `TimePeriodPeriodicAmount_Decimal` and `TimePeriodYtdAmount_Decimal`, and the `*Raw_Int`/`*Os_NVarchar` period and year codes become `TimePeriod_Int`/`TimePeriodName_NVarchar` and `TimeYear_Int`/`TimeYearName_NVarchar`.

See [Standard Columns](appGroups/admin/relational/supporting/standardColumns.md) and the [catalog](appGroups/admin/relational/supporting/standardColumns.md#standard-column-catalog).

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

View security is now also checked **wherever data is changed or taken out**, not only when a view is drawn: saving in the grid, File Import, data loads, Delete, Copy and Bulk Update need write access to the view and to each column being set, and File Export and Extract leave out columns the user can't read.

### Security Roles

A new **[Security Roles](appGroups/admin/security/Roles/index.md)** page (**Admin | Security | Roles**) maps permissions to your own security groups, so running the Framework no longer needs full OneStream administrator rights.

- **Revfore Administrator** configures the Framework's extension tables, models, views and pages, syncs with OneStream, loads setup packages that contain no SQL, and manages debug settings, impersonation and the license key.
- **Developer** switches debug logging on for a user.
- Installing, upgrading and uninstalling Revfore, anything that runs SQL, changing roles, and changing Revfore's own tables, models and views stay with OneStream administrators.
- Both roles start out mapped to **Administrators**, so nothing changes until you map a group of your own.

Solutions can add roles of their own and check them in code — see [Security Roles](appGroups/admin/security/Roles/index.md).

### License keys

The license now applies wherever your tables, models and views are used, not only when a page opens. File imports
and exports, setup packages, structure sync and solution code are all covered.

- A **Starter** key lets OneStream administrators use the solution. Everyone else needs a **Full** key, including in
  designer mode.
- With no valid key, installing, upgrading and entering a license key still work, but your tables, models and views
  can't be used. Enter a key before you create any.

See [Manage License Key](appGroups/config/manage-license-key.md).

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

A dimension member column says *which* member a row points at. A **workflow unit member set** says what that member means in context — it hangs a full cube intersection off a workflow unit, holding the cube together with its base entity, account, flow, intercompany and UD1–UD8 members (the scenario comes from the cycle's workflow scenario), effective-dated so the intersection can change over time without rewriting history.

That context is what lets a detail table stay thin. On detail tables that ultimately post to the cube, the member set should be **assigned automatically from extension code** rather than captured on each row: the workflow unit the record belongs to already determines its intersection, so having the user restate it is both redundant and a source of error.

The resolved context is also serialized into the **`param_RfaSharedWf_MemberSetContextJson`** parameter, which is how cube views pick up their context. A cube view placed next to a relational grid reads the same intersection the workflow unit resolved to, so both halves of the page are looking at the same slice of the cube without any wiring between them.

See [Workflow Unit Member Sets](appGroups/admin/workflow/units/memberSets.md).

#### Item Category Member Sets

An **item category member set** does the same job one level down, against the item category rather than the workflow unit, and is typically used to **derive the account member from the category assigned to a record**.

This is the common case where a user picks something meaningful to them — a category of spend, a type of activity — and the account it posts to follows from that choice. Holding the mapping as an effective-dated member set keeps it configuration rather than code, and keeps it correct when the chart of accounts changes.

See [Item Category Member Sets](appGroups/admin/workflow/supporting/itemCategoryMemberSets.md) and [Member Set Types](appGroups/admin/workflow/supporting/memberSetTypes.md).

#### Cube views alongside relational data

Cube views can now be displayed **alongside a relational data grid**, so users can see cube data and the relational data that drives it on the same page instead of navigating between them.

See [Cube Views](appGroups/admin/relational/supporting/cubeViews.md).

### Workflow capabilities

Workflow has been extended on both sides — **Revfore workflow** (units, cycles, areas, item types and categories) and its **integration with OneStream workflow**.

See [Workflow](appGroups/admin/workflow/units/index.md).

### Workflow instances are now workflow cycles

The workflow **instance** has been renamed **cycle**, which is what a budget, a forecast round or a close already is to the people running it. The rename runs through the database, the code and the documentation:

- Tables: `WfInstance` is now `WfCycle`, `WfInstanceType` is `WfCycleType`, `WfInstanceType_MemberSet` is `WfCycleType_MemberSet`, `WfInstanceType_Time` is `WfCycleType_Time` and `WfInstance_Unit` is `WfCycle_Unit`. Their key columns follow (`WfInstanceId` is `WfCycleId`, `WfInstanceTypeId` is `WfCycleTypeId`, and so on), as do their views and lookups (`WfCycle_mE`, `{RfaCore}.WfCycle_Lookup`, `{RfaCore}.WfCycleType_Lookup`) and model column prefixes (`WfInst_` is `WfCyc_`, `WfInstTyp_` is `WfCycTyp_`, `WfInstUnt_` is `WfCycUnt_`).
- The standard column `WorkflowInstance_Int` is now `WorkflowCycle_Int`.
- Workspace: the workflow dashboard parameters, the `GetWfCycleList` dataset, the Admin page (`Admin_WorkflowGroup_Cycles`) and the workflow dialog's argument keys use the new names too.
- Expression tokens: `{WfInstanceId}` is now `{WfCycleId}`, and likewise `{WfCycleName}`, `{WfCycleDisplayName}`, `{WfCycleStartDate}`, `{WfCycleYear}`, `{WfCycleTypeId}` and `{CurrentUserReadableWfCycleIds}`.
- Extension code: `rfa_wf.Instance` is now `rfa_wf.Cycle` (with `CycleType`, `CycleTypeMemberSet`, `CycleTypeTime` and `CycleUnit`), and the workflow context's `WfInstanceId`, `InstanceYear` and related members are now `WfCycleId`, `CycleYear` and so on.
- Admin pages: **Instances**, **Instance Types** and **Instance Units** are now [Cycles](appGroups/admin/workflow/cycles/cycles.md), [Cycle Types](appGroups/admin/workflow/cycles/cycleTypes.md) and [Cycle Units](appGroups/admin/workflow/cycles/cycleUnits.md).

**Update your solution files.** Import JSON and extension code written for an earlier release still use the old names. Search and replace `WorkflowInstance_Int` with `WorkflowCycle_Int`, `{RfaCore}.WfInstance` with `{RfaCore}.WfCycle`, your tables' `WfInstanceId` columns with `WfCycleId`, and the `{WfInstance...}` expression tokens with their `{WfCycle...}` forms.

### Unit hierarchies

Workflow units can now be arranged **under one another**, so a unit higher up a structure can see and review the work of the units beneath it. Without this, every unit was an island: it had its own data and nobody had a view across it.

A hierarchy is one record and one grid of parent-and-child pairs. From that, **Process** builds two further tables the application maintains for you — every ancestor-to-descendant pair at any depth, and a pivoted form with each unit's ancestors spread across columns. Those are what turn *"every unit below this one, however deep"* into a single join instead of a recursive walk.

A unit can belong to more than one hierarchy, because a reporting structure and an approval structure are rarely the same shape, and units can sit at different depths or appear under more than one parent.

See [Unit Hierarchies](appGroups/admin/workflow/units/hierarchies.md).

### Per-unit workflow status

A cycle used to have a single status covering everything in it. That is enough to open and close a round of work, but not to run a submission process — within one open cycle, a department may still be drafting while another has submitted and a third has been approved.

Each unit now carries **its own status within a cycle**, alongside who submitted it, when, and any review comments. The two levels combine as restrictions: whatever either forbids is forbidden, so a submitted unit is locked while its cycle stays open, and everything in a closed cycle is locked whatever the individual units say.

Records are created for a unit and everything beneath it when a user sets their workflow context, so a submission grid has something in it when they arrive. Set the cycle's **Default Unit Status** to turn this on.

See [Cycle Units](appGroups/admin/workflow/cycles/cycleUnits.md).

### Workflow scenarios

A new **[Workflow Scenarios](appGroups/admin/workflow/cycles/workflowScenarios.md)** page (**Admin | Workflow | Cycles**) defines what a cycle plans — Budget, Forecast, Actual. Each names one OneStream scenario member and can override the unit's Flow and UD members, effective-dated.

- A cycle now has a **Workflow Scenario** instead of a scenario member. There is one cycle per cycle type, workflow scenario and year.
- A branch or version of a plan, such as "Budget - Upside", is its own workflow scenario: the same scenario member, with different UD members.
- The scenario has one owner: the unit member set's **Base** and **Scope Scenario** columns have been removed.
- The item category member set's **Base Scenario** column has been removed too; the scenario comes from the workflow scenario.

### Cycle type member sets and the data source

The members every unit of a cycle type shares are now set once, on the cycle type, instead of on every unit.

- **Cycle types** carry a **[Member Sets](appGroups/admin/workflow/cycles/cycleTypes.md#member-sets-every-units-defaults)** grid: per cube and effective-dated, the default Account, Flow, IC and UD members (base and scope) and the [data source](appGroups/admin/workflow/cycles/cycleTypes.md#data-source) for every unit of the type. Blank Flow, IC and UD members are filled with the cube's default member when you save.
- A **[unit member set](appGroups/admin/workflow/units/memberSets.md)** now holds only the unit's exceptions — usually just its Entity. A blank member takes the cycle type's, and a cube the unit has no member set for gets the cycle type's alone. Saving a unit member set now defaults only a blank Base Entity.
- The data source dimension and its Readable, Controlled, Controlled Itemized, Controlled Direct and Controlled Actual members are set once per cube on the cycle type's member set (**Data Source Dimension Type** and the five **Data Source ... Scope** columns). The unit member set's **Data Source** columns have been removed.
- The data source is the cycle type's alone: a unit, a **[workflow area](appGroups/admin/workflow/areas/index.md)** or an **[item type](appGroups/admin/workflow/supporting/itemTypes.md)** cannot name its own. The **Data Source Dimension** column has been removed from workflow areas and item types; their **Data Source Dim Member** is a member of the cycle type's data source dimension.

### Comparing cycles

A cycle can compare with up to two **workflow scenarios**, each at a **year offset** from its own year — a budget against last year's actuals, a forecast against the prior one. No cycle has to exist to be compared with: the compare-to member sets are built from the workflow scenario and year. Users can choose their own in the change workflow dialog. The values reach the solution without a lookup: in the workflow context, as a full compare-to member set beside each of the cycle's own, and as tokens for view and lookup filters.

- New tokens `{WfScenarioId}`, `{WfScenarioName}`, `{WfScenarioDisplayName}` and `{WfScenarioMemberName}`, and per compare-to `{WfCompareTo1ScenarioId}`, `{WfCompareTo1ScenarioDisplayName}`, `{WfCompareTo1ScenarioMemberName}` and `{WfCompareTo1Year}` (and `2`).
- `{WfCompareTo1ScenarioName}` is now the workflow scenario's name; use `{WfCompareTo1ScenarioMemberName}` for the member.
- A new member set field, `ScenarioMemberFilter`, gives a compare-to's scenario plus every dimension where its member set differs from the cycle's (`S#Forecast:U5#V1`), so a cube view compare-to column picks up overrides without knowing which dimensions they are.
- A cycle type's new **Show Compare To** setting hides the compare-to fields in the change workflow dialog, for solutions that use workflow without posting to the cube or comparing.
- `{WfInstanceScenarioName}`, `{WfCompareTo1InstanceId}` and `{WfCompareTo1DisplayName}` (and `2`) have been removed.

See [Compare-to](appGroups/admin/workflow/cycles/cycles.md#compare-to).

### Statuses are now yours to define

Workflow statuses were a fixed list built into the application. They are now records you maintain, so the stages can be named after your own process rather than around someone else's.

Each status says what it **restricts** — a status marked Read Only stops data being entered — and whether it applies to a whole cycle, to a single unit, or to both. Restrictions rather than permissions is what lets the cycle and unit levels combine without precedence rules.

See [Statuses](appGroups/admin/workflow/cycles/statuses.md).

### Base and Parent units

A workflow unit now declares whether it sits at the bottom of a structure or **groups other units**. Both kinds can hold records of their own — a Parent unit is not a heading, and may enter its own data as well as review what is beneath it.

The profile type is a label and nothing more — what a Parent unit actually sees and may change is decided per view, not per unit.

That split is what makes a review screen possible. The same Parent unit is typically read-only on a data entry screen and fully editable on the review screen beside it, and a single setting on the unit could not express both.

See [Unit Profile Type](appGroups/admin/workflow/units/units.md#unit-profile-type).

### Views know which units they are for

Two new settings on a Relational View decide how it behaves for the unit looking at it:

- **Unit Records Shown** — whether the view returns the unit's own rows, the rows of the units below it in a hierarchy, or both. The default is what every view did before the setting existed, so nothing changes by accident.
- **Parent Unit Editability** — what a Parent unit may do here. Left blank it is read-only, which is the safe default for a unit looking across other units' data. Set, it *replaces* the view's editability for a Parent, which is how a review screen stays editable for an approver on a grid nobody else can change.

Where a view genuinely shows more than one unit's rows, the workflow unit column appears by itself. Nothing needs configuring, and it stays hidden everywhere else rather than repeating a single value down the page.

See [Workflow behavior](appGroups/admin/relational/relationalViews/views.md#workflow-behavior).

### Workflow actions

View actions can now be marked as **Workflow** actions — Submit, Approve, Reject, Recall and the Calculate that runs with them.

A workflow action is gated on the **cycle** being open, not on the unit's own status. That distinction matters: a unit-level read-only status freezes that unit's *data*, not the workflow's ability to move it on. Marked as an edit action instead, a Submit button would disable itself the moment it succeeded, and nothing could ever recall it.

See [Availability and the Crud flags](appGroups/admin/relational/relationalViews/actions.md#availability-and-the-crud-flags).

### Debug logging per user

Administrators can turn on **Debug** for a single user on **Admin | Security | Users**, optionally until a set date. While it is on, the application writes detail about what that user does to the OneStream error log — the queries behind their grids and drop-downs, why a screen is read-only, security decisions, which custom code ran and how long things took — tagged with their name so it can be filtered out.

It turns "I can't reproduce it" into a log to read. Custom code can add its own lines with `RF.ErrorLog.Debug`, which costs nothing for anyone without Debug on.

See [Debug logging](appGroups/admin/security/Users/index.md#debug-logging).

### Columns and actions for some units only

View columns and view actions have a new **Unit Profile Types** setting that limits them to **Base** units, **Parent** units, or both.

That turns what used to be a single built-in rule — the workflow unit column shown to Parent units on review views — into something any column or action can say for itself. A submission screen can now show **Submit** and **Recall** to the units doing the work and **Approve** and **Reject** to the units reviewing it, on the same grid. A restricted action is refused if it is run anyway, not just hidden.

Blank means every unit, which is what every column and action did before, so nothing changes on upgrade.

See [Showing a column to some units only](appGroups/admin/relational/relationalViews/columns.md#showing-a-column-to-some-units-only) and [Actions for some units only](appGroups/admin/relational/relationalViews/actions.md#actions-for-some-units-only).

### Core and extension logic together

A Relational View can now say **which assembly handles its save and delete logic, and in what order** — core, extension, or both in either order.

Previously it was one or the other, decided by whether the view was an extension. Adding a rule to a core view meant taking over its behavior entirely and reproducing whatever the core already did. Now *Core, then Extension* leaves the framework's own validation and stamping in place and runs your handler afterwards on the same save.

See [Business Rule Flags](appGroups/admin/relational/relationalViews/views.md#business-rule-flags).

### Security that stays out of the way

Security now works quietly in the background instead of interrupting people.

- **No more blocking errors for views you can't open.** Previously, opening a page with a view you had no access to raised an error that could leave OneStream unresponsive. Now the view is simply shown empty — no columns, records or buttons — with *"You don't have access to this view"* in its title, and the rest of the page keeps working. Clicking through to such a view still tells you plainly that you don't have rights to it.
- **Lists only offer what you can open.** The **Navigate To** tree and the **Edit+** Child Objects list leave out views you don't have access to, and Edit+ opens even when none of a record's child views are open to you.
- **Workflow shows only your units and cycles.** Workflow selectors, unit and cycle drop-downs, and "this unit and below" grids show only the units and cycles whose read group you belong to — including through nested groups. Parent units you can't read stay in the unit tree, so you can still reach the units beneath them.
- **The workflow header appears in the Navigate dialog** as it does on the main grids: view, unit, cycle, cycle status and unit status.
- **Page settings are protected** against being altered between requests, so a page can't be tricked into showing data its settings would not allow.

See [What You Can See and Do](security/index.md).

### Viewing as another user

Administrators can now **impersonate** another user — see the application exactly as that user does, with their views, columns, buttons, workflow units and records — to check access or troubleshoot without borrowing anyone's sign-in.

- Impersonation is **read-only**: nothing can be changed while it is on, and anything recorded still names the administrator.
- It is set by an administrator **on their own user record**, and only for a user whose access the administrator **already has**, so it can never reveal more than they could already see.
- The **Users** page always runs as the administrator, so impersonation can always be turned off.

See [Viewing as Another User](security/impersonation.md).

### Setup packages

A solution now installs and upgrades as **one zip**. Its structure, data, page settings and any SQL of its own go into its setup package, `<SolutionCode>SetupPackage.zip`, which runs the steps in order — import, sync, load — so nobody loads files or presses Sync by hand.

- **One file installs and upgrades.** Steps can run always, on install only, or on upgrade from a given version, so an install that skips releases still runs each migration it needs and pressing Setup again changes nothing.
- **Built and checked locally.** The [Extension Kit](extending/extension-kit.md) builds the zip from a short manifest, validating every file in it and refusing a data load with no sync before it.
- **Several solutions per application.** Loading a package keeps it in the workspace's `rfaExtensionSetupPackage.zip` beside every other solution's, and **Setup and Upgrade** runs them all, in order. A workspace zip of several solutions installs them in one load.
- **Reloadable while you build.** **Load** takes the zip as well as a JSON file, and reloading the release you are building reruns its steps. Loading a new release runs what Setup would, so customers keep their page changes.
- **Only an administrator** who is not impersonating can run one, because a package can run SQL.

Solutions that need code at setup still have it: the setup method in the extension assembly runs the packages, and custom code goes either side of it.

See [Setup Packages](extending/setup-packages.md).

### Extension Kit and OneStream Developer Studio

The [Extension Kit](extending/extension-kit.md) now gets its code from your OneStream environment rather than carrying a copy. The kit ships its `rfaRef` and `rfa_actnExtension_os` folders empty, and you download both assemblies into them with OneStream Developer Studio, so the kit always matches the release you're running.

- **Claude Code hooks up the projects.** After the download, Claude Code adds the target framework and the package and project references the build needs.
- **Pushed back the same way.** Your handlers deploy with a Developer Studio push of `rfa_actnExtension_os`. With ODS's own skill installed, Claude Code previews the push and pushes only after you confirm.
