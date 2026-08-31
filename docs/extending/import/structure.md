# Structure Definitions

[← Back to Import Files](index.md)

The `Tables`, `Models`, `Views` and `Lookups` sections define a solution's structure. This page covers what each one contains.

For the linking model and how to run an import, see [Import Files](index.md).

## The three layers

Revfore Framework separates what is stored from how it is assembled from how it is accessed, and the import file mirrors that:

```
Tables    what is stored          physical columns, indexes, foreign keys
   ↓
Models    how it is assembled     joins, stored and expression columns, column behaviour
   ↓
Views     how it is accessed      which columns are exposed, actions, security, filters
```

A table usually has one model over it, and a model usually has several views — a data entry view, a lookup view, a read-only view. That is why view names commonly carry a suffix such as `_mE` to distinguish them.

### Views are more than screens

It is tempting to read a view as "a screen", but it is the access layer for the data generally. The same definition serves every consumer:

| Consumer | Reads or writes through a view |
|---|---|
| **Screens** | The grids and forms users work in |
| **Dashboard adapters** | [Extension adapter dashboards](../../concepts/metadataDrivenUI/contentSubItem/customAdapterDashboard.md) bind to a view as their data source |
| **AI agents and integrations** | [REST APIs](../../integrations/restAPIs/index.md), [Excel](../../integrations/excel/index.md) and agent access all go through views |
| **Data imports** | [Data files](data.md) write their rows through a view, not into the table |
| **Extension code** | A `RecordSet` is bound to a view, not a table — see [Working with Records](../records.md) |

This is the reason so much configuration lives on the view rather than the table. Column security, required rules, defaults and lookups are defined once and then apply to **every** route into the data — a user typing into a grid, a seed file, an API call, or an agent. Enforcing a rule at the view means it cannot be bypassed by arriving through a different door.

It also explains a design habit worth adopting: give a model the views its consumers need rather than reusing one view everywhere. A data entry view, a lookup view and an integration view over the same model can expose different columns with different permissions, which is far easier to reason about than one permissive view shared by all of them.

## Tables

Defines a physical table.

```json
{
  "Name": "{RfaExtension}.CpxDepartment",
  "DisplayName": "Cpx Departments",
  "Alias": "CpxDept",
  "ObjectCategoryId_IntegrationCode": "RfaTable",
  "Columns": [ ... ],
  "Indexes": [ ... ],
  "Relationships": [ ... ]
}
```

**Required:** `Name`, `DisplayName`, `Alias`, `ObjectCategoryId_IntegrationCode`, `Columns`.

The **`Alias`** is the short prefix used when this table's columns appear in a model — `CpxDept` produces model columns like `CpxDept_Name`. Keep it short and distinctive; it shows up throughout the solution.

### Columns

```json
{
  "Name": "CpxDepartmentId",
  "DisplayName": "Id",
  "StandardColumnId_IntegrationCode": "Key_Int",
  "AutoIncrement": true,
  "IsPrimaryKey": true
}
```

**Required:** `Name`, `DisplayName`, `StandardColumnId_IntegrationCode`.

**Standard columns** are the framework's column templates. Referencing one — `Key_Int`, `Name_Short`, and so on — brings its data type, length and defaults with it, so column definitions stay short and consistent across solutions. See [Relational Standard Columns](../../appGroups/admin/relational/supporting/index.md).

Optional properties cover the usual overrides: `IsNullable`, `MaxLength`, `Precision`, `Scale`, `DefaultValue`, `IsEnabled`.

### Positioning columns

`SequenceNumber` sets a column's position outright. More often you want to place one relative to an existing column, which is what the position properties are for:

```json
"PositionAfterObjectColumnId_IntegrationCode": "{RfaExtension}.CpxDepartment|Name"
```

These work for both new and existing columns, so re-importing with a changed position moves the column rather than failing. The same pattern exists on view columns as `PositionBeforeViewColumnId_IntegrationCode` / `PositionAfterViewColumnId_IntegrationCode`.

### Indexes and relationships

`Indexes` define indexes on the table. `Relationships` define foreign keys to other tables, referenced — as everywhere — by integration code.

!!!Note Nullable foreign keys
    A foreign key on a nullable column should use a left join in the model, otherwise rows with no related record disappear from the view entirely. This is a common source of "my row vanished after I saved it".

## Models

Defines how tables are joined together, which columns are available to views — stored and calculated — and how each of those columns behaves.

This is the layer that does most of the work. Queries are written against **views**, but a view can only expose what its model makes available, and much of a column's behaviour is settled here rather than on the view.

```json
{
  "Name": "{RfaExtension}.CpxDepartment",
  "DisplayName": "Cpx Departments",
  "RelObjectId_IntegrationCode": "{RfaExtension}.CpxDepartment",
  "Sources": [ ... ],
  "Columns": [ ... ],
  "Relationships": [ ... ]
}
```

**Required:** `Name`, `RelObjectId_IntegrationCode`, `Sources`, `Columns`.

`RelObjectId_IntegrationCode` names the model's **base table** — the one whose rows the model returns.

### Sources

Each source is a table participating in the model, with the alias its columns will carry:

```json
{ "RelObjectId_IntegrationCode": "{RfaExtension}.CpxDepartment", "Alias": "CpxDept" }
```

The base table is the first source. Additional sources are joins — a lookup table whose display name you want to show, for instance. Each brings a join type and the relationship it joins on.

### Model columns

```json
{
  "Name": "CpxDept_CpxDepartmentId",
  "RelObjectColumnId_IntegrationCode": "{RfaExtension}.CpxDepartment|CpxDepartmentId"
}
```

The `Name` is the aliased form — source alias, underscore, column name. This is the name views and [extension code](../records.md#reading-and-writing-values) use, so it is worth getting the alias right early.

A model column does not have to map to a stored column. Set `Expression` instead, naming the sources it draws from (`ExpressionRelModelSource1Id_IntegrationCode` through `...Source5Id...`), and the value is calculated — which is how derived values are produced without any code.

Model columns also carry the column's **behaviour**, and this is where most of it is defined:

| Group | Properties |
|---|---|
| Keys and periods | `KeyFlags`, `PeriodFlags`, `PeriodValue` |
| Behaviour | `RequiredFlags`, `AllowUpdates`, `SecurityLevel`, `IsVisible` |
| Lookups | `RelLookupId_IntegrationCode`, `LookupRelModelSourceId_IntegrationCode`, `LookupDataSet` |
| Presentation | `DisplayName`, `FormatString` |
| Defaulting | `DefaultRelFunctionId`, `DefaultExpression` |
| On insert | `InsertFlags`, `InsertRelFunctionId`, `InsertExpression` |
| On update | `UpdateFlags`, `UpdateRelFunctionId`, `UpdateExpression` |
| On copy | `CopyFlags`, `CopyRelFunctionId`, `CopyExpression` |

The `Default`, `Insert`, `Update` and `Copy` sets are what let a column populate itself — a timestamp on insert, the current user on update, a cleared reference on copy — with no code involved. Define them here and they apply to every view over the model.

!!!Note Unset does not mean blank
    An attribute you leave off a model column inherits from its [Standard Column](../../appGroups/admin/relational/supporting/index.md), not from nothing. That is the point of standard columns — you set only what differs. The same is true of view columns.

## Views

Defines how a set of data is accessed — by a screen, an adapter, an API, an agent or an import.

```json
{
  "Name": "{RfaExtension}.CpxDepartment_mE",
  "DisplayName": "Cpx Departments",
  "RelModelId_IntegrationCode": "{RfaExtension}.CpxDepartment",
  "ObjectCategoryId_IntegrationCode": "RfaDataEntryView",
  "EditabilityModeFlags": "AlwaysAddableAndEditableAndDeletable",
  "Columns": [ ... ],
  "Actions": [ ... ],
  "Security": [ ... ],
  "Filters": [ ... ]
}
```

**Required:** `Name`, `RelModelId_IntegrationCode`, `ObjectCategoryId_IntegrationCode`, `Columns`.

`ObjectCategoryId_IntegrationCode` sets what kind of view it is — a data entry view, a lookup view, a read-only view — which determines how it behaves and where it can be used.

Optional `WhereClause` and `HavingClause` filter the view at the SQL level; `ParentViewId_IntegrationCode` makes it a child view of another.

### View columns

The only required property is which model column it shows:

```json
{ "RelModelColumnId_IntegrationCode": "{RfaExtension}.CpxDepartment|CpxDept_Name" }
```

A view column's job is to **select** a model column and settle how it appears and behaves in this particular view:

| Group | Properties |
|---|---|
| Display | `DisplayName`, `Width`, `IsVisible`, `SequenceNumber`, position properties |
| Behaviour | `AllowUpdates`, `RequiredFlags`, `SecurityLevel` |
| Lookups | `RelLookupId_IntegrationCode`, `LookupDataSet`, `LookupFilterExpression` |
| Grouping | `IsGroupBy`, `SummaryFunction`, `SummaryNames` |
| Sorting | `OrderBySequenceNumber`, `OrderByAscending` |
| Filtering | `FilterFlags`, `FilterExpression` |
| Defaulting | `DefaultRelFunctionId`, `DefaultExpression`, and the `Insert*`, `Update*`, `Copy*` equivalents |

Several of these names also appear on the [model column](#model-columns), which is deliberate — the model settles how a column behaves everywhere, and the view column settles it for one view. Set a rule on the model when it is true of the data, and on the view when it is true only of that screen or integration.

Grouping, sorting and filtering are genuinely view-level and have no model equivalent — they are about presenting a particular set of rows, not about the column itself.

### Actions, security and filters

`Actions` are the buttons on the view. Most are built-in; one flagged as custom is handled in [extension code](../actions.md). `Security` controls who can see and change the view. `Filters` define saved filters users can apply.

## Lookups

Defines a reusable dropdown.

```json
{
  "Name": "{RfaExtension}.CpxCategoryLookup",
  "BasedOnViewId_IntegrationCode": "{RfaExtension}.CpxCategory_mL",
  "KeyViewColumn_Name": "Cat_CpxCategoryId",
  "DisplayViewColumn_Name": "Cat_Name",
  "IsEnabledViewColumn_Name": "Cat_IsActive",
  "IsPrimary": true
}
```

**Required:** `Name`.

A lookup is defined once and referenced from any number of view columns, so the same list behaves identically everywhere it appears. `IsEnabledViewColumn_Name` is what lets a retired entry stay on historic records while disappearing from the dropdown for new ones.

## Notes

- Define tables, models and views together in the structure file; the framework resolves them in dependency order within it. Seed data goes in a separate file, imported after the sync.
- Get aliases right before anything references them; changing one later ripples through model columns, view columns and code.
- Prefer standard columns over spelling out types. Consistency across solutions is the point of them.
- Most behaviour people reach for code to do — defaults, required rules, dropdown filtering — is a view column property. Check [Configuration first](../index.md#configuration-first).
