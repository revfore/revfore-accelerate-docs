---
title: Viewing as Another User
---

# Viewing as Another User (Impersonation)

Impersonation lets an administrator **see the application exactly as another user sees it** — the same views, columns, buttons, workflow units and records — without signing in as them. It is the quickest way to answer "why can't Sam see this?" or to check a new user's access before they start.

Impersonation is **for looking, not for working**: while you are impersonating, nothing can be changed.

---

## Who can impersonate

- **Administrators** (members of OneStream's Administrators group) and the **built-in administrator account**.
- You can only turn it on **on your own user record**.
- You can only impersonate someone whose access **you already have**. If the other user belongs to a security group you don't — or has more rights in a group than you do — you are told which groups and impersonation is refused. This means impersonation can never show you anything you couldn't already see.
- Nobody can impersonate the built-in administrator account except that account itself.

---

## Start impersonating

1. Go to **Admin | Security | Users**.
2. Find **your own** user record and click **Edit**.
3. In **Impersonate User**, choose the user you want to view as.
4. Click **Save**.
5. Close and reopen the page you want to check.

From now on the application shows you what that user sees.

---

## What changes while you impersonate

| | While impersonating |
|---|---|
| **Views, columns and buttons** | As the other user sees them. Views they can't open are empty for you too. |
| **Workflow units and instances** | Only those the other user can read. |
| **"My records" filters** | Show the other user's records. |
| **Making changes** | Not allowed. Grids are read-only, and saving, deleting or copying shows: *"You are impersonating another user, so changes are not allowed. Clear Impersonate User on your own user record to stop."* |
| **Audit history** | Anything recorded still names **you**, not the user you are viewing as. |

---

## Stop impersonating

1. Go to **Admin | Security | Users**.
2. Edit **your own** user record.
3. Clear **Impersonate User**.
4. Click **Save**, then close and reopen your pages.

The **Users** page always works as *you*, even while you are impersonating — so you can always get back to it to stop, even if the user you are viewing as could never open it themselves.

---

## Messages you may see

| Message | Meaning |
|---|---|
| *Only administrators can impersonate another user.* | You are not an administrator. |
| *Impersonation can only be set on your own user record…* | You edited someone else's record. Edit your own. The message shows the record you edited and the user you are signed in as; if it says your sign-in does not match any user record, your OneStream user name and your user record's name differ — ask your administrator. |
| *You cannot impersonate … they have rights you do not, in these security groups: …* | The other user has access you don't. You need to be in (at least) the groups listed. |
| *Only user 1 can impersonate user 1.* | The built-in administrator account cannot be impersonated. |
| *You are impersonating another user, so changes are not allowed…* | You tried to change something while impersonating. Stop impersonating first. |
