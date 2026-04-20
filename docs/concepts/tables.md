---
title: Tables
---

# Tables

Tables are the **foundation of all relational data structures** in Revfore Accelerate.

They define how your business data is stored, organized, and managed.

---

## 🧠 What is a Table?

A table represents a **business entity** and contains a set of records (rows) and attributes (columns).

Examples of common tables:

- Employees
- Assets
- Products
- Services
- Locations

Each table is designed to capture data at its **natural level of detail**.

---

## 🧱 Table Structure

A table consists of:

### Columns (Fields)
Each column defines a specific attribute of the data.

Example (Employee table):

- Employee Name  
- Department  
- Role  
- Salary  

---

### Rows (Records)
Each row represents an individual instance of that entity.

Example:

| Employee Name | Department | Role        | Salary |
|--------------|------------|------------|--------|
| John Smith   | Finance    | Analyst     | 75,000 |
| Jane Doe     | HR         | Manager     | 95,000 |

---

## ⚙️ How Tables Work in Revfore Accelerate

Tables in Revfore Accelerate are:

- **Defined through metadata** (no SQL required)
- **Editable through a common UI**
- **Reusable across workflows and dashboards**
- **Integrated with relationships and views**

Once a table is defined, it becomes part of a broader relational model.

---

## 🔗 Tables and Relationships

Tables rarely exist in isolation.

They are typically connected to other tables using relationships.

Example:

- Employee → Department  
- Asset → Location  
- Product → Service Line  

These relationships allow you to build more powerful and flexible data models.

---

## 🧭 Designing Tables (Best Practices)

### Think in Business Terms
Define tables based on real-world entities, not technical structures.

✔️ Good:
- Employee
- Asset
- Product

❌ Avoid:
- Generic or unclear tables like "Data1" or "Misc"

---

### Keep Tables Focused
Each table should represent **one primary concept**.

Avoid combining unrelated data into a single table.

---

### Use Clear Naming
Use names that are:
- Easy to understand
- Consistent across your model

---

### Plan for Reuse
Tables should be designed so they can be reused across:
- Planning models
- Dashboards
- Workflows

---

## 🚀 Example Use Case

Let’s say you want to plan labor costs.

You might create:

**Employee Table**
- Employee Name
- Department
- Role
- Base Salary

Then:
- Link to a Department table
- Use the data in planning workflows
- Build views for reporting

---

## 💡 Tips

- Start with your core entities first  
- Add additional attributes over time  
- Avoid overcomplicating your initial design  
- Keep your model flexible  

---

## 👉 Next Steps

- Learn about [Relational Tables](../appGroups/admin/relationalTables/index.md)
- Learn about [Relational Models](models.md)
- Explore [Relational Views](views.md)