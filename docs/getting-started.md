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

###Claude assisted design and file creation

###Database Tables & Views
??

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

1  **Gather requirements

1. **Use Claude to review the requirements, design and create json schem, json data and code assembly files

2. **Import the json schema files
   - This will create all your table, model and view definitions

3. **Review and Sync table and view definitions
   - This will create the database tables and views

4. **Import json data files for supporting tables
   - Now that the database tables and view are created, data can be imported into them.

5. **Add code assembly files
   - This will put in place all the logic that needs to run when data is entered, submitted, etc.

6. **Add new Genesis navigation groups and pages

7. **Link Revfore to Genesis pages and configure the presentation

8. **Start using the solution

---

## 🏁 Your First Use Case

A common starting point:

> Open Claude and start prompting to help you build a simple solution that handles a particular use-case

Example:
???

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