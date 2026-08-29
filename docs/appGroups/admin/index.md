# Admin

The **Admin** application group is used for **day-to-day administration** of relational data structures in Revfore Framework.

This is where users define, manage, and maintain the core components that power relational planning and operational workflows.

While the **Config** application group is focused on setup, upgrades, and system-level tasks, the **Admin** application group is where users actively build and manage their data structures.

---

## Admin Sections

The Admin application group is organised into four sections:

**Relational** – the relational data model and the database behind it

- [Database](relational/database/index.md) – the tables and views that physically exist in the database
- [Relational Tables](relational/relationalTables/index.md) – table definitions, columns, indexes and relationships
- [Relational Models](relational/relationalModels/index.md) – models, sources, columns and relationships
- [Relational Views](relational/relationalViews/index.md) – views, security, columns, actions and filters
- [Supporting](relational/supporting/index.md) – standard columns, lookups, cube views and forms

**Workflow** – the workflow structures a process runs against

- [Units](workflow/units/index.md), [Instances](workflow/instances/index.md), [Areas](workflow/areas/index.md) and [Supporting](workflow/supporting/index.md)

**Cube** – the OneStream cubes and dimension members relational data maps to

- [Cubes](cube/cubes/index.md) and [Dimension Members](cube/dimensionMembers/index.md)

**Security** – who can see and edit data

- [Users](security/Users/index.md) and [Groups](security/Groups/index.md)

---

## Overview

Use the Admin application group to:

- define and manage relational data structures
- organize relationships between entities
- create reusable data views
- manage supporting lookup data
- configure forms for user interaction and data entry

---

## How the Relational Sections Work Together

The Relational components are designed to work together as part of a relational framework:

### Relational Tables
Define the core business entities and store detailed data.

### Relational Models
Connect tables together and define relationships between entities.

### Relational Views
Provide reusable, queryable outputs of relational data for reporting and application use.

### Lookups
Support standardized values and controlled lists used across tables and forms.

### Forms
Provide user-facing interfaces for entering and managing data.

---

## Typical Workflow

A common workflow when building a relational solution:

1. Create **Relational Tables** to define your data structure  
2. Build **Relational Models** to link those tables together  
3. Define **Relational Views** to expose and use the data  
4. Configure **Lookups** for controlled values  
5. Use **Forms** to allow users to interact with the data  

---

## When to Use Admin

Use the Admin application group when:

- building or modifying relational data structures
- configuring planning or operational models
- managing data definitions and relationships
- creating user-facing data entry or interaction experiences

---

## Related Application Groups

- Use **Config** for setup, upgrades, licensing, and system-level tasks  
- Use **Home** to expose relational views in navigation pages  
- Use **Help** for documentation and support resources  