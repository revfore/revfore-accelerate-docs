# Create New Instance

[← Back to Config Summary](index.md)

**Create New Instance** is used to install an additional instance of Revfore Framework inside the same OneStream application.

Each instance gets its own workspace and its own custom SQL schema, so instances are fully separated from one another — their tables, models, views and data are independent. You can create as many as you need.

## Overview

Use Create New Instance to:

- run more than one Revfore Framework instance in a single OneStream application
- keep each instance's relational structures and data separate
- stand up an instance for a different purpose, team, or environment

The process produces a new install file. It does not install anything by itself — you create the file here, then load it through the OneStream Load/Extract process.

## When to use it

An additional instance is worth creating when work needs to be genuinely separate rather than merely organised:

- separating solutions that must not share tables, views, or data
- giving a team or business area its own environment within the same application
- keeping development, testing, or demonstration work away from a live instance

Where separation is not required, a single instance with well-named solutions is usually simpler to maintain.

## Create the instance file

### Step 1: Download and unzip the full install file

- Download or locate the **full install file** for the version of Revfore Framework you want the new instance to run
- Unzip it and extract the included `ApplicationWorkspaces.xml` file

The new instance is created at the version of the install file you select, so use the version you actually intend to run.

### Step 2: Select the file and create the new instance file

- Click **Select & Create Xml File**
- Select the `ApplicationWorkspaces.xml` file you extracted

This creates a new install xml file for the new instance and saves it to your OneStream file explorer. The path is shown on screen once it has been created.

### Step 3: Download the file

- Go to the OneStream **File Explorer**, by clicking File Explorer at the top left of your OneStream application
- Locate the file at the path shown and download it to a local folder

### Step 4: Load the file

This is a manual step.

- Load the new instance xml file through the **Application tab | Tools | Load/Extract** process in OneStream

Once the file is loaded, relaunch the application. The new instance then follows the normal setup process — see [Setup & Upgrade](setup-upgrade.md).

## Notes

- Each instance has its own custom SQL schema, so nothing is shared between instances. Structures cannot be referenced across them.
- The new instance is created at the version of the install file you select. To create an instance at a different version, start again from that version's install file.
- Creating the file changes nothing on its own — the instance does not exist until the file is loaded through Load/Extract.
- Because each instance is separate, each is set up, licensed, and upgraded on its own.
