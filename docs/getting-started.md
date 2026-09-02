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

Revfore publishes a **Claude skill** for this. The skill gives Claude the Framework's schema, conventions, standard columns, core table structures, and worked examples, so what it generates follows the same rules a hand-built solution is held to. Install the skill before you start — without it, Claude has no knowledge of the Framework.

With the skill loaded, Claude takes a solution from a requirements conversation through to the files you import.

### The design workbook

Claude does not go straight from a conversation to import files. It first produces a **design workbook** — an Excel file with a sheet for each part of the configuration: tables, columns, views, actions, lookups, reference data and behaviour.

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

Here’s the typical process for building a solution:

1. **Gather requirements**

2. **Work through the design with Claude, which produces a design workbook**
   - An Excel file covering the tables, columns, views, actions, lookups and reference data

3. **Review and edit the workbook, with your stakeholders**
   - Hand it back to Claude, which reports what changed and produces the next version. Expect several rounds

4. **When the design has settled, ask Claude for the JSON schema, JSON data, and code assembly files**

5. **Import the JSON schema files**
   - This will create all your table, model and view definitions

6. **Review and sync the table and view definitions**
   - This will create the database tables and views

7. **Import the JSON data files for supporting tables**
   - Now that the database tables and views are created, data can be imported into them

8. **Add the code assembly files**
   - This will put in place all the logic that needs to run when data is entered, submitted, and so on

9. **Add new Genesis navigation groups and pages**

10. **Link Revfore to the Genesis pages and configure the presentation**

11. **Start using the solution**

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