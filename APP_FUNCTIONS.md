# User guide

This guide describes what you can do in the investment portfolio app and how the main screens work. It is written for people using the app day to day, not for developers.

**Terminology:** The app uses **net capital** for the cash-flow balance of capital in versus capital out on a project (investments minus returns). Older materials or exports may still use the word “equity” for the same idea; in the app, labels and reports use **net capital**.

---

## Table of contents

1. [Home screen](#home-screen)
2. [Help](#help)
3. [Projects](#projects)
4. [Transactions and NAV](#transactions-and-nav)
5. [Categories](#categories)
6. [Currencies](#currencies)
7. [Alerts and alert rules](#alerts-and-alert-rules)
8. [Import and export](#import-and-export)
9. [Performance metrics and charts](#performance-metrics-and-charts)
10. [Insights](#insights)
11. [Reverse NAV calculator](#reverse-nav-calculator)
12. [Portfolio overview](#portfolio-overview)
13. [Print and share reports](#print-and-share-reports)
14. [Settings](#settings)
15. [App language](#app-language)
16. [Reset data](#reset-data)
17. [Tips and best practices](#tips-and-best-practices)
18. [Troubleshooting](#troubleshooting)
19. [Key metrics in plain language](#key-metrics-in-plain-language)

---

## Home screen

The Home screen is the main entry point. It lists your projects as cards (not a table).

### What you see on each project card

- **Name and currency** — Project name and its currency code (for example USD or EUR).
- **Totals** — Depending on your settings, you may see invested, returned, NAV, IRR, and other metrics. Amounts are shown in your **reference currency** when one is set, with an optional second line in the project currency when they differ.
- **Status and alerts** — Closed projects show an archive indicator. The app can show a “below target IRR” style indicator when relevant, and an alert severity icon when an alert rule applies to that project.

### Filters

Use the filter control to show **Active** (default), **Closed**, or **All** projects. Counts appear on the filters. The filter is remembered as you move around the app.

### Alerts

When alert rules are active, the **Alerts** action in the header can show a badge with the total number of alerts. Open **Alerts** to see which projects are affected and drill into a project’s transaction list. Alerts refresh when you return to the app and periodically in the background.

### Selection mode

Open the **filter** menu (filter icon in the header), then:

- **Select all** — Enters selection mode and selects every project currently visible (respecting Active / Closed / All).
- **Manual selection** — Enters selection mode without pre-selecting; tap cards to select or deselect.

With projects selected, the same menu can offer **View Summary**, **Export selected**, and **Print selected**. Use **Clear selection** to leave selection mode.

### Header shortcuts

- **Summary** — Opens charts for the current filter or for your selection in selection mode. A badge shows how many projects are included. Disabled if there are no projects (or no selection when in selection mode).
- **Portfolio** — Opens the portfolio overview (aggregated metrics and links to the projects table and portfolio-level Summary).
- **Alerts** — Opens the Alerts screen.

### Menu (three dots)

Typical items (exact order may vary by version):

- **Import data** — Import from a JSON file.
- **Export all** — Export all projects.
- **Print all** — Open print and share for all projects.
- **Manage categories**, **Manage currencies**, **Manage alert rules** — Maintain lists and rules.
- **Interface language** — Choose app language or follow the device.
- **Settings** — Metrics visibility, valuation options, exchange rates.
- **Help** — Searchable help, metric definitions, and insight topics.
- **Unlock limits** — (If shown) Upgrade flow for higher project limits on the free tier.
- **Reset data** — Delete projects and optionally restore defaults; may include loading sample data.

The **+** button creates a new project.

---

## Help

Open **Help** from the Home menu.

- **Search** — Find topics by title, keywords, or related words.
- **Articles** — Short guides (for example getting started, projects, transactions and NAV, import and export, language).
- **Metric definitions** — Opens the same metric help you get from **(i)** icons on cards and charts, with **Previous** / **Next** to move through metrics.
- **Project insight definitions** — Explains terms used on the Insights screen, with the same **Previous** / **Next** flow.

In-app metric help explains **net capital** as: total invested minus total returned from your cash-flow transactions (NAV is tracked separately and used in other metrics such as TVPI and RVPI).

---

## Projects

### Creating a project

1. On Home, tap **+**.
2. Fill in the form:
   - **Name** — Required, unique.
   - **Description** — Optional.
   - **Category** — Required; drives defaults and Insights context.
   - **Currency** — Required; **cannot be changed after creation**.
   - **Target IRR** — Optional; used for target-based metrics and comparisons.
   - **Planned hold (years)** — Optional; used on the Insights screen for time horizon. If you leave it empty, Insights may assume a synthetic hold and show a notice.
   - **Initial balance** — Optional; if set, creates an opening investment. **Project start date** is then required for that first investment.

Tap **Save** to create the project.

### Editing a project

From the transaction list, tap the project name in the header card to open the form.

- **Name** — Editable only if the project has no transactions yet.
- **Description**, **category**, **target IRR**, **planned hold** — Can be updated as needed.
- **Active** — When you turn a project **off** (closed), you must set a **closed date** after the latest transaction or NAV. Turning it **on** again clears the closed date.

Currency cannot be changed after creation.

### Deleting a project

From the project form (edit), use **Delete** and confirm. This removes the project and its transactions and NAV history. This cannot be undone.

### Closed projects

Closed projects are excluded from “active” portfolio views but can appear in “all” views and filters. Performance is evaluated through the close date rather than as an ongoing open investment.

---

## Transactions and NAV

### Adding a transaction

1. Open a project to its **transaction list**.
2. Tap **New transaction** or **+**.
3. Choose **Operation type**:
   - **Investment** — Capital into the project.
   - **Return** — Distribution or money out to you.
   - **NAV** — A valuation / NAV record (not a cash movement by itself).

Fill in **amount**, **date and time** (must be unique per project), **exchange rate** (filled from the project currency), and optional **notes**.

For **NAV** entries you also set **source** (official, manual, or system), **confidence**, and **how NAV is used in IRR** (reported, estimated, or transaction-adjusted).

**Cash transactions (investment or return)** also create an **automatic NAV** at the same timestamp so the valuation stays consistent with your cash flows. Those automatic NAVs are created with **system** source, **high** confidence, and **transaction-adjusted** IRR mode; you can edit them later if needed.

**Rules:** One transaction or NAV per timestamp per project. Dates cannot be after the project’s closed date if the project is closed. Future dates are allowed for planning where the app permits it.

### Editing or deleting

Use the edit control on a row to change or delete an entry. NAV rows cannot be turned into cash transactions. Deleting or changing cash flows may recalculate following automatic NAVs.

---

## Categories

Open **Manage categories** from the Home menu.

### Fields

- **Name** and **description**
- **Risk level** — Required (for example low through very high).
- **Investment strategy** — Required when creating a category; describes how the app interprets liquidity, time horizon, and similar ideas for **Insights** and strategy alignment. Built-in categories include sensible defaults.
- **Typical hold years** — Optional range for the category (minimum / maximum).
- **IRR range** — Optional minimum, base, and maximum expected IRR for the category.

Protected (built-in) categories cannot be edited or deleted; you can restore defaults from the category list. Categories in use may have restrictions on renaming or deletion.

### Order

Use the up/down controls to change display order (protected categories may stay fixed).

---

## Currencies

Open **Manage currencies** from the Home menu.

Each currency has a code, name, symbol, decimal places, and (for non-reference currencies) an **exchange rate** relative to the **reference currency**. The reference currency is fixed and always appears first; its rate is 1.

Currencies used by projects can only have their **exchange rate** edited (other fields are locked). Updated rates apply to converted figures across the app.

---

## Alerts and alert rules

### Viewing alerts

Use the **Alerts** action on Home or the portfolio quick link. You see projects with firing rules, severity, and (for value-based rules) how far values are from thresholds. Tap a project to open its transactions.

### Managing rules

**Manage alert rules** from the Home menu lets you create, edit, delete, export, import, restore built-in rules, or remove all rules (with confirmation).

**Time rules** fire when something has not happened within the last *N* days — for example last NAV update, last return, or last investment.

**Value rules** compare a metric to a threshold (or to a past period for percent change). Metrics include IRR, NPV, DPI, TVPI, RVPI, NAV, invested, returned, **net capital**, gap to target, and similar. Rules can apply to all projects or only to selected categories.

**Severity** and **display order** control how rules appear. Disabled rules are ignored.

---

## Import and export

### Export

- **Export all** from the Home menu, or **Export selected** after selecting projects in selection mode.
- Optionally set a **filename**; otherwise a dated default name is used.
- Share or save the JSON file as a backup or to move data to another device.

Exports include projects, transactions, NAV history, and related categories/currencies as needed.

### Import

Choose **Import data**, pick a JSON file, and resolve **conflicts** (same project name as an existing project): skip, overwrite, or rename. Imported categories and currencies are created if missing. Overwriting keeps the existing project currency — a mismatch with the file will fail with a clear error.

---

## Performance metrics and charts

### Transaction list (project metrics)

The project header area shows performance figures such as IRR, NPV, DPI, TVPI, RVPI, invested, returned, current NAV, and **net capital**, depending on settings. Use **Go to Summary** for charts or **Insights** for strategy context (see [Insights](#insights)).

### Summary (charts)

Open **Summary** from Home (current filter or selection). You get:

- **Line charts over time** — Metrics depend on **Settings → Metrics display → Line charts (Summary)** (and can mirror the project card selection).
- **Interval** — Month, quarter (trimester), or full detail; changing it refreshes the charts.
- **Comparison bars** — When two or more projects are included, horizontal bar charts can compare the latest values for metrics you enabled for comparison charts.

With a **single** project, a **Reverse NAV calculator** button may appear below the summary metrics (see [Reverse NAV calculator](#reverse-nav-calculator)).

### Projects table

From **Portfolio**, open **View projects table** (or the equivalent shortcut). The table lists all projects with sortable columns (name, dates, status, IRR, gap to target, NPV, ratios, invested, returned, target-based metrics, **net capital**, NAV, and others depending on settings).

Use **Refresh all metrics** to recompute everything (useful after import or when date-dependent figures should be brought up to date). The table shows when metrics were last updated.

**Print**, **Share HTML**, and **Share PDF** export the table for reporting.

---

## Insights

**Insights** (menu label on the transaction list) opens a focused view for **one project**: strategy context, key multiples, performance vs target, time horizon, and (for **open** projects) a **strategy alignment** snapshot.

**What you need**

- At least one **cash-flow transaction** (investment or return) for the main content.
- **Strategy alignment** is computed for **open** projects only; it is not shown for closed projects.

**Sections (typical)**

- **Strategy context** — Category, **investment strategy** profile, risk level, and short guidance on how to read metrics for that strategy.
- **Liquidity** — DPI, TVPI, RVPI with links to definitions.
- **Performance** — IRR, target IRR, gap to target.
- **Horizon** — Years since first investment, your **planned hold** (if set), and the category’s typical hold range when available.
- **Strategy alignment** — A prototype score (0–100) with a short band such as on track, mixed, or watch, plus breakdowns for liquidity, upside, and time vs the profile. This is **not** investment advice; it is a structured comparison to typical paths for the strategy profile.

**Banners**

- The feature may be labeled as a **prototype**; scoring may evolve.
- If you did not set a **planned hold**, the app may use a **synthetic** time horizon and explain that in a banner.

Info icons open the same help articles as **Help → Project insight definitions**.

---

## Reverse NAV calculator

Available on **Summary** when exactly **one** project is open. Tap **Reverse NAV calculator** to open a sheet.

You can work from:

- **Reported TVPI (multiple)** — Enter the multiple as a decimal (for example `1.18` for 1.18x).
- **Reported net IRR** — Enter the percentage as you would read it from a report (for example `9.5`).

The calculator shows how a **synthetic NAV** would compare to your **current NAV** in the reference currency and, when relevant, in the project currency. You can **save** the synthetic NAV as a new NAV record; saved entries are marked as manual, lower confidence, and estimated for IRR use, and the app returns you to the transaction list after saving.

Because exchange rates can differ across dates, the synthetic NAV versus current NAV can look different in reference currency than in project currency. That does not necessarily mean anything is wrong; it reflects mixed rates over time.

---

## Portfolio overview

**Portfolio** on Home shows three summary blocks: **all projects**, **active only**, and **closed only**. Each block shows counts, totals for invested and returned (in the reference currency), a **weighted target IRR** where applicable, **portfolio XIRR**, and optional metrics (NAV, **net capital**, target-based metrics, and others) according to **Settings → Portfolio screen**.

**Quick actions** often include:

- **Synchronize all data** — Recalculates stored metrics and related results for all projects.
- **View projects table** — Opens the sortable table.
- **View portfolio Summary** — Opens Summary with the full portfolio.
- **Alerts** — Opens the Alerts screen.

---

## Print and share reports

**Print all** (Home menu) or **Print selected** (selection mode) opens the print and share screen.

Options typically include:

- **Performance level** — Basic (core metrics) or complete (full set including ratios, **net capital**, target-based metrics, and more).
- **Transaction list** — Optional chronological list of cash flows and NAV rows; optional grouping by project.
- **Include alerts** — Optional summary of alert severities and details per project.

**Generate report**, then **Print**, **Share as PDF**, or **Share as HTML**. Content follows your current language and locale formatting.

---

## Settings

Open **Settings** from the Home menu. Tabs commonly include:

### Exchange rates

View and edit rates for non-reference currencies. Only positive rates are accepted.

### Metrics display

- **Valuation date** — Controls how the app picks the **end of the horizon** for **date-dependent** metrics. This applies to **single-project** views **and** to **portfolio-level** Summary and Portfolio charts (including when several projects are selected).  
  - **Latest transaction date** (default): For each **open** project, the horizon runs through the **later** of your **last cash-flow date** and your **latest NAV date**. For a **portfolio** view, the timeline extends through the **latest** of those effective dates across the projects included. If a project has no activity yet, the app falls back to **today** for that project. **Closed** projects are always taken only **through their close date**; this setting does not extend them past that.  
  - **Today's date**: **Open** projects are valued through **today** (the app still respects each project’s close date when a project is closed).
- **Show in project card** — Which optional metrics appear on Home cards (core metrics like invested, returned, IRR, and NAV usually stay visible).
- **Line charts (Summary)**, **Comparison bar charts (Summary)**, **Portfolio screen**, **Print and share** — Each section can **mirror the project card** or use its **own** checklist of metrics.

Changes apply after you leave the screen; some changes trigger a background refresh of stored metrics.

---

## App language

**Interface language** in the Home menu lets you follow the **system language** or choose a fixed language (for example English, Spanish, or Russian). Labels, messages, and number/date formatting follow your choice.

---

## Reset data

**Reset data** in the Home menu offers destructive actions:

- **Delete all projects** — Removes projects and all related transactions and NAVs; categories, currencies, and alert rules stay as they are unless you choose otherwise.
- **Delete projects and restore defaults** — Also restores built-in categories, currencies, and alert rules to their defaults (custom items may be removed).

Some versions also offer **Load sample data** from this area to explore the app with prefilled projects; you can delete sample data later with the same reset options.

**Warning:** Deletion cannot be undone. Export first if you need a backup.

---

## Tips and best practices

1. Set up **categories** (with investment strategy) and **currencies** before creating many projects.
2. Pick **project currency** carefully; it cannot be changed later.
3. Keep **NAV** reasonably current for meaningful TVPI, RVPI, and charts.
4. Export regularly for **backups**.
5. After **import** or a long absence, use **Refresh all metrics** on the projects table or **Synchronize all data** on Portfolio.
6. Use **selection mode** on Home to compare a subset of projects in Summary or in print/export.
7. Use **Help** and **(i)** icons whenever a metric is unclear — especially **net capital** versus NAV.
8. On **Insights**, set **planned hold** when you can so horizon and alignment are easier to interpret.

---

## Troubleshooting

| Issue | What to try |
| ----- | ----------- |
| Cannot delete a transaction or NAV | Some rows may be required for a consistent history; edit instead of delete if needed. |
| Cannot change project currency | Create a new project in the desired currency and move activity (or re-import). |
| Numbers look stale | Use **Refresh all metrics** or **Synchronize all data**; check **Settings** valuation date. |
| Import fails | Confirm the file is from this app’s export; on replace, currency must match the existing project. |
| Negative NAV not accepted | NAV must be zero or positive; adjust transactions or earlier NAVs if something looks inconsistent. |

---

## Key metrics in plain language

- **IRR (XIRR)** — Annualized return that fits your dated cash flows; money-weighted.
- **NPV (xNPV)** — Present value of cash flows at a discount rate (often related to target IRR).
- **DPI** — Returned divided by invested (what you got back per unit of capital called).
- **TVPI** — (Returned + NAV) divided by invested (total value per unit called).
- **RVPI** — NAV divided by invested (unrealized per unit called).
- **Net capital** — Invested minus returned on a **cash-flow basis** only; NAV is separate and is not added into net capital. Negative net capital can mean you have taken more out than you put in; NAV still captures remaining value.
- **Target value / required return / accumulated value** — Target-based views: where invested capital would stand at the target rate, how much more return is needed to hit the target IRR, and how historical cash flows compound at the target rate to the valuation date. Each project uses its own target IRR; portfolio figures combine projects using the same **valuation date** setting as in **Settings → Metrics display**.

For full definitions and “why this number might look different,” use **Help → Metric definitions** or the **(i)** icons in the app.
