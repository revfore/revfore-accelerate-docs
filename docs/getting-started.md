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

All without writing SQL.

---

## ⚙️ Setup

Before getting started, make sure your environment is ready:

👉 Follow the [Setup And Installation](setup/setup.md) to install and configure Revfore Framework.

---

## 🧠 Core Concepts

Before you begin, it’s helpful to understand a few key ideas:

### Claude-assisted design and file creation
Requirements are turned into the files a solution is built from — the JSON that defines its tables, models and views, the JSON that seeds its reference data, and the C# that implements its business logic. You review and adjust those files rather than writing them from scratch.

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

2. **Use Claude to review the requirements, then design and create the JSON schema, JSON data, and code assembly files**

3. **Import the JSON schema files**
   - This will create all your table, model and view definitions

4. **Review and sync the table and view definitions**
   - This will create the database tables and views

5. **Import the JSON data files for supporting tables**
   - Now that the database tables and views are created, data can be imported into them

6. **Add the code assembly files**
   - This will put in place all the logic that needs to run when data is entered, submitted, and so on

7. **Add new Genesis navigation groups and pages**

8. **Link Revfore to the Genesis pages and configure the presentation**

9. **Start using the solution**

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