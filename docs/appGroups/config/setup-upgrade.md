# Setup & Upgrade

[← Back to Config Summary](index.md)

The **Setup & Upgrade** section is used to install Revfore Accelerate for the first time and to upgrade it later to a newer version.

The available options change depending on whether the database has already been set up and, if so, what version of the database currently exists.

## Overview

Use this section to:

- perform the initial setup of the solution
- create the required database objects
- launch the solution after setup
- prepare and process upgrade files
- upgrade the database for a newer version

---

## Initial Setup

### Step 1: Setup Database

Use **Setup Database** to create the initial database structures required by Revfore Accelerate.

This includes items such as:

- schemas
- tables
- stored procedures
- views
- default data

This step prepares the environment for the solution to run.

### Step 2: Launch Solution

After the database setup is complete, the solution is ready to use.

At that point, users will typically go to the **Admin** application group to begin reviewing existing and creating new:

- Relational Tables
- Relational Models
- Relational Views

---

## Upgrade

When upgrading Revfore Accelerate, both the application components and the database may need to be updated.

### Step 1: Create Upgrade File, Uninstall, and Load

To upgrade Revfore Accelerate, you must first create an upgrade file from a full install package.

#### Instructions

Please create an upgrade file from a Revfore Accelerate full install XML file.

You will need to download the full install ZIP file for the desired version from the **OneStream Solution Exchange**, unzip it, and select the `ApplicationWorkspaces.xml` file here.

In addition, you will need to choose whether to upgrade:

- both **Core** and **Custom** RFA maintenance units, or
- only the **Core** RFA maintenance units

#### Important Note

The **Genesis maintenance units** are always ignored during a Revfore Accelerate upgrade.

If you want to upgrade the Genesis maintenance units, go to the **Revfore Accelerate Designer** page and then navigate to:

1. **Settings**
2. **Instance Management**
3. **Upgrade**

#### After Processing the File

After the file is selected and processed, an XML file is created at the displayed path.

You will need to:

1. save that file to a local folder
2. load it through the **Application Load/Extract** process in OneStream

### Step 2: Upgrade Database

Use **Upgrade Database** to apply any database changes required for the upgraded version.

This step updates the database structures as needed to align with the new application version.

## Notes

- The Setup & Upgrade section is the primary place to install and upgrade Revfore Accelerate.
- In most upgrade scenarios, this section should be used instead of manually uninstalling maintenance units.
- Available actions may vary depending on the current database state and version.