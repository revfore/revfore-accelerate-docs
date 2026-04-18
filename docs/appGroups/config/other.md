# Other

[← Back to Config Summary](index.md)

The **Other** section includes additional system-level actions that administrators can run as needed.

These actions are generally safe to run and are helpful for synchronizing metadata or relinking records after installs or upgrades.

## Available Actions

### Sync All Managed Relational Views

This action loops through all managed relational views and syncs their definitions with the actual SQL views in the database.

#### When to Use

This is helpful when:

- you have made changes to relational view definitions
- you want to ensure those changes have been applied to the database
- you want to confirm that all managed SQL views are in sync

#### Notes

- This action is safe to run anytime.
- The system automatically runs this during setup and after any upgrade.

---

### Relink Forms and Users

Forms and Users are linked using both:

- their hidden unique IDs
- their names

This process takes the names and updates the unique IDs.

Revfore Accelerate normally uses the unique IDs, so these records need to be relinked after an install or upgrade.

#### When to Use

Use this action when:

- forms or users need to be relinked after an install
- forms or users need to be relinked after an upgrade
- the automatic relinking process did not complete as expected

#### Notes

- This process should automatically run, but it can also be run manually here if needed.
- It is safe to run anytime.
- Revfore Accelerate automatically creates user records when it encounters a new user login.