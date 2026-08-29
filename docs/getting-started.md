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

Solutions are designed and built with AI assistance. Two tools are involved, and only the second is required.

### Custom GPT — optional

The **Revfore Framework Assistant** is a custom GPT for the ideation stage. It is quicker to think out loud in, and good for shaping a solution before any files exist.

[Revfore Framework Assistant (V200)](https://chatgpt.com/g/g-6a763d1d09688191ac7d1b3988aefabd-revfore-framework-assistant-v200)

!!! note "Access required"
    The custom GPT is not publicly available. Request access from Revfore before using the link above.

You can skip it entirely — Claude can take requirements directly and do the ideation itself. Use the GPT when you want to explore a problem before committing to a design; go straight to Claude when the requirements are already clear.

### Claude — required

Claude does the engineering and build: it turns a design, or requirements directly, into the import JSON and the C# a solution is made of.

Revfore publishes a **Claude skill** for this. The skill gives Claude the Framework's schema, conventions, standard columns, core table structures, and worked examples, so what it generates follows the same rules a hand-built solution is held to. Install the skill before you start — without it, Claude has no knowledge of the Framework.

See [AI Model Integrations](integrations/aiModels/index.md) for how the two stages fit together.

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

2. **Shape the solution in the custom GPT** *(optional)*
   - Produces a written design to take forward. Skip this and go straight to Claude if the requirements are already clear

3. **Use Claude to review the requirements or design, then create the JSON schema, JSON data, and code assembly files**

4. **Import the JSON schema files**
   - This will create all your table, model and view definitions

5. **Review and sync the table and view definitions**
   - This will create the database tables and views

6. **Import the JSON data files for supporting tables**
   - Now that the database tables and views are created, data can be imported into them

7. **Add the code assembly files**
   - This will put in place all the logic that needs to run when data is entered, submitted, and so on

8. **Add new Genesis navigation groups and pages**

9. **Link Revfore to the Genesis pages and configure the presentation**

10. **Start using the solution**

---

## 🏁 Your First Use Case

A common starting point:

> Open Claude and start prompting to help you build a simple solution that handles a particular use case

Example:

> "I need to capture rate changes by product and effective date, with an approver on each change. "
> "Design the tables, models and views for that and generate the import JSON."

---

## 💡 Tips

- Start simple — one table at a time  
- Think in terms of business entities (not technical structures)  
- Reuse views whenever possible  
- Keep naming consistent  

---

## 👉 Next Steps

- Learn about [Relational Tables](concepts/tables.md)
- Understand [Relational Models](concepts/models.md)
- Explore [Relational Views](concepts/views.md)