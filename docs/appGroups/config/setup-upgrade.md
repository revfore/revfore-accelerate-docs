# Setup & Upgrade

[← Back to Config Summary](index.md)

The **Setup & Upgrade** section is used to install Revfore Accelerate for the first time and to upgrade it later to a newer version.

The available options change depending on whether the database has already been set up or not and the stage of the upgrade process.

## Overview

Use this section to:

- perform the initial setup of the solution
- create the required database objects
- launch the solution after setup
- prepare and process upgrade files
- upgrade the database for a newer version

---

## Setup

### Step 1: Setup Database

Use **Setup Database** to create the initial database structures required by Revfore Accelerate.

This includes items such as:

- schemas
- tables
- stored procedures
- views
- default data

This step prepares the environment for the solution to run.

### Step 2: Request a Full License Key

Use this step to copy the **Customer Reference Code** from the **Manage License Key** page and send it to OneStream to request a Full License Key

!!!
    Once you receive the **Full License Key**, Go to the Config -> Manage License Keys page
    - Paste the key into the **License Key** field
    - Click **Validate**

### Step 3: Launch Solution

After the database setup is complete, the solution is ready to use.

At that point, users will typically go to the **Admin** application group to begin reviewing existing and creating new:

- Relational Tables
- Relational Models
- Relational Views

---

## Upgrade

When upgrading Revfore Accelerate, both the application components and the database may need to be updated.

Here are the options
- Upgrade All (Genesis & Core RFA & Custom RFA)
- Upgrade Only Core RFA & Custom RFA
- Upgrade Only Core RFA

---
## Upgrade All (Genesis & Core RFA & Custom RFA)

You will need to download the full install zip file for the desired version from the OneStream Solution Exchange and use it for the upgrade

### Step 1: Uninstall All Maintenance Units

This will remove all maintenance units including Genesis, Core RFA and Custom RFA maintenance units.

### Step 2: Load File

This is a manual step.  You will need to manually load the full install package zip file through the Application tab | Tools | Load/Extract process

- Download the desired version of Revfore Accelerate from the **OneStream Solution Exchange**
- Load the full install package zip file through the **Application tab | Tools | Load/Extract** process in OneStream

After you have loaded the file, relaunch the application and finalize the upgrade.  See **Upgrade - Finalize** below.

---
## Upgrade Only Core RFA & Custom RFA | Upgrade Only Core RFA

To upgrade only a subset of the maintenance units, you must must create an upgrade file from a full install package.

You will need to download the full install zip file for the desired version from the OneStream Solution Exchange, unzip it and select the ApplicationWorkspaces.xml, which will then create an upgrade file.

!!!!Note: The Genesis maintenance units will be ignored.  If you want to upgrade to a newer version of Genesis, please go to the Revfore Accelerate Designer page, click on the Settings button on the top right, then Instance Management and finally the Upgrade tab.

### Step 1: Select & Create Xml File

>Step 1a: Download and Unzip the Revfore Accelerate full install zip file

- Download the desired version of Revfore Accelerate from the **OneStream Solution Exchange**
- Unzip it and extract the `ApplicationWorkspaces.xml` file

>Step 1b: Create the upgrade file

- Click on **Select & Create Xml File**
- Select the `ApplicationWorkspaces.xml` file you extracted
- This will create an upgrade xml file and save it to your OneStream file explorer.  The path should be provided.
- Go to the OneStream File Explorer, by clicking on the File Explorer at the top left of you OneStream application
- Locate the file and download it to a local folder

### Step 2: Uninstall RFA Maintenance Units

- Click on **Uninstall RFA Maintenance Units**
- This will uninstall the RFA Maintenance Units you chose to remove

### Step 3: Load File

This is a manual step.  You will need to manually load the new upgrade .xml file that was just created through the Application tab | Tools | Load/Extract process

- Load the upgrade .xml file through the **Application tab | Tools | Load/Extract** process in OneStream

After you have loaded the file, relaunch the application and finalize the upgrade.  See **Upgrade - Finalize** below.

---
## Upgrade - Finalize

After relaunching the solution, you will first be prompted to re-enter your Full License Key
- Re-enter **Full License Key**
- Click **Validate** and **Launch Solution**
- The **Finalize Upgrade** page should appear

### Step 1: Finalize Upgrade

>Click on **Finalize Upgrade** to apply any database changes required for the upgraded version, refresh all Relational Views and to relink Forms and Users

### Step 2: Launch Solution

After the database upgrade is finalized, the solution is ready to use.


## Notes

- The Setup & Upgrade section is the primary place to install and upgrade Revfore Accelerate.
- In most upgrade scenarios, this section should be used instead of manually uninstalling maintenance units.
- Available actions may vary depending on the current database state and version.