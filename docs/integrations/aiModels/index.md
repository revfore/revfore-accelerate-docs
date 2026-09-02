# AI Model Integrations

Revfore Framework is designed so that a solution's structure can be **generated from plain-English requirements** rather than hand-built.

Because tables, models, and views are defined as metadata — importable JSON rather than hand-written SQL and screens — an AI model can produce a complete, reviewable solution definition, and that definition can be validated before anything reaches the database.

Revfore provides a **Claude skills file** that teaches Claude the Framework: its schema, conventions, standard columns, extension points, and the shape of every file it produces. With that loaded, Claude takes a solution from a conversation through to the files you import.

## How a solution gets built

| Stage | What comes out of it |
|---|---|
| **Requirements conversation** | An understanding of what the solution needs to do, and a written design |
| **Design workbook** | An Excel workbook you and your stakeholders edit directly — the configuration in a form anyone can review |
| **Import JSON** | The tables, models, views, lookups and reference data, generated once the workbook has settled |
| **Extension code** | The C# that implements the solution's business logic |

Each stage is a deliverable you can read and change. Nothing reaches the database until you import it.

## The design workbook

The middle stage is the one that changes how a solution gets designed.

Rather than going straight from a conversation to import files, Claude produces an **Excel design workbook** — one sheet per part of the configuration:

| Sheet | Holds |
|---|---|
| **Tables** | One row per table: names, kind, parent, business key |
| **Columns** | The heart of it — every column, its type, whether it is visible and editable, how it is populated |
| **Views** | The screens and lists the solution needs |
| **ViewColumns** | Per-view overrides of the column defaults |
| **Actions** | The buttons on each view |
| **Lookups** | The dropdowns, and where their values come from |
| **ReferenceData** | Seed rows the solution needs to function |
| **Behaviour** | The rules that need code behind them |

### Why it exists

A design written as prose is hard to review and harder to argue with. A workbook is a **configuration tool**: the person who knows the business can open it, change a display name, mark a column hidden, add a row, and hand it back — without knowing anything about the Framework's schema.

The sheets constrain what can be entered, so edits stay valid: dropdowns for standard columns, lookups, action names, and how each column is populated. Where something is genuinely unknown, a `?` records it as an open question rather than a guess.

### Editing it offline, with stakeholders

The workbook is an ordinary `.xlsx` file. Once Claude has produced it you can take it away entirely — email it round, sit down with the finance team and work through the Columns sheet on a screen, or collect changes from several people over a week.

That is the point. **The design conversation does not have to happen in front of Claude.** It happens where the knowledge is, in a tool everyone already has, on your own timescale.

### Iterating

When the workbook comes back, Claude reads it and reports **only what changed** — a short list of edited rows, not the whole file. You confirm or discuss those changes, and it produces the next version.

This normally takes several rounds, and that is expected. The workbook stage is where a design gets argued out.

!!! note "Import JSON comes last"
    Claude does not generate the import JSON until you ask for it. Generating early wastes work — the JSON goes stale the moment another cell changes. Ask for it once the workbook has settled; the extension code comes later still.

## What Claude produces

- the **design workbook**, and each revised version as the design evolves
- the **import JSON** that defines tables, models, views and lookups
- the **import JSON** that seeds reference data
- the **C# extension code** that implements the solution's business logic

Claude validates what it generates against the Framework's schema and conventions before you import it, which catches the class of mistakes that produce valid-looking JSON that is still wrong.

## Capabilities

- Turn plain-English requirements into a proposed solution design
- Produce a design workbook for stakeholders to review and edit directly
- Read an edited workbook back and report only what changed
- Generate importable Tables, Models, Views, Lookups and Data JSON once the design has settled
- Generate the C# customization code behind a custom action or business rule
- Validate generated JSON against the current schema before import
- Review and explain an existing solution's definitions
- Answer product, setup, and configuration questions

## Benefits

- **Move from requirement to working solution in a fraction of the time**, without hand-writing SQL, screens, or import files
- **Non-technical stakeholders can review the actual configuration**, not a description of it
- **The design is settled before anything is built**, so changes happen in a spreadsheet cell rather than in deployed structures
- **Design decisions surface early**, while they are still cheap to change
- **Output follows the same conventions every time**, so solutions built by different people look and behave alike
- **Everything is reviewable** — the workbook, the JSON, and the code are all files you can read and change before anything is applied
- **The framework does the enforcing**, so a generated solution is held to the same rules as a hand-built one

## Typical Use Cases

- Standing up a new solution from a business requirement
- Working through a design with the people who own the process, in a workbook they can edit
- Adding a table, column, or view to an existing solution
- Seeding reference data for a new solution
- Writing the handler code behind a custom action
- Reviewing an existing design for convention and schema issues
- Exploring whether a requirement is achievable before committing to it

## Notes

- Each design session gets its own dated working folder, so an edited workbook is never confused with an earlier version.
- Generated output is always reviewed before import — nothing reaches the database without a person applying it.
- A vague design produces vague files. The workbook stage exists to make the design specific before it is built.
- See [Getting Started](../../getting-started.md) for where these stages sit in the overall build process, and [Extending the Framework](../../extending/index.md) for what the generated files contain.
