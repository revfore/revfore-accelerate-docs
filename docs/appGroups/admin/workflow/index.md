# Workflow

[← Back to Admin](../index.md)

Workflow is what turns a set of relational tables into a **process**: it says who a piece of work is for, when it is being done, and what state it is in.

A solution is driven by **either Revfore workflow or OneStream workflow** — you choose one or the other. What is not a choice is the underlying structure: **both options are built on the Revfore workflow tables.** Choosing OneStream workflow does not replace them, it drives them.

That means the shape of a solution is the same either way. A workflow unit says *who* a piece of work is for, a workflow instance says *when*, and a solution's own tables carry both. Choosing OneStream workflow adds one further step — linking those units and instances to the OneStream workflow they correspond to — rather than removing anything.

So the decision is about **where the process is governed from**, not about which tables to build.

## The structures both options use

Revfore workflow is assembled from a small number of pieces:

- [Units](units/index.md) – **who** the work is performed for: a department, cost centre, entity or region. Units carry the security groups that control access, effective dates, and the [member sets](units/memberSets.md) that map a unit's data to the cube.
- [Instances](instances/index.md) – **when**: a single cycle of the process, paired with an [instance type](instances/instanceTypes.md) that says what kind of cycle it is.
- [Areas](areas/index.md) – how a large process is partitioned into manageable sections.
- [Supporting](supporting/index.md) – the vocabulary the process is described in: [item types](supporting/itemTypes.md), [item categories](supporting/itemCategories.md), [member set types](supporting/memberSetTypes.md) and the pairings between them.

An instance and a unit together identify a specific piece of work: the instance says *when*, the unit says *for whom*.

To make a solution's own records workflow-aware, add the workflow [standard columns](../relational/supporting/standardColumns.md#workflow) to its tables. **A table needs `WorkflowUnit_Int` and `WorkflowInstance_Int` whichever workflow drives the process** — they are what let a record be filtered, secured and progressed by the same mechanics as every other workflow-enabled view, with no per-solution code.

!!! note "Workflow is configured in two places, and both are required"
    The columns are only the input. Workflow also has to be **enabled on the [Relational View](../relational/relationalViews/views.md)** and configured on the **[Content Sub Item](../../../concepts/metadataDrivenUI/contentSubItem/index.md)** that renders it. Each carries its own workflow settings, and both must be set — configuring one without the other leaves the page non-workflow, even though the table and model are correct.

    The setting also names **which** workflow drives the view — Revfore or OneStream — so it must match the option the solution has chosen.

## Revfore workflow

Choose Revfore workflow when the process is governed inside the application. Typically that means one of the following.

- **Users will be working in Genesis.** The process is reached through Genesis pages and navigation, so there is no OneStream workflow step for it to hang off.
- **The workflow unit needs a scope that is not entity-dependent.** A OneStream workflow profile is tied to an entity. A Revfore workflow unit is not: through its [member set](units/memberSets.md) it can be set to **any scope or slice of the cube, including all the UD dimension members**. That makes the unit whatever the process is actually performed for — a product line, a project, a channel, a cost centre, or any combination the UDs can express — rather than forcing the process into an entity structure that does not fit it.
- **The process is specific to the solution**, does not correspond to a OneStream workflow step, or needs more granularity than one.

Units and instances are defined and progressed here, and nothing further is required — the [OneStream Workflow Reference](#linking-the-units-and-instances) fields stay blank.

## OneStream workflow

Choose OneStream workflow when the process is one OneStream is already running — a close, budget or forecast cycle — and the solution needs to sit inside that cycle rather than beside it. Typically that means one of the following.

- **Users are already working in OneStream workflow.** The process is part of a cycle they run today, and the solution should be a step within it rather than somewhere else they have to go.
- **The Revfore dashboards should be surfaced from OneStream workflow.** A Content Item dashboard configured in Genesis can be opened directly from a workflow profile input step, so users reach it without leaving the workflow — see [below](#linking-a-workflow-profile-form-to-a-revfore-content-item-dashboard).

Note that the workflow unit then follows OneStream's structure: a workflow profile is entity-based, so this is the option that ties the unit to an entity.

The Revfore workflow units and instances are still required, and a solution's tables still carry the workflow standard columns. What is added is the link between the two.

### Linking the units and instances

Each Revfore unit and instance names the OneStream workflow it corresponds to, through the **OneStream Workflow Reference** field:

- on a [unit](units/units.md), the OneStream **parent workflow profile name**
- on an [instance](instances/instances.md), the scenario and time key, as `Scenario|TimeKey` (e.g. `Budget|2026003000` — see [the time key format](instances/instances.md#onestream-workflow-reference))

These are what tie the two workflows together, so they must be populated for every unit and instance participating in the OneStream-driven process. They are left blank only when the solution is driven by Revfore workflow.

Where a record needs to hold OneStream's own keys directly — to reconcile against a OneStream step, or to confirm one is complete before allowing a relational action — use the `OneStreamWorkflow*Key` [standard columns](../relational/supporting/standardColumns.md#workflow).

### Linking the user experience

A OneStream workflow profile step can open a Revfore page directly, so users reach the solution through the workflow they already use rather than navigating to it separately. See below.

## Linking a Workflow Profile form to a Revfore Content Item dashboard

This links a OneStream workflow profile input step to a [Content Item](../../../concepts/metadataDrivenUI/contentItem.md) dashboard that has already been configured in [Genesis](../../../integrations/genesis/index.md).

**1. Find the Linked Page Reference.**

In Genesis, open the workspace in **designer mode**, select the page, and click **Setup** in the form header. Take note of the **Linked Page Reference** on the Revfore Setup page — for example `Home_Planning_Submit_Set1000`.

**2. Open the workflow profile.**

Go to **Workflow | Workflow Profiles** and navigate to a base input form.

**3. Set the Description.**

In the **General** section, type the Linked Page Reference into the **Description** field.

**4. Point the step at the dashboard.**

Under **Workflow settings**:

- select **Workspace** as the Workflow Name
- select the **same Revfore Content Item dashboard** that was selected in Genesis

!!! note "The dashboard must match on both sides"
    The Content Item dashboard chosen on the workflow profile has to be the one Genesis is configured against, and the Description has to carry that page's Linked Page Reference. If either differs, the step will not resolve to the intended page.
