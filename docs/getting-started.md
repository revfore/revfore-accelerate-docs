---
title: Getting Started
---

# Getting Started with Revfore Accelerate

Welcome to Revfore Accelerate.

This guide will help you understand the core concepts and get up and running with building relational data solutions inside OneStream.

---

## 🚀 What is Revfore Accelerate?

Revfore Accelerate is a **no-code relational framework** that allows you to:

- Create relational tables along with their relationships to other tables
- Create relational models that source data from any number of relational tables
- Build reusable relational views from the relational models along with allowed user actions
- Surface data in editable grids and dashboards

All without writing SQL.

---

## ⚙️ Setup

Before getting started, make sure your environment is ready:

👉 Follow the [Setup And Installation](setup/setup.md) to install and configure Revfore Accelerate.

---

## 🧠 Core Concepts

Before you begin, it’s helpful to understand a few key ideas:

### Relational Tables
Structured datasets that store your business data and define how that data links together through relationships  
(e.g., Customers, Rates, Employees, Assets, Products, Requests)

### Relational Models
Unified structures that bring data from multiple tables into one place. Models allow you to define lookups, defaults, required fields, and additional expression-based columns to simplify data entry and enforce consistency

### Relational Views
Reusable, queryable representations of your relational data. Views are also where you define what actions users can take, such as adding, editing, or managing data

### Metadata-Driven UI
Core structures are defined in metadata, enabling consistent, automatically generated interfaces that can be leveraged in OneStream Genesis, Partner Solutions, and custom solutions to deliver user-friendly applications and dashboards

---

## ⚙️ Basic Workflow

Here’s the typical process for building a solution:

1. **Create tables and relationships**
   - Define your data structures and how they link together

2. **Build relational models**
   - Unify data across tables with lookups, defaults, required fields, and calculated columns

3. **Build views**
   - Create reusable, queryable data outputs, define filters, and control available user actions

4. **Expose in UI**
   - Use dashboards and grids for interaction

---

## 🏁 Your First Use Case

A common starting point:

> Build a rate table for products or services

Example:
- Create a `Product` or `Service` table
- Create a `Rate` table to store pricing or cost details
- Define rate-related attributes (e.g., unit price, cost, effective dates)
- Link to other tables (e.g., customers, regions, or units of measure)
- Build relational models unify data and to apply lookups, defaults, and required fields
- Create views with filters and user actions for managing and maintaining the data

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