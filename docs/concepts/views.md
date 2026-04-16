---
title: Views
---

# Views

Views are the **foundation of all relational data structures** in Revfore Accelerate.

They define how your business data is stored, organized, and managed.

---

## 🧠 What is a View?

A View represents a **business entity** and contains a set of records (rows) and attributes (columns).

Examples of common Views:

- Customers

Each View is designed to capture data at its **natural level of detail**.

---

## 🧱 View Structure

A View consists of:

### Columns (Fields)
Each column defines a specific attribute of the data.

Example (Employee View):

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

## ⚙️ How Views Work in Revfore Accelerate

Views in Revfore Accelerate are:

- **Defined through metadata** (no SQL required)
- **EdiView through a common UI**
- **Reusable across workflows and dashboards**
- **Integrated with relationships and views**

Once a View is defined, it becomes part of a broader relational model.

---

## 🔗 Views and Relationships

Views rarely exist in isolation.

They are typically connected to other Views using relationships.

Example:

- Employee → Department  
- Asset → Location  
- Product → Service Line  

These relationships allow you to build more powerful and flexible data models.

---

## 🧭 Designing Views (Best Practices)

### Think in Business Terms
Define Views based on real-world entities, not technical structures.

✔️ Good:
- Employee
- Asset
- Product

❌ Avoid:
- Generic or unclear Views like "Data1" or "Misc"

---

### Keep Views Focused
Each View should represent **one primary concept**.

Avoid combining unrelated data into a single View.

---

### Use Clear Naming
Use names that are:
- Easy to understand
- Consistent across your model

---

### Plan for Reuse
Views should be designed so they can be reused across:
- Planning models
- Dashboards
- Workflows

---

## 🚀 Example Use Case

Let’s say you want to plan labor costs.

You might create:

**Employee View**
- Employee Name
- Department
- Role
- Base Salary

Then:
- Link to a Department View
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

- Learn about [Relationships](relationships.md)
- Explore [Views](views.md)