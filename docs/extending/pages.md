# Solution Pages

[← Back to Extending the Framework](index.md)

A solution's screens are **pages** in the Revfore navigation. An application group, such as **Home**, holds navigation groups, and a navigation group holds pages. Each page has a [Content Item](../concepts/metadataDrivenUI/contentItem.md) layout with one or two [Content Sub Items](../concepts/metadataDrivenUI/contentSubItem/index.md): a drop-down of grids, a list, summary cards, cube views, and so on. What each content sub item shows is set by the page's **linked page settings**.

A page is set up in two parts, in this order:

1. **Create the page in OneStream**: its navigation entry and the page itself. These are workspace objects, created by hand, and they ship in your workspace XML.
2. **Configure what the page shows**: the linked page settings. These are data. Claude writes them in `420-<Solution>-pages.json`, and the [setup package](setup-packages.md#pages) loads them on install.

Keeping the settings as data means the same file sets up the page in every environment the solution is installed in, without anyone repeating the configuration by hand.

## Names and the linked page reference

Every page has a **linked page reference**, built from the three names you give it in OneStream, joined with `_` and always ending `_Set1000`:

```
<ApplicationGroup>_<NavigationGroup>_<Page>_Set1000
     Home        _ ProjectTracking _ Projects _Set1000   ->   Home_ProjectTracking_Projects_Set1000
```

| Name | What it is | Example | Rule |
|---|---|---|---|
| Application group | The top-level menu the page lives under | `Home` | One of `Home`, `Admin` or `Config`. You can't create your own |
| Navigation group | The group of page buttons your solution adds under that menu, normally one per solution, named after it. Created once, with its first page | `ProjectTracking` | Letters and digits only. No spaces or underscores |
| Page | One page button in that navigation group | `Projects`, `Supporting` | Letters and digits only. No spaces or underscores |

**Which application group:**

- **Home** is for end users' screens: everyday work, and the business data they maintain, such as a customer list.
- **Admin** is for administrators' screens: lookups, reference lists and mappings on a **Supporting** page, and the solution's general settings, if it has any, on a **Settings** page.
- **Config** is not used for a solution's screens.

Home's caption — what the menu shows — can be changed, to "Revenue" for example. Its name stays `Home`, and the reference always uses the name.

!!! warning "Type the names exactly"
    The three names are what connect the page you create in OneStream to its settings in `420-<Solution>-pages.json`. A page created with any other spelling never picks up its settings, and nothing reports it. The text on the page's button is separate and can be anything, so `Projects` can show as "Projects and Tasks".

When Claude writes the pages, it lists each one split into these names, with the dashboard to link to it:

| Application group | Navigation group | Page | Linked dashboard | Linked page reference |
|---|---|---|---|---|
| `Home` (existing) | `ProjectTracking` (new) | `Projects` | `pge_RfaConItm101_Set1000_0_ContentItem` | `Home_ProjectTracking_Projects_Set1000` |
| `Admin` (existing) | `ProjectTracking` (new) | `Supporting` | `pge_RfaConItm100_Set1000_0_ContentItem` | `Admin_ProjectTracking_Supporting_Set1000` |

## Layouts

Every page is linked to one of three content item dashboards, which decides its layout:

| Linked dashboard | Layout | Content sub items |
|---|---|---|
| `pge_RfaConItm100_Set1000_0_ContentItem` | One content sub item | 1 |
| `pge_RfaConItm101_Set1000_0_ContentItem` | Two, stacked vertically | 1 (top), 2 (bottom) |
| `pge_RfaConItm102_Set1000_0_ContentItem` | Two, side by side | 1 (left), 2 (right) |

Any content type can go in any content sub item.

On a two-item page the items share the space equally by default. To weight them, give each a **Size** on the design workbook's Pages sheet — its height on a stacked page, its width side by side. `2*` and `1*` give the first item two thirds, `Auto` fits an item to its content, and a number fixes it in pixels.

## Planning the pages

Claude proposes the solution's pages as part of the design, on the **Pages** sheet of the [design workbook](../integrations/aiModels/index.md#the-design-workbook): which pages, under which application group and navigation group, in what order, each one's layout, and what each part shows.

A typical solution has:

- a main work page under **Home** for each header table — master and detail when it has lines
- a summary or analysis page, if the solution reports
- a **Supporting** page under **Admin**, with the solution's lookup, reference and mapping lists in a drop-down
- a **Settings** page under **Admin**, when the solution has general settings of its own

Change the sheet like any other. Claude then generates `420-<Solution>-pages.json` from it and lists the pages for you to create.

## Step 1: Create the page in OneStream

The quickest way is to **copy an existing page and rename it**. Pick a page with the same layout, then copy each object below and replace that page's `<ApplicationGroup>_<NavigationGroup>_<Page>` with yours **everywhere it appears**: object names, parameter values, embedded dashboard names, and the button settings.

In the tables, `<App>_<Nav>_<Page>` is your three names — with the example above, `Home_ProjectTracking_Projects`.

**The page**, in its own maintenance unit:

| Object | Name |
|---|---|
| Maintenance unit | `Pview_<App>_<Nav>_<Page>` |
| Dashboard group, in it | `Page_<App>_<Nav>_<Page>` |
| Dashboard, in the group | `page_bag_<App>_<Nav>_<Page>`, layout **Uniform**, with one member: |
| Embedded dashboard component | `ebd_XLL_<App>_<Nav>_<Page>_xx<content item dashboard>`, embedding the layout's `pge_RfaConItm10x_Set1000_0_ContentItem` |
| Embedded dashboard component | `Embedded page_bag_<App>_<Nav>_<Page>` |

**The navigation entry**, in the application group's navigation maintenance unit, `Pnavs_<App>`:

| Object | Name |
|---|---|
| Parameter | `bin_Page_<App>_<Nav>_<Page>` |
| Buttons | `bti_Page_<App>_<Nav>_<Page>` and `btn_Page_<App>_<Nav>_<Page>`, with the page's caption as their text |
| Supplied parameter | `spp_bin_Page_<App>_<Nav>_<Page>` |
| Dashboards | `bin_Page_…` in `Template_ButtonIndicators_<App>`, `bpr_Page_…` in `Template_ButtonPairs_<App>`, and `bwr_Page_…` in `Template_ButtonWrappers_<App>` |
| Embedded dashboard components | `Embedded bin_Page_…`, `Embedded bpr_Page_…` and `Embedded bwr_Page_…` |
| Members of the navigation group's dashboards | Add `spp_bin_Page_…` to `container_bag_<App>_<Nav>`, and `Embedded bwr_Page_…` to `container_barPage_<App>_<Nav>` and `container_barNavCollapsed_<App>_<Nav>`, in the order the pages should appear |

A **new navigation group** is the dashboard group `Pnav_<App>_<Nav>` with its `container_*_<App>_<Nav>` dashboards, in `Pnavs_<App>`. Copy an existing navigation group the same way.

**Link the layout.** Select the page you just created, click **Link an Existing Content Item from a Shared Workspace** (the chain-link button), and pick the layout's dashboard from the [table above](#layouts).

**Check it.** Open the application group: the new button appears, and clicking it opens an empty page. It has no settings yet.

## Step 2: Configure what the page shows

### From the design workbook

With a Pages sheet, Claude generates the settings from it. In the [Extension Kit](extension-kit.md):

```
python .claude/skills/revfore-framework/scripts/build_pages.py solutions/<Solution>/200-<Solution>-spec.json
```

This writes and validates `420-<Solution>-pages.json` and prints the list of pages to create in OneStream.

### By describing the page

Without a Pages sheet, ask Claude for the page's settings. Give it the linked page reference, the layout, and what each content sub item should show — for example:

> *Home_Journals_Journals_Set1000, layout 101: item 1 the Journals grid, OneStream workflow on area Jm_Journals; item 2 the Controlled Itemized Area Scope cube view, Revfore workflow on the same area.*

Claude writes one row per page in `420-<Solution>-pages.json`:

```json
{
  "Data": [
    {
      "ViewId_IntegrationCode": "{RfaCore}.SolSettingValue_LinkedPage_mE",
      "Rows": [
        {
          "SolSetVal_IntegrationCode": "Home_Journals_Journals_Set1000",
          "SolSetVal_Name": "Home_Journals_Journals_Set1000",
          "SolSetVal_SettingValue": { "LnkPgeRef": "Home_Journals_Journals_Set1000", "InTrnstMd": true, "VwrCntxtSettings": { "1": { ... }, "2": { ... } } }
        }
      ]
    }
  ]
}
```

The settings are in **transit mode**: views, cube views and workflow areas are named, never given by id, so the same row applies in any application. The kit's `samples/GLEntry/420-GLEntry-pages.json` is a complete example.

### What a content sub item can show

| Content type | What it shows |
|---|---|
| Grids in a drop-down | One or more of the solution's, or the Framework's, grids — each with its name in the drop-down and what users may do in it: add, edit, delete, or read only |
| Cube views in a drop-down | One or more [cube views](../appGroups/admin/relational/supporting/cubeViews.md) |
| Single grid, or list | One view, and which of its actions show. Its columns — width, visibility, editability and header filters — are the view's own, set on the view rather than on the page |
| [Summary cards](../concepts/metadataDrivenUI/contentSubItem/summaryCard.md) | Totals from one view, normally a summary view: which columns are cards, with what summary (sum, min, max, count, average) and label, and which columns are filters |
| [Pivot grid](../concepts/metadataDrivenUI/contentSubItem/pivotGrid.md) | One view summarised: which columns are row fields, column fields and totalled data fields |
| [Adapter dashboard](../concepts/metadataDrivenUI/contentSubItem/adapterDashboard.md) | An adapter data set on one view, and which columns it carries, for an adapter-based component such as a BI Viewer |
| Any of them, under workflow | Revfore or OneStream workflow, on a workflow area |
| Master and detail | A list or grid in one content sub item; selecting a row refreshes the other — customers on the left, the selected customer's contracts on the right |

### Adapter dashboards

The page sets up only the data set. The component that shows it, you build in OneStream:

1. Create an extension dashboard with an adapter-based component on it, such as a BI Viewer.
2. Put it in a custom maintenance unit in the same **Revfore Framework** workspace: `XCP_xRfaConSubItm_AdptrDshbrds`, which has a sample adapter and dashboard, or a new one whose name starts `XCP_x`.
3. Give the component a **Custom Adapter** with the Method Query Claude gives you, for example:

    ```
    {Workspace.Current.rfa_shared_os.HelperQueries}{GetRelViewRecords}{LinkedPageRef = Home_RevRec_Analysis_Set1000, ContentSubItemNumber = 1, SolutionCode = Rfa, ModuleCode = SharedRel}
    ```

    The page shows the same text under **Setup**, in its adapter instructions.

### Capturing a page you configured by hand

Some settings, such as a static filter, are easier to set up on the page than to describe. Set the page up in OneStream, then capture it as data:

1. In Designer mode, open the page, click **Setup**, configure it and click **OK**.
2. Go to **Admin | Relational | Supporting | Linked Page Settings**, select your pages — only yours, as the Framework's own pages are in the same list — and click **Extract**.
3. The message, and one entry in the OneStream error log, ends with a block starting `/* As a pages data file ...`. That block is the same pages as rows for `420-<Solution>-pages.json`. Give it to Claude, or paste it into the file, as it is.

Extract reads every page back and compares it with what is saved. Check what it reports before you use the output:

| In the output | Meaning |
|---|---|
| `-- Transit round trip: every page reads back...`, at the top | All good |
| `-- Transit round trip: N difference(s)...`, under a page | That page doesn't come back as it was. Each line is `setting: saved -> read back` — usually a setting for a column the view no longer has. Re-save the page's Setup and extract again |
| `-- Not extracted:`, at the end | The page couldn't be written by name — for example, it lists a view that has been deleted. Fix the page and extract again |

## What checks the pages

The validator, which the setup package builder also runs, checks every page: its name, its content sub items and their parameter sets, the dashboards for each content type, that each grid's view exists in the solution's structure file, the drop-down order, and that workflow has an area.

On install, each page is checked again with the classes that load it. A page naming a view, cube view, column, filter or workflow area the application doesn't have is refused with the reason, rather than failing later when someone opens it.

## Installing and upgrading pages

The pages file runs as an **install-only** step in the setup package. The shipped pages are applied once, and a customer's later changes to a page survive Setup being pressed again.

A release that adds or changes pages ships them in a file of its own as well, so existing installs pick them up when they upgrade. If a release moves a page to a different layout, the administrator has to re-link it by hand after Setup. See [Pages in later releases](setup-packages.md#pages-in-later-releases).

## Notes

- Create the page in OneStream before its settings are loaded. Settings for a page that doesn't exist yet do nothing until it does.
- The page's objects are workspace objects, so they move with the workspace XML, not with the setup package.
- A page's columns are the view's. To change what a grid shows on every page, change the view; to change which grids a page offers, change the page.
