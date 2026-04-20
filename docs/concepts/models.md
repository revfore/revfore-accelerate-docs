# Relational Models

Relational Models organize and connect relational data structures in Revfore Accelerate.

They define how data from one or more relational tables is brought together into a meaningful structure for data entry, reporting, analysis, and application use.

* * *

## What is a Relational Model?

A relational model represents a structured combination of one or more relational tables and the relationships between them.

It provides the layer between raw table definitions and user-facing outputs such as relational views.

Examples of common models:

- Employee Model
- Product Rates Model
- Transaction Model
- Customer and Transaction Model

Each model is designed around a specific business purpose or use case.

* * *

## Types of Relational Models

Relational Models can support different types of use cases.

### Data Entry Models

Some models are centered around a **single primary table** along with the foreign key tables it can join to.

These models are commonly used for:

- data entry
- data maintenance
- operational workflows
- reporting and analysis on a primary business entity

Examples include:

- an Employee table joined to Department and Location tables
- an Asset table joined to Asset Type and Location tables
- a Product table joined to Category and Service Line tables

These models are often used when users need to manage records directly while also seeing related descriptive data.

### Read-Only Analysis and Reporting Models

Other models are designed primarily for **analysis and reporting**.

These models are typically read-only and often join to a wider set of related tables.

They may represent structures such as:

- parent-child hierarchies
- master data and transaction data
- broad analytical datasets that combine many related entities

Examples include:

- a customer master table joined to transaction history
- a product hierarchy joined to sales or forecast detail
- a parent-child organizational structure joined to performance metrics

These models are generally optimized for reporting, analysis, and user consumption rather than direct data entry.

* * *

## Model Structure

A relational model consists of:

### Model Sources

Sources define which relational tables are included in the model.

Examples of sources:

- Employee Table
- Department Table
- Asset Table
- Location Table
- Transaction Table

* * *

### Model Columns

Columns define the fields that the model exposes for downstream use.

Examples of model columns:

- Employee Name
- Department Name
- Asset Type
- Location Code
- Transaction Amount

* * *

### Model Relationships

Model Relationships define which Table Relationships will be exposed for downstream use in Navigation pages

* * *

## ⚙️ How Relational Models Work in Revfore Accelerate

Relational Models in Revfore Accelerate are:

- defined through metadata
- built from one or more relational tables
- reusable across views, dashboards, and workflows
- structured to support business-focused use cases

Once a model is defined, it can be used as the foundation for one or more relational views.

* * *

## Models and Views

Models are not the primary end-user interface.

Instead, they provide the structured data layer that powers relational views.

A single model can support multiple views, each designed for a different purpose.

Example:

- one workforce model
- multiple views for data entry, reporting, and analysis

This allows you to separate data structure from user presentation.

* * *

## Designing Relational Models (Best Practices)

### Start with the Business Use Case

Design the model around the business question, workflow, or maintenance process you are trying to support.

✔️ Good:

- Workforce Planning Model
- Asset Management Model
- Product Forecast Model
- Customer Transaction Analysis Model

❌ Avoid:

- overly generic models with no clear purpose

* * *

### Match the Model to Its Intended Use

If the model will support data entry, keep it centered around a primary table and the related foreign key tables needed to maintain and describe those records.

If the model will support reporting or analysis, it can join more broadly across related tables, hierarchies, and transaction structures.

* * *

### Keep Models Focused

Each model should support one primary business purpose.

Avoid trying to combine too many unrelated entities into a single model.

* * *

### Use Reusable Structures

Models should be designed so they can support more than one downstream view when appropriate.

This improves consistency and reduces duplication.

* * *

### Build on Well-Defined Tables

Strong models depend on well-structured relational tables and clear relationships.

Good table design makes model design much easier.

* * *

## Example Use Cases

### Example 1: Data Entry Model

You want to maintain employee setup data.

You might create a model based on:

- Employee Table
- Department Table
- Location Table

This allows users to enter and maintain employee records while referencing related department and location information.

### Example 2: Read-Only Analysis Model

You want to analyze customer transactions.

You might create a model based on:

- Customer Table
- Transaction Table
- Product Table
- Organization Hierarchy Table

This allows users to review and analyze related data across master and transaction structures.

* * *

## Tips

- start with the intended use case
- use a single-table-centered model for most data entry use cases
- use broader read-only models for analysis and reporting
- keep relationships easy to understand
- reuse models when they support multiple views

* * *

## Next Steps
- Learn about [Relational Models](../appGroups/admin/relationalModels/index.md)
- Learn about [Relational Views](views.md)
- Review your table and relationship definitions before building more advanced views