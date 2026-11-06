# Security Roles

[← Back to Admin](../../index.md)

Security Roles are **permissions to do something**, as opposed to access to data. Each role is mapped to one
[security group](../Groups/index.md), and the members of that group hold the role.

Where a [security group](../Groups/index.md) on a view, unit or instance decides **what data** a user can see and
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
| Revfore Administrator | `RevforeAdmin` | Configuring the Framework: tables, models, views, pages, settings and users. |
| Solution Installer | `SolutionInstaller` | Running setup packages and SQL scripts, and uninstalling a solution. |
| Schema Manager | `SchemaManager` | Creating, changing and dropping database tables and the SQL views built for views. |
| Impersonator | `Impersonator` | Viewing the application as another user. |
| Developer | `Developer` | Switching debug logging, and verbose debug, on for a user. |
| License Manager | `LicenseManager` | Entering or changing the license key. |

!!! warning "Not yet in effect"
    In this release, the Framework's own screens and actions still require membership of OneStream's
    **Administrators** group directly. Revfore's roles are in place so they can be mapped ahead of time, but mapping a
    group to one of them does not yet give that group's members anything. Roles added by your solutions take effect
    as soon as they are mapped.

## Notes

- Map a role to a group named for the permission ("Capital Approvers"), not for a team, so the mapping still reads
  correctly after the team changes.
- A user who holds a role but is [viewing as another user](../../../../security/impersonation.md) cannot use it until
  they stop.
- A user without the role who tries the action sees a message naming the role, for example
  *"Security Error: you need the Acme.Approver security role to approve a request above the limit."*
