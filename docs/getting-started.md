---
title: Getting Started
---

# Getting Started with Revfore Framework

Welcome to Revfore Framework.

This guide will help you understand the core concepts and get up and running with building relational data solutions inside OneStream.

---

## 🚀 What is Revfore Framework?

Revfore Framework is a **low-code relational framework** that allows you to:

- Create relational tables along with their relationships to other tables
- Create relational models that source data from any number of relational tables
- Build reusable relational views from the relational models along with allowed user actions
- Surface data in editable grids and dashboards

All of it defined as metadata rather than code — which is what makes it fast to build, cheap to maintain, and secure by default.

### Built for AI assistance

Because a solution is **defined as metadata rather than hand-written SQL and screens**, it can be described in files — and files are something an AI model can write, read back, and check.

That is the difference between AI that guesses at a solution and AI that builds one. Claude works from the Framework's own schema and conventions, produces the import JSON and code a solution is actually made of, and validates them before anything reaches the database. You review files, not screenshots of screens someone else has already clicked together.

The practical effect is that the slow part of a project — standing up the tables, models, views and the code behind them — stops being the slow part.

### Less custom architecture to maintain

Everything a solution needs is defined inside the Framework: the tables, the relationships, the views, the user actions, the security. There is no bespoke schema to document, no hand-rolled screens to keep in step with it, and no one-off SQL scattered across dashboards.

That matters most after go-live. A solution built this way is one another person can pick up and understand, because it looks like every other solution built the same way — and an upgrade moves the whole estate forward rather than breaking somebody's custom work.

### Stronger security by default

Security is part of the model rather than something layered on afterwards.

Users and security groups come from OneStream, and the group hierarchy and user assignments are synced and flattened, so **row-level user security is a join rather than a piece of custom code**. Access is defined once, on the view, and applies wherever that view is used.

Custom-built alternatives tend to reimplement this per solution, which is where inconsistencies and gaps get in.

---

## ⚙️ Setup

Before getting started, make sure your environment is ready:

👉 Follow the [Setup And Installation](setup/setup.md) to install and configure Revfore Framework.

---

## 🤖 AI Tooling

Solutions are designed and built with AI assistance, using **Claude**.

Revfore ships an **Extension Kit** for this: a VS Code folder where you work with Claude Code on your own machine. It brings together:

- the **Claude skills**, already installed in the folder, which give Claude the Framework's schema, conventions, standard columns, core table structures and worked examples, so what it generates follows the same rules a hand-built solution is held to
- a **design area** (`solutions/`) where each solution's design documents, workbooks and import JSON are kept, so you can version, diff and review them
- a **buildable copy of the extension assembly**, so the handler code Claude writes compiles against the Revfore and OneStream APIs before you deploy it

Open the kit folder and start Claude Code, and it loads the skills on its own. There is nothing to upload or configure. From there Claude takes a solution from a requirements conversation through to the files you import and the extension code behind them.

### Getting the Extension Kit

Download **rfaExtensionKit.zip** from OneStream: **Application | Presentation | Workspaces**, select your Revfore Framework workspace (**Revfore Framework (RFA)**, or *name (code)* if you [created your own instance](appGroups/config/create-new-instance.md)), open the **XCP_xRfaDlg_ActnExtension** maintenance unit, and take the file from **Files**.

Extract it to a folder for the project, open that folder in VS Code, and start Claude Code. Take a new kit after each Framework upgrade, so Claude is working from the current schema. See [Extension Kit (VS Code)](extending/extension-kit.md) for the requirements, setup steps and the full design-to-deploy workflow.

!!! note "Using Claude Desktop or claude.ai instead"
    The same skills are available on their own as **rfaClaudeSkills.zip**, in the same maintenance unit, for uploading to Claude Desktop or claude.ai. That gives you the design conversation and the import files, but not the solution folder or the code build. See [Using the skills without the kit](integrations/aiModels/index.md#using-the-skills-without-the-kit).

### The design workbook

Claude does not go straight from a conversation to import files. It first produces a **design workbook** — an Excel file with a sheet for each part of the configuration: tables, columns, views, actions, lookups, reference data and behavior.

The workbook is where the design gets settled. Because it is an ordinary `.xlsx`, you can take it away and work through it with the people who own the process — email it round, sit down with them and go through the columns, gather changes over a week. **The design conversation does not have to happen in front of Claude.**

Hand the edited workbook back and Claude reports only what changed, then produces the next version. This normally takes several rounds.

Only once the workbook has settled, and only when you ask, does Claude generate the import JSON. The extension code comes after that.

See [AI Model Integrations](integrations/aiModels/index.md) for the full picture.

---

## 🧠 Core Concepts

Before you begin, it’s helpful to understand a few key ideas:

### AI-assisted design and file creation
Requirements are turned into the files a solution is built from — the JSON that defines its tables, models and views, the JSON that seeds its reference data, and the C# that implements its business logic. You review and adjust those files rather than writing them from scratch. See [AI Model Integrations](integrations/aiModels/index.md) for how this works.

### Database Tables & Views
The physical objects in the database, as opposed to the definitions below that describe them. A definition is what you edit; syncing it creates or updates the real table or view. Both are visible in the application, so the two can be compared when they drift apart.

### Relational Table Definitions
Structured datasets that store your business data and define how that data links together through relationships  
(e.g., Customers, Rates, Employees, Assets, Products, Requests)

### Relational Model Definitions
Unified structures that bring data from multiple tables into one place. Models allow you to define lookups, defaults, required fields, and additional expression-based columns to simplify data entry and enforce consistency

### Relational View Definitions
Reusable, queryable representations of your relational data. Views are also where you define what actions users can take, such as adding, editing, or managing data

### Metadata-Driven UI
Core structures are defined in metadata, enabling consistent, automatically generated interfaces that can be leveraged in OneStream Genesis, Partner Solutions, and custom solutions to deliver user-friendly applications and dashboards

---

## ⚙️ Basic Workflow

Here’s the typical process for building a solution. Steps 2 to 4 happen in the [Extension Kit](extending/extension-kit.md), working with Claude Code in VS Code; everything it produces is written to the solution's folder under `solutions/` in the kit.

1. **Set up the Extension Kit**
   - Download and extract the kit (one folder per project or customer), open it in VS Code and start Claude Code

2. **Work through the design with Claude, which produces a design workbook**
   - Give Claude the requirements: what the solution needs to plan or track, who works on it, how it is approved, and whether it posts to the cube. Include any existing configuration you know of, such as workflow units, cubes and dimensions
   - An Excel file covering the tables, columns, views, actions, lookups and reference data, saved in the solution folder

3. **Review and edit the workbook, with your stakeholders**
   - Save the edited workbook back into the solution folder and ask Claude to read it. It reports what changed and produces the next version. Expect several rounds

4. **When the design has settled, ask Claude for the JSON schema, JSON data, and code assembly files**
   - Claude validates the JSON and builds the extension code in the kit, so schema and compile errors are caught before anything reaches OneStream
   - It then builds the solution's [setup package](extending/setup-packages.md), **rfaExtensionSetupPackage.zip**, in the solution folder

5. **Load the rfaExtensionSetupPackage.zip file**
   - Use **Load** on **Admin | Relational | Tables** and select the zip. In one run it imports the table, model and view definitions, syncs them to create the database tables and views, loads the data the solution needs, and loads the page settings

6. **Deploy the code assembly files from the kit**
   - Copy the handlers you changed into the **rfa_actnExtension_os** assembly, or push them with OneStream Developer Studio. This puts in place all the logic that needs to run when data is entered, submitted, and so on

7. **Add the navigation groups and pages documented in the design**
   - Claude's page list gives each page's application group, **Navigation Group** and **Page** name. Create them in OneStream with those names, typed exactly. See [Create the page in OneStream](extending/pages.md#step-2-create-the-page-in-onestream)

8. **Link each page to the Revfore dashboard documented in the design**
   - Select the page and click **Link an Existing Content Item from a Shared Workspace**, choose the Revfore Framework workspace, and pick the **Linked Revfore dashboard** (the content item layout) the page list gives for it. The page then shows the grids and content from its page settings

9. **Start using the solution**

---

## 🏁 Your First Use Case

A common starting point:

> Open Claude and start prompting to help you build a simple solution that handles a particular use case

Example:

> "I need to capture rate changes by product and effective date, with an approver on each change. "
> "Design the tables, models and views for that and give me a design workbook."

Work through the workbook, hand it back with your edits, and ask for the import JSON once it looks right.

---

## 💡 Tips

- Start simple — one table at a time  
- Think in terms of business entities (not technical structures)  
- Reuse views whenever possible  
- Keep naming consistent  
- Settle the workbook before asking for JSON — a change is a cell edit at that stage, and a rebuild after it  
- Put a `?` in the workbook where you are unsure; it comes back as a question rather than a guess  

---

## 👉 Next Steps

- Learn about [Relational Tables](concepts/tables.md)
- Understand [Relational Models](concepts/models.md)
- Explore [Relational Views](concepts/views.md)