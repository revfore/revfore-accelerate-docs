# Uninstall

[← Back to Config Summary](index.md)

The **Uninstall** section is used to remove some or all of Revfore Accelerate.

There are two uninstall options, depending on whether you want to remove only the application components or completely remove the entire solution.

## Uninstall Options

### Option 1: Uninstall UI / Business Rules

This option removes the UI and Business Rules stored in maintenance units while leaving the database in place.

This is typically used as part of an upgrade-related process where application components need to be removed and reloaded.

You can choose a subset of maintenance units to remove.

#### Typical Use

In most cases, users should use the **Upgrade** option in **Setup & Upgrade** instead of this uninstall option.

Use this option only if directed by Revfore.

### Option 2: Uninstall Full

This option removes everything, including:

- the database
- all maintenance units
- any custom work that has been done in the solution

#### Use This Option Only When

Use **Uninstall Full** only when:

- you are no longer using the solution, or
- you want to fully reset the environment by uninstalling and reinstalling it

## Warning

A full uninstall removes all solution components, including customizations. Make sure you fully understand the impact before proceeding.