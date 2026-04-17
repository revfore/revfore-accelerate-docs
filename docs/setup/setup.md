# Setup and Installation

This guide walks you through installing and configuring Revfore Accelerate in your OneStream environment.

---

## 📦 Dependencies

Revfore Accelerate requires:

- OneStream **version 9.1.1 or later**
- A standard OneStream installation (no additional external dependencies required)

### Security Requirements

The user installing the solution will need to be part of the Administrator security group.

---

## 🛠️ Installation Steps

### Step 1: Import Solution Package

- Download the Revfore Accelerate solution package from the OneStream Solution Exchange
- Import it into your OneStream application by going to Application -> Tools -> Load/Extract and loading the zip file.

---

### Step 2: Request and Install Starter License Key

- Request a **Starter License Key** from OneStream which will allow you to use the Designer pages
- Provide your **Customer Name**, located in:
  - `Application → Application Properties`
- Once received:
  - Paste the key into the **License Key** field
  - Click **Validate**

---

### Step 3: Setup Database

- This step initializes the solution by:
  - Creating all required relational tables
  - Creating views and supporting objects
  - Seeding base configuration data

---

### Step 4: Launch Solution

- If not already there, Open the Revfore Accelerate Designer dashboard
- Select **Admin** from the main menu:
  - Begin building your own relational solution by creating new tables, models, and views
  - Note: You will need a Full license key before you can use the Views in the end-user Navigation pages

---

### Step 5: Request and Install Full License Key

- A **Full License Key** is required for full functionality including the Navigation pages
- To request:
  - Navigate to:  
    `Config → Manage License Key`
  - Copy the **Customer Reference Code**
  - Send it to OneStream to request a Full License Key

- Once received:
  - Paste the key into the **License Key** field
  - Click **Validate**
  - You can now use any of the Relational Views in the end-user Navigation pages

---

## 💡 Tips
  
- Review existing Relational Table, Model and View definitions to get familiar with them
- Start with a simple use case (e.g., rate tables or reference data)  

---

## 👉 Next Steps

- Return to the [Getting Started Guide](../index.md)
- Learn about [Relational Tables](../concepts/tables.md)
- Explore [Relational Models](../concepts/models.md)
- Build your first [Views](../concepts/views.md)