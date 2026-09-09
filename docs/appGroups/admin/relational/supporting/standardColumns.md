# Standard Columns

[← Back to Relational Supporting Overview](index.md)

The **Relational Standard Columns** page defines the reusable column definitions that table columns are built from.

A standard column carries the data type, key behaviour, nullability, default value, and display settings for a kind of column. When a table column is created from a standard column it inherits all of that, so columns of the same kind behave consistently everywhere.

## Overview

Use the Relational Standard Columns page to:

- review the standard columns available when defining table columns
- see the data type, key flags and defaults each one carries
- see which settings a table column will inherit if it does not override them

Standard columns are supplied with the platform. Most solutions consume them rather than adding new ones.

## Relational Standard Column Record Fields

The following fields are used for a Relational Standard Column record.

| Field | Data Type| Purpose | Notes |
|---|---|---|---|
| Standard Column Name | nvarchar | Name of the standard column. | This is the value selected when defining a table column.
| Standard Column Description | nvarchar | Description of the standard column and what it is for. |
| Table/View Column Name | nvarchar | The default column name applied when this standard column is used. |
| Table/View Column Display Name | nvarchar | The default display name applied when this standard column is used. |
| Standard Base Column | int | The base column definition this standard column derives from. | Groups related standard columns that share underlying behaviour.
| Key Flags | int | How the column participates in keys. | Identifies primary, foreign, integration and parent key behaviour, which drives how the column is treated across models and views.
| Data Type | int | The column's data type. |
| Is Nullable | bit | Whether columns created from this standard column allow nulls by default. | A table column can override this.
| Max Length | int | Default maximum length for text columns. |
| Precision | int | Default precision for numeric columns. |
| Scale | int | Default scale for numeric columns. |
| Auto Increment | bit | Whether the column is an identity column. | Only applies to a table's own identity primary key.
| Default Value | nvarchar | Default value applied to the physical column. |
| Bulk Update Flags | int | Whether and how the column can be bulk updated. |
| Is Display Label | bit | Whether the column is used as the display label for its record. |
| Filter Flags | int | How the column can be filtered. |
| Filter Expression | nvarchar | Expression used when filtering on the column. |

## Key Concepts

- **Key Flags is the most consequential setting.** It determines whether a column is treated as a primary key, a foreign key needing a lookup, or the integration code a record is identified by.
- Choosing the right standard column matters more than the column's own settings — it is what makes columns of the same kind behave alike.
- A standard column is referenced by its **integration code**, which follows a `Purpose_DataType` convention: `Key_Int`, `Name_NVarchar`, `DmMemberEntity_Int`. The suffix tells you the storage type; the prefix tells you what the column means to the Framework.

## Standard Column Catalogue

The platform supplies **117 standard columns**. They are grouped below by purpose, with the integration code used to reference each one from an [import structure file](../../../../extending/import/structure.md).

The **Nullable** column is the default a table column inherits; a table column can override it.

### Primary keys

Every managed table has exactly one. All four carry `KeyFlags: Primary`, and the integer and GUID variants are not updatable once set.

| Code | Nullable | Description |
|---|---|---|
| `Key_Int` | No | The standard auto-increment surrogate primary key, used on almost every managed table. Pair with `IsPrimaryKey: true` and `AutoIncrement: true`. |
| `Key_BigInt` | No | Use instead of `Key_Int` only when the table is expected to exceed roughly two billion rows. |
| `Key_UniqueIdentifier` | No | GUID primary key, for tables needing globally unique keys — cross-instance sync, for example. |
| `Key_NVarchar` | No | String primary key. Rare; prefer `Key_Int` unless the natural key genuinely is a string. |

### Foreign keys

The `ForeignKey_*` codes carry `KeyFlags: Foreign` and are nullable. The `ForeignKeyParent_*` codes carry `KeyFlags: ForeignAndParent` and are **not** nullable — a child row must have a parent.

| Code | Nullable | Description |
|---|---|---|
| `ForeignKey_Int` | Yes | Standard foreign key to another table's `Key_Int`, used for Primary and Secondary relationships (lookups). |
| `ForeignKeyParent_Int` | No | Foreign key for a Child → Parent relationship, where the relationship's `CategoryFlags` is `Child`. |
| `ForeignKey_BigInt` | Yes | Foreign key variant for a related table keyed on `Key_BigInt`. |
| `ForeignKeyParent_BigInt` | No | Parent foreign key variant for a related table keyed on `Key_BigInt`. |
| `ForeignKey_UniqueIdentifier` | Yes | Foreign key variant for a related table keyed on `Key_UniqueIdentifier`. |
| `ForeignKeyParent_UniqueIdentifier` | No | Parent foreign key variant for a related table keyed on `Key_UniqueIdentifier`. |

### Naming, numbering and integration

| Code | Nullable | Description |
|---|---|---|
| `Name_NVarchar` | No | The record's short display and lookup name. Carries `IsDisplayLabel: true` and joins the composite filter, so it is what users search on. Almost always paired with a unique index. |
| `NameDisplay_NVarchar` | No | A separate, longer display-only name, for when `Name_NVarchar` has to stay short because it is a code. |
| `Description_NVarchar` | Yes | Free-text description. Already nullable — there is no need to set `IsNullable` on it. |
| `Number_Int` | No | A numeric business or document number, distinct from the primary key. |
| `Number_BigInt` | No | Numeric business or document number for high-volume tables. |
| `Number_NVarchar` | No | String business or document number, typically with a prefix pattern as its default value, such as `CpEx-10000`. |
| `IntegrationKey_NVarchar` | No | External-system key for import and sync scenarios. Carries `KeyFlags: Integration`, which is how a record is identified across instances. |

### Status flags

Both default to true and appear as a header filter on views. Which one you use is a matter of the domain's wording — the workflow item tables use `IsEnabled`.

| Code | Nullable | Description |
|---|---|---|
| `IsActive_Bit` | No | The near-universal "is this record active" flag. |
| `IsEnabled_Bit` | No | Enabled/disabled flag, used interchangeably with `IsActive_Bit`. |

### Audit columns

Include all four on every managed table. They bring their own auto-population wiring — current timestamp, current user, on insert, update and copy — and are not user-updatable, so they need no configuration beyond being present.

| Code | Nullable | Description |
|---|---|---|
| `CreatedAt_DateTime2` | No | Timestamp set on insert and on copy. |
| `CreatedBy_Int` | No | Foreign key to the user who created the record. Wire a Secondary relationship to `{RfaCore}.AuthUser` with `JoinAliasSuffix` `Cre`. |
| `UpdatedAt_DateTime2` | No | Timestamp set on insert, update and copy. Also offered as a header filter. |
| `UpdatedBy_Int` | No | Foreign key to the user who last updated the record. Wire a Secondary relationship with `JoinAliasSuffix` `Upd`. |

### Effective dating

| Code | Nullable | Description |
|---|---|---|
| `EffectiveStartDate_Date` | No | Start of an effective-dated record's validity window. |
| `EffectiveEndDate_Date` | No | End of an effective-dated record's validity window. |

### Workflow

These are what tie an extension table into workflow — both **Revfore workflow** and **OneStream workflow**. Adding them is what makes a solution's records workflow-aware rather than free-floating: the record knows which unit and instance it belongs to, so it can be filtered, secured and progressed by the same mechanics that drive every other workflow-enabled view.

The first six are foreign keys into the Revfore workflow tables. All carry `KeyFlags: Foreign` and appear as **header filters** on a view, which is what gives users the workflow selectors at the top of a grid without any configuration.

| Code | Nullable | Description |
|---|---|---|
| `WorkflowInstance_Int` | No | Foreign key to `{RfaCore}.WfInstance`. Use on any table whose rows hang off a specific workflow instance — a period's run of a process — as `WfItem` does. This is the column that scopes a record to one execution of a workflow. |
| `WorkflowUnit_Int` | No | Foreign key to `{RfaCore}.WfUnit`. The unit is the *who* of workflow — the entity, region or team a row of work belongs to — and is what row-level workflow security is applied against. |
| `WorkflowItemType_Int` | No | Foreign key to `{RfaCore}.WfItemType`. Classifies what kind of item a row is, which drives the behaviour and actions offered for it. |
| `WorkflowItemCategory_Int` | No | Foreign key to `{RfaCore}.WfItemCategory`. Groups item types into categories for reporting and filtering. |
| `WorkflowArea_Int` | No | Foreign key to `{RfaCore}.WfArea`. The area a workflow item sits within, used to partition a large process into manageable sections. |
| `WorkflowMemberSetType_Int` | No | Foreign key used for dimension-member-set type linkage on workflow unit tables — how a workflow unit resolves to the set of dimension members it covers. |

The remaining four store **OneStream's own workflow keys**, so a Revfore record can be cross-referenced against the OneStream workflow it corresponds to. Populate them when a solution has to reconcile against, or hand off to, native OneStream workflow — for example to confirm a OneStream workflow step is complete before allowing a relational action.

| Code | Nullable | Description |
|---|---|---|
| `OneStreamWorkflowKey_UniqueIdentifier` | No | The OneStream workflow key. |
| `OneStreamWorkflowProfileKey_UniqueIdentifier` | No | The OneStream workflow profile key. |
| `OneStreamWorkflowScenarioKey_Int` | No | The OneStream workflow scenario key. |
| `OneStreamWorkflowTimeKey_Int` | No | The OneStream workflow time key. |

!!! note "Workflow key columns are hidden on views by default"
    A view column built on a workflow key inherits `IsVisible: false` regardless of what the model column says. The keys do their work behind the grid — driving filtering and security — rather than being shown as columns. Set visibility explicitly only when you genuinely want the key on screen.

See [Workflow](../../workflow/units/index.md) for the tables these point at.

### Cube linkage

These identify **which cube** a relational row relates to, and are the outer frame of cube intelligence: a row that carries a cube reference can be resolved against the right cube without hard-coded mapping.

Each reference comes in two forms. The `_Int` form stores the internal key and is a genuine foreign key to the synced cube list, so it validates and gives users a dropdown of real cubes. The `_NVarchar` form stores the name as text, which is the form OneStream itself works in — use it where the value will be handed to the cube, and the `_Int` form where the value has to be selected and enforced.

| Code | Nullable | Description |
|---|---|---|
| `CbCube_Int` | Yes | Reference to a OneStream cube by internal id. Carries `KeyFlags: Foreign`. |
| `CbCube_NVarchar` | Yes | Cube reference stored by name. |
| `Dim_Int` | Yes | Generic dimension reference by id. Carries `KeyFlags: Foreign`. Prefer a specific `DmMember*` code when the dimension is known. |
| `Dim_NVarchar` | Yes | Generic dimension reference by name. |
| `DmType_Int` | Yes | Reference to a dimension *type* — Entity, Account, Flow and so on — rather than a member of one. |
| `DmType_NVarchar` | Yes | Dimension type reference by name. |
| `DmTypeDataSource_Int` | Yes | Dimension type reference scoped to the data source dimension. |
| `DmTypeDataSource_NVarchar` | Yes | Dimension type reference scoped to the data source dimension, by name. |

!!! note "The cube code is `CbCube_Int`, not `DimCube_Int`"
    There is no `DimCube_*` code. Cube references use the `CbCube_` prefix; everything with a `Dim`/`Dm` prefix refers to dimensions and their members.

### Dimension members

This is the heart of cube intelligence. A dimension member column lets a relational row **name its own cube intersection** — its entity, account, scenario, flow and user-defined members — so moving data between the table and the cube becomes a matter of reading the row rather than translating it. Without these columns, every relational-to-cube movement needs hand-written mapping code; with them, the intersection travels with the data.

Each dimension offers the same three variants, and choosing between them is the decision that matters:

- **`_Int`** — stores the member's internal key as a foreign key to the synced dimension member list. Validated, and presented to users as a member dropdown. This is the form to store.
- **`_NVarchar`** — stores the member name as text, which is what OneStream expects in a data buffer or cube view. This is the form to move data with.
- **`Base_Int`** — as `_Int`, but the selectable list is restricted to **base members**. Use this for anything that will actually post data to the cube, since only base members are writable — it makes an invalid intersection impossible to enter rather than something to validate later.

| Dimension | By id | By name | Base members only |
|---|---|---|---|
| Scenario | `DmMemberScenario_Int` | `DmMemberScenario_NVarchar` | `DmMemberScenarioBase_Int` |
| Entity | `DmMemberEntity_Int` | `DmMemberEntity_NVarchar` | `DmMemberEntityBase_Int` |
| Account | `DmMemberAccount_Int` | `DmMemberAccount_NVarchar` | `DmMemberAccountBase_Int` |
| Flow | `DmMemberFlow_Int` | `DmMemberFlow_NVarchar` | `DmMemberFlowBase_Int` |
| Intercompany | `DmMemberIC_Int` | `DmMemberIC_NVarchar` | `DmMemberICBase_Int` |
| UD (generic) | `DmMemberUd_Int` | `DmMemberUD_NVarchar` | `DmMemberUdBase_Int` |
| UD1 | `DmMemberUd1_Int` | `DmMemberUD1_NVarchar` | `DmMemberUd1Base_Int` |
| UD2 | `DmMemberUd2_Int` | `DmMemberUD2_NVarchar` | `DmMemberUd2Base_Int` |
| UD3 | `DmMemberUd3_Int` | `DmMemberUD3_NVarchar` | `DmMemberUd3Base_Int` |
| UD4 | `DmMemberUd4_Int` | `DmMemberUD4_NVarchar` | `DmMemberUd4Base_Int` |
| UD5 | `DmMemberUd5_Int` | `DmMemberUD5_NVarchar` | `DmMemberUd5Base_Int` |
| UD6 | `DmMemberUd6_Int` | `DmMemberUD6_NVarchar` | `DmMemberUd6Base_Int` |
| UD7 | `DmMemberUd7_Int` | `DmMemberUD7_NVarchar` | `DmMemberUd7Base_Int` |
| UD8 | `DmMemberUd8_Int` | `DmMemberUD8_NVarchar` | `DmMemberUd8Base_Int` |
| UD (data source) | `DmMemberUdDataSource_Int` | `DmMemberUD_DataSource_NVarchar` | `DmMemberUdDataSourceBase_Int` |

Use the generic UD codes when a column holds a member of whichever UD dimension the context supplies; use the numbered codes when the column is always UD1, always UD2, and so on.

!!! warning "Integration codes are case-sensitive, and UD casing is inconsistent"
    The id and base variants spell it `Ud` (`DmMemberUd1_Int`, `DmMemberUd1Base_Int`) while the name variants spell it `UD` (`DmMemberUD1_NVarchar`). The data-source name variant also carries an extra underscore: `DmMemberUD_DataSource_NVarchar`, not `DmMemberUdDataSource_NVarchar`. Copy these codes rather than typing them — an unrecognised code fails the import.

!!! note "Dimension member columns are hidden on views by default"
    As with workflow keys, a view column built on a dimension member standard column inherits `IsVisible: false`. The member drives the cube intersection rather than being shown in the grid.

See [Cubes](../../cube/cubes/index.md) and [Dimension Members](../../cube/dimensions/index.md) for the synced lists these resolve against.

### Time period and year

The other half of a cube intersection is **when**. These columns carry the period and year a row belongs to, in both the internal and the OneStream-facing form, and hold the amounts that move to and from the cube.

The `_Int` and `Name_NVarchar` codes are **designed to be used as a pair**: store both on the row. The integer is the raw internal key — cheap to index, correct to sort and join on — and the string is the same period rendered in OneStream's own format, which is what a data buffer or cube view expects. Keeping both means neither side of the movement has to convert anything.

| Code | Nullable | Description |
|---|---|---|
| `TimePeriod_Int` | Yes | The row's period as a raw internal integer key. Pair with `TimePeriodName_NVarchar`. |
| `TimePeriodName_NVarchar` | Yes | The row's period rendered as a OneStream-formatted period string. Max length 50. Pair with `TimePeriod_Int`. |
| `TimeYear_Int` | Yes | The row's year as a raw internal integer key. Pair with `TimeYearName_NVarchar`. |
| `TimeYearName_NVarchar` | Yes | The row's year rendered as a OneStream-formatted year string. Max length 50. Pair with `TimeYear_Int`. |
| `TimePeriodPeriodicAmount_Decimal` | Yes | The row's own single-period amount, as opposed to a cumulative figure — the P01–P12 style periodic value. `decimal(19,4)`, formatted `N2`. How the system knows *which* period the amount is for depends on the table's layout — see [Tall and wide period tables](#tall-and-wide-period-tables). |
| `TimePeriodYtdAmount_Decimal` | Yes | The row's year-to-date amount through its period. `decimal(19,4)`, formatted `N2`. |

#### Tall and wide period tables

A periodic amount is meaningless until the system knows which period it belongs to, and there are two layouts that answer that question differently. Both rely on the model column's `PeriodFlags` and `PeriodValue` — it is that pair, not the column's name, that makes a decimal into a period column.

`PeriodFlags` always matches the standard column the table column was built from: `1` (Standard, a periodic amount) for `TimePeriodPeriodicAmount_Decimal`, `4` (YTD) for `TimePeriodYtdAmount_Decimal`, with `2` for EOY. `PeriodValue` is the part that depends on the layout.

**Wide — a column per period.** Twelve amount columns on a single row, named `P01` through `P12` or similar, with no `TimePeriod_Int`. Nothing but `PeriodValue` distinguishes one period column from the next, so **every period column must carry it**: `P01` → `1`, through `P12` → `12`.

Working in the application you rarely set this by hand. When a table column is assigned a periodic-amount standard column and `PeriodValue` is still unset, the system derives it from the model column's name — the first run of digits that resolves to a number between 1 and 1000. `P01` gives 1, `Period_07` gives 7. A name with no usable digits leaves the value unset, and a name carrying an unrelated number can pick up the wrong one, so it is worth confirming the derived value on a column whose name was not written with this in mind.

!!! note "Derivation does not happen on import"
    The name-based derivation runs when the standard column is assigned through the application. An import file is taken as written: **set `PeriodValue` explicitly on every period column in a wide table**, or the columns arrive indistinguishable from one another. The derivation also applies only to the periodic-amount base column, not to YTD.

**Tall — a column per row.** A single amount column, with each record representing one period. Here the row itself says which period it is, which is what the `TimePeriod_Int` pair is for: **a tall table must carry `TimePeriod_Int`** (normally alongside `TimePeriodName_NVarchar`) so the amount has a period to belong to. There is nothing for `PeriodValue` to tell apart, so leave it unset.

Choosing between them is a data-shape decision rather than a preference. Wide suits a fixed twelve-period grid that users edit across a row; tall suits a variable or open-ended set of periods, and is the shape that moves most directly to and from the cube, since each row is already a single intersection.

Holding the periodic and year-to-date amounts as separate columns is deliberate: a cube stores one and derives the other, and which one a solution is authoritative for differs by use case. Storing both explicitly means a movement in either direction knows which figure it is carrying.

!!! note "Renamed in PV912-SV200"
    These six codes were renamed; the underlying standard columns are unchanged, so existing data and definitions are unaffected, but **import files referencing the old codes must be updated**.

    | Old code | New code |
    |---|---|
    | `DimGeneralPeriod_Decimal` | `TimePeriodPeriodicAmount_Decimal` |
    | `DimYtdPeriod_Decimal` | `TimePeriodYtdAmount_Decimal` |
    | `TimePeriodRaw_Int` | `TimePeriod_Int` |
    | `TimePeriodOs_NVarchar` | `TimePeriodName_NVarchar` |
    | `TimeYearRaw_Int` | `TimeYear_Int` |
    | `TimeYearOs_NVarchar` | `TimeYearName_NVarchar` |

### General-purpose

These carry no domain meaning — they are plain typed columns for business data that no other standard column describes. Reach for them only after checking that a meaningful code does not already exist.

| Code | Nullable | Description |
|---|---|---|
| `General_NVarchar` | Yes | Free-form unicode text. Pair with `MaxLength`. |
| `General_Varchar` | Yes | Free-form non-unicode text. |
| `General_Char` | Yes | Fixed-length non-unicode text. |
| `General_NChar` | Yes | Fixed-length unicode text. |
| `General_Int` | Yes | Plain integer business column — quantities, levels, ordinals. Formatted `N0`. |
| `General_BigInt` | Yes | Plain big-integer business column. Formatted `N0`. |
| `General_SmallInt` | Yes | Plain small-integer business column. Formatted `N0`. |
| `General_TinyInt` | Yes | Plain tiny-integer business column, 0–255. Formatted `N0`. |
| `General_Decimal` | Yes | Plain decimal business column. Pair with `Precision` and `Scale` — 18 and 2 for currency. Formatted `N2`. |
| `General_Numeric` | Yes | Alternate exact-numeric type, functionally similar to `General_Decimal`. |
| `General_Money` | Yes | SQL `money` type for currency amounts. |
| `General_SmallMoney` | Yes | SQL `smallmoney` type for smaller currency amounts. |
| `General_PercentDecimal` | Yes | Decimal column intended to hold a percentage. |
| `General_Float` | Yes | Floating-point business column. |
| `General_Real` | Yes | Single-precision floating-point business column. |
| `General_Bit` | No | Plain boolean flag that is not `IsActive` or `IsEnabled` — a custom yes/no field. |
| `General_Date` | Yes | Plain date-only column. Defaults to the current date and formats as `MM-dd-yyyy`. |
| `General_DateTime` | Yes | Plain datetime column, legacy precision. Defaults to the current date and time. |
| `General_DateTime2` | Yes | Plain datetime column — preferred over `General_DateTime` for its higher precision. |
| `General_SmallDateTime` | Yes | Plain low-precision datetime column. |
| `General_UniqueIdentifier` | Yes | Plain GUID business column, not a key or foreign key. |
| `General_Binary` | Yes | Fixed-length binary data. |
| `General_Varbinary` | Yes | Variable-length binary data. |

## What a column inherits, and when to override it

Inheritance runs in a chain: a **table column** takes its physical shape from its standard column; a **model column** takes its behaviour from the same standard column; and a **view column** takes `IsVisible`, `AllowUpdates` and `SecurityLevel` from the model column, and `IsDisplayLabel`, `FilterFlags`, `FilterExpression` and the summary settings from the standard column directly.

An attribute you leave unset is therefore **seeded, not left blank** — which is why audit columns need no configuration to populate themselves, and why a `Name_NVarchar` column is searchable without being told to be. An explicit value always wins over an inherited one.

Two exceptions override whatever you set: view columns built on **workflow key** and **dimension member** standard columns are forced to `IsVisible: false`.

!!! note "Inheritance is a safety net, not a reason to omit a value that matters"
    Working **in the application**, leave a setting unset when you want the standard column's behaviour — that is what the standard column is for, and overriding it by hand is how columns of the same kind drift apart.

    Writing an **import file**, prefer to set a value explicitly when it matters for that column, even where it matches what would be inherited. A structure file is reviewed, diffed and audited as a document, and a reader should be able to see that a column is read-only or hidden without cross-referencing the standard column it came from. Genuinely omit an attribute only where its value does not matter for that column.

## Typical Use Cases

- Checking which standard column to use for a new table column
- Confirming what nullability or default a column will inherit
- Understanding why a column is being treated as a foreign key
- Reviewing the data type and length a standard column applies
- Finding the right dimension member or time period code when wiring a table to the cube

## Notes

- Standard columns are shared across every solution in the application; treat them as read-only unless there is a clear reason to change one.
- If no standard column fits a requirement, review the list again before adding one — the intended column often exists under a different name.
- Integration codes are case-sensitive and are validated on import; an unrecognised code fails the file rather than falling back to a default.
- See [Table Columns](../relationalTables/columns.md) for how a standard column is applied when defining a column, and [Structure Definitions](../../../../extending/import/structure.md) for how one is referenced in an import file.
