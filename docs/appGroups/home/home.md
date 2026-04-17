---
title: Models
---

# Models

Models are the **foundation of all relational data structures** in Revfore Accelerate.

They define how your business data is stored, organized, and managed.

---

## 🧠 What is a Relationship?

A Relationship represents a **business entity** and contains a set of records (rows) and attributes (columns).

Examples of common Models:

- Employees
- Assets
- Products
- Services
- Locations

Each Relationship is designed to capture data at its **natural level of detail**.

---

## 🧱 Relationship Structure

A Relationship consists of:

### Columns (Fields)
Each column defines a specific attribute of the data.

Example (Employee Relationship):

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

## ⚙️ How Models Work in Revfore Accelerate

Models in Revfore Accelerate are:

- **Defined through metadata** (no SQL required)
- **EdiRelationship through a common UI**
- **Reusable across workflows and dashboards**
- **Integrated with Models and views**

Once a Relationship is defined, it becomes part of a broader relational model.

---

## 🔗 Models and Models

Models rarely exist in isolation.

They are typically connected to other Models using Models.

Example:

- Employee → Department  
- Asset → Location  
- Product → Service Line  

These Models allow you to build more powerful and flexible data models.

---

## 🧭 Designing Models (Best Practices)

### Think in Business Terms
Define Models based on real-world entities, not technical structures.

✔️ Good:
- Employee
- Asset
- Product

❌ Avoid:
- Generic or unclear Models like "Data1" or "Misc"

---

### Keep Models Focused
Each Relationship should represent **one primary concept**.

Avoid combining unrelated data into a single Relationship.

---

### Use Clear Naming
Use names that are:
- Easy to understand
- Consistent across your model

---

### Plan for Reuse
Models should be designed so they can be reused across:
- Planning models
- Dashboards
- Workflows

---

## 🚀 Example Use Case

Let’s say you want to plan labor costs.

You might create:

**Employee Relationship**
- Employee Name
- Department
- Role
- Base Salary

Then:
- Link to a Department Relationship
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
- Learn about [Relational Tables](tables.md)
- Explore [Relational Views](views.md)