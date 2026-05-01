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

### Step 1: Set your OneStream Customer Name and Request a Starter License Key

- Go to the Solution Exchange at https://solutionexchange.onestream.com/
- Locate and Select the Revfore Accelerate Solution
- Click on **Download** and then **Request a key**
- Take note of the **Customer Name** value on the Request Information Page.  
- Open OneStream and go to `Application → Application Properties`
- Ensure the value in the **Company Name** field matches your company name in the Solution Exchange Request Information page
- Go back to Solution Exchange Request Information page the fill out all the fields, click **Accept** and **Submit**
- Once you have received your **Starter key**, go to Step 2

!!!
    The solution supports a **Starter License Key** that provides **Only Designer** access and a **Full License Key** that provides **Both Designer and Navigation** access.
    A **Starter License Key** can be generated prior to the installation of the solution with just the application Customer Name found at (Application -> Application Properties -> Customer Name)
    **The Customer Reference Code** which is found on the **Manage License Key** page is required to obtain a **Full License Key** Please provide this code to OneStream when requesting your **Full License Key**.
---

### Step 2: Import Solution Package

- Go to the Solution Exchange at https://solutionexchange.onestream.com/
- Locate and Select the Revfore Accelerate Solution
- Click on Download and Enter you Starter Key 
- Download the Revfore Accelerate solution package from the OneStream Solution Exchange
- Import it into your OneStream application by going to `Application -> Tools -> Load/Extract` and loading the zip file.

---

### Step 3: Launch Revfore Accelerate Designer

- Go to `OnePlace → Dashboards → Revfore Accelerate Designer (RFA) → Revfore Accelerate Designer`
- The **Manage License Key** page should appear

---

### Step 3: Enter a Starter License Key

  - Paste the **Starter Key** into the **License Key** field
  - Click **Validate**
  - Click **Launch Solution**
  - The **Setup Solution** page should appear

!!!
    If you already have a **Full License Key**, enter it instead of the Starter License Key
---

### Step 4: Setup Database

- Click **Setup Database**
  - You will need to be part of the **Administrators** security group to run this step.
  - This step initializes the solution by:
    - Creating all required relational tables
    - Creating views and supporting objects
    - Seeding base configuration data

---

### Step 5: Request/Enter a Full License Key, if you only have a Starter License Key

- Click **Request a Full License Key**
  - Copy the **Customer Reference Code**
  - Send it to OneStream to request a Full License Key
  - Proceed to next step if you have to wait for a Full License Key.  If not, enter the Full License Key and click **Validate**

!!!
    Once you receive the **Full License Key**, return to this page found at Config -> Manage License Keys
    - Paste the key into the **License Key** field
    - Click **Validate**
    - You can now use any of the Relational Views in the end-user Navigation pages

---

### Step 6: Launch Solution

- Click on **Launch Solution**
- Select **Admin** from the main menu:
  - Begin building your own relational solution by creating new tables, models, and views

!!!
    You will need a Full license key before you can use the Views in the end-user Navigation pages


---

## 💡 Tips
  
- Review existing Relational Table, Model and View definitions to get familiar with them
- Start with a simple use case (e.g., rate tables or reference data)  

---

## 👉 Next Steps

- Return to the [Getting Started Guide](../index.md)
- Learn about [Relational Tables](../concepts/tables.md)
- Explore [Relational Models](../concepts/models.md)
- Build your first [Relation Views](../concepts/views.md)