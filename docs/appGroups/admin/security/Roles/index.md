# Security Roles

[← Back to Admin](../../index.md)

Security Roles are **permissions to do something**, as opposed to access to data. Each role is mapped to one
[security group](../Groups/index.md), and the members of that group hold the role.

Where a [security group](../Groups/index.md) on a view, unit or cycle decides **what data** a user can see and
change, a role decides **whether a user may perform an action** - for example, approving a request above a limit. A
solution checks for the role by its code, so you decide who holds it by choosing the group, and you can change that
at any time without any change to the solution.

!!! note "Administrators hold every role"
    Members of OneStream's **Administrators** group, and the built-in administrator account, hold every role
    whatever group it is mapped to - so no mapping can lock an administrator out. Every role starts out mapped to
    **Administrators**, so until you map a group of your own, only administrators hold it.

## Overview

Use the Security Roles page to:

- see the roles available in this application - Revfore's own and those your solutions add
- choose the security group that holds each role
- disable a role, so that only administrators hold it

## Security Role Record Fields

| Field | Data Type | Purpose | Notes |
|---|---|---|---|
| Name | nvarchar | The role's name. | Must be unique.
| Description | nvarchar | What the role allows. | Required.
| Security Group | int | The security group whose members hold the role. | Defaults to **Administrators**. Nested groups count: members of groups nested under it hold the role too.
| Integration Code | nvarchar | The code the solution checks. | Must be unique. Cannot be changed on Revfore's roles.
| Is Extension | bit | Whether a solution added the role. | Read-only. Off for Revfore's roles, on for roles a solution adds, and for any role added on this page.
| Is Enabled | bit | Whether the role is in use. | A disabled role is held by administrators only.
| Created At | datetime | Date and time the record was created. |
| Updated At | datetime | Date and time the record was last modified. |
| Created By | int | User who created the record. |
| Updated By | int | User who last modified the record. |
| Security Role Id | int | Unique identifier for the role record. | Assigned by the system.

## Map a role to a security group

1. Go to **Admin | Security | Roles**
2. Find the role, and in **Security Group** choose the group that should hold it
3. Save

The change takes effect for each user's next action. If the group you want is not in the list, add it in OneStream
and **Sync** on [Security Groups](../Groups/index.md) first.

## Who can change roles

Only an administrator who is not [viewing as another user](../../../../security/impersonation.md) can add, change or
delete roles - otherwise a user holding a role could map their own group to every role.

- **Revfore's roles** (Is Extension off) cannot be deleted, and their Integration Code cannot change. To take a
  role away from everyone but administrators, map it back to Administrators or disable it.
- **Roles added by a solution** (Is Extension on) arrive with the solution's setup package, mapped to Administrators.
  Map them to your own groups once the solution is installed.
- Roles are deleted one at a time. They cannot be deleted by a filter or all at once.

## Revfore's roles

| Role | Integration Code | Covers |
|---|---|---|
| Revfore Administrator | `RevforeAdmin` | Configuring the Framework's **extension** tables, models, views and pages: creating models and views, adding columns, sources, relationships and actions to them, resequencing columns, syncing their database structure, dropping database tables and views, rebuilding SQL views, adding table and view definitions, configuring pages, relinking to OneStream records, loading files that contain structure, and loading setup packages that contain no SQL. Also syncing users, security groups, cubes and dimensions with OneStream, viewing the application as another user, debug settings, and the license key. |
| Developer | `Developer` | Switching debug logging, and verbose debug, on for a user. Give it to developers, or an agent account, who need debug without administrator rights. |

### What stays with OneStream administrators

These need membership of OneStream's **Administrators** group, whatever the roles are mapped to:

- installing, upgrading and uninstalling Revfore - **Setup & Upgrade**, **Uninstall**, **Create New Instance**
- **Run Maintenance Scripts**, and any setup package that contains SQL
- adding or changing security roles
- changing Revfore's own tables, models, views and lookups, or anything under them - their columns, indexes,
  relationships, sources, actions, filters and security

### Loading data files

Loading a data file does not need a role. Each view the file loads into needs your **write access** - the same access
that lets you edit its rows in the grid, including write access to any column the file sets that is secured
separately. A file that loads into a view you can't write to is refused, names the view, and saves nothing. A file
that adds or changes tables, models or views needs the Revfore Administrator role.

The same check applies wherever you change data: saving in the grid, **File Import**, **Delete**, **Copy** and
**Bulk Update** (which also needs write access to the column you choose). Exporting follows your **read access**:
**File Export** and **Extract** leave out columns you can't see in the grid, and Extract skips a view you can't open.

## Notes

- Map a role to a group named for the permission ("Capital Approvers"), not for a team, so the mapping still reads
  correctly after the team changes.
- A user who holds a role but is [viewing as another user](../../../../security/impersonation.md) cannot use it until
  they stop.
- A user without the role who tries the action sees a message naming the role, for example
  *"Security Error: you need the Acme.Approver security role to approve a request above the limit."*
