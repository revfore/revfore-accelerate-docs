# AI Model Integrations

Revfore Framework is designed so that a solution's structure can be **generated from plain-English requirements** rather than hand-built.

Because tables, models, and views are defined as metadata — importable JSON rather than hand-written SQL and screens — an AI model can produce a complete, reviewable solution definition, and that definition can be validated before anything reaches the database.

Two AI tools are used, at two different stages.

## The two stages

| Stage | Tool | Produces |
|---|---|---|
| **Ideation and design** | Revfore Framework custom GPT | A written solution design — the tables, models, views, lookups, actions and behaviour a solution needs, and why |
| **Engineering and build** | Claude | The import JSON and the C# that implement that design |

The design is the handoff. You work through the shape of a solution in the GPT, then take the finished design to Claude, which turns it into the files you actually import.

## Custom GPT — ideation and design

The Revfore Framework custom GPT is the **thinking-out-loud stage**. It is fast at exploring requirements, answering product questions, and shaping a solution before any files exist.

Use it to:

- turn a business requirement into a proposed set of tables, models and views
- decide which structures are needed and which are not
- check what is possible before committing to a design
- surface the design decisions that are expensive to change later

It knows the Framework's schema, conventions, standard columns, and existing platform structures, so what it proposes is buildable rather than merely plausible. It does not write the import JSON or the C# — that is the next stage.

## Claude — engineering and build

Claude takes the finished design and produces the files a solution is actually made of:

- the import JSON that defines the tables, models, views and lookups
- the import JSON that seeds reference data
- the C# assembly code that implements the solution's business logic

Claude validates what it generates against the Framework's schema and conventions before you import it, which catches the class of mistakes that produce valid-looking JSON that is still wrong.

## Capabilities

- Generate a complete solution design from plain-English requirements
- Generate importable Tables, Models, Views, Lookups and Data JSON from that design
- Generate the C# customization code that implements a solution's business logic
- Validate generated JSON against the current schema before import
- Review and explain an existing solution's definitions
- Answer product, setup, and configuration questions

## Benefits

- **Move from requirement to working solution in a fraction of the time**, without hand-writing SQL, screens, or import files
- **Design decisions surface early**, while they are still cheap to change
- **Output follows the same conventions every time**, so solutions built by different people look and behave alike
- **Everything is reviewable** — the design, the JSON, and the code are all files you can read and change before anything is applied
- **The framework does the enforcing**, so a generated solution is held to the same rules as a hand-built one

## Typical Use Cases

- Standing up a new solution from a business requirement
- Adding a table, column, or view to an existing solution
- Seeding reference data for a new solution
- Writing the handler code behind a custom action
- Reviewing an existing design for convention and schema issues
- Exploring whether a requirement is achievable before committing to it

## Notes

- The two stages are deliberately separate: the GPT is quick and exploratory, Claude is precise and produces the deliverables.
- Generated output is always reviewed before import — nothing reaches the database without a person applying it.
- The design produced in the first stage is what makes the second stage reliable; a vague design produces vague files.
- See [Getting Started](../../getting-started.md) for where these stages sit in the overall build process.
