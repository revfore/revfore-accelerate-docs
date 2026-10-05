---
title: What You Can See and Do
---

# What You Can See and Do

What you can see and change in the application is decided by the **security groups** you belong to. Your administrator places you in groups in OneStream; you don't set anything yourself.

This page explains what that looks like day to day — what happens when you open something you don't have access to, why some workflow units or views don't appear for you, and what to ask for when you need more.

---

## Views you don't have access to

Every view (grid) has a group that may read it. If you open a page containing a view you are not allowed to read, the page still opens normally — that view is simply **empty**:

- no columns and no records are shown
- no buttons (Add, Edit, Delete and so on) are shown
- the title of the grid reads **"… | You don't have access to this view"**

Everything else on the page keeps working, so you can carry on with the parts you do have access to, or move to another page.

!!! tip "Need access?"
    Ask your administrator to add you to the view's read group. Tell them the view name shown in the title.

### Clicking through to a view you can't open

If you click a button or link that would open a view you don't have access to — for example **Edit+**, a related view, or a **Navigate To** entry — you get a message instead:

> You don't have sufficient rights to open this view.<br>
> View Name: …

Close the message and carry on; nothing has changed.

### Lists only show what you can open

To avoid listing things you can't use, these lists only show views you are allowed to open:

- the **Navigate To** tree of related views
- the **Child Objects** list in the **Edit+** dialog

If none of a record's child views are open to you, **Edit+** still opens so you can work on the record itself — the Child Objects list and the child grid are just empty.

### Read-only columns and hidden columns

A view can also restrict individual columns or buttons. Depending on your groups you may see a column you can't change, or not see a column at all. That is configured by your administrator and is expected.

---

## Workflow units and instances

When you choose your workflow — the **instance** (cycle) and the **unit** you are working on — you only see the ones you are allowed to read.

- **Instances and units you can't read don't appear** in the workflow selectors or in drop-downs that list them.
- **In the unit tree**, a parent unit you can't read is still shown so the tree keeps its shape and you can reach the units beneath it. If you select it, you are told you don't have access to it.
- **Records** belonging to units you can't read don't appear in grids that show "this unit and the units below it".

!!! note "For administrators"
    Visibility follows the unit's or instance's **Read Security Group**, including groups nested beneath it. A unit or instance with no read group is visible to no one. Make sure users in the read-write group are also covered by the read group, for example by nesting the read-write group under it. The **Admin | Workflow** maintenance grids are not filtered, so administrators can always maintain every unit and instance.

### The workflow header

Grids that follow your workflow show it in their title:

**View name | Unit | Instance | Instance status | Unit status**

The unit status appears only when your solution uses per-unit status. If the title reads **WORKFLOW SELECTION IS MISSING**, choose a workflow unit and instance before working on that page. The **Navigate** dialog shows the same header as the main grids.

---

## Common questions

**A grid is empty and its title says I don't have access.**
You are not in the view's read group. Ask your administrator for access, quoting the view name.

**I can't find a workflow unit or instance my colleague can see.**
You are not in its read group. Ask your administrator.

**I can see a parent unit in the tree but can't select it.**
You can read units beneath it but not the parent itself. That is expected; ask your administrator if you need the parent too.

**Everything is read-only and I get "You are impersonating another user…" when I save.**
You are viewing the application as another user. See [Viewing as Another User](impersonation.md) for how to stop.
