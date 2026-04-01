# App Functions Guide

This document provides a comprehensive guide to all functions available in the investment portfolio management app.

## Table of Contents

1. [Home Screen](#home-screen)
2. [Projects Management](#projects-management)
3. [Transactions Management](#transactions-management)
4. [NAV Records Management](#nav-records-management)
5. [Categories Management](#categories-management)
6. [Currencies Management](#currencies-management)
7. [Alerts and Alert Rules](#alerts-and-alert-rules)
8. [Import and Export](#import-and-export)
9. [Performance Metrics and Analytics](#performance-metrics-and-analytics)
10. [Reverse NAV Calculator](#reverse-nav-calculator)
11. [Portfolio Overview](#portfolio-overview)
12. [Print and Share Reports](#print-and-share-reports)
13. [Settings (Configuration)](#settings-configuration)
14. [App Language (Locale)](#app-language-locale)

---

## Home Screen

The Home screen is the main entry point of the application and provides a quick overview of all projects.

### Home Screen Features

1. **Projects List**:
   - Displays all projects as cards (not a table)
   - Each project card shows:
     - **Project Name and Currency**: Name of the project and its currency code (e.g. `USD`, `EUR`)
     - **Total Invested**: Total invested amount. When a reference currency is configured, the main value is shown in the reference currency, with an optional second line in the project currency when they differ.
     - **Total Returned**: Total returned amount, with the same reference-currency-first display pattern as Invested.
     - **NAV**: Current Net Asset Value, shown in reference currency when available, with an optional second line in the project currency.
     - **Status & Alerts**: A small archive icon for closed projects, a "Below target IRR" badge when performance is below the configured target IRR, and (when applicable) an alert severity icon representing the worst active alert for that project.
   - Supports scrolling for long lists
   - Each project card is clickable and navigates to the Transaction List screen for that project (unless selection mode is active)

2. **Project Filtering**:
   - Filter buttons at the top allow switching between:
     - **Active**: Show only active projects (default)
     - **Closed**: Show only closed projects
     - **All**: Show all projects (both active and closed)
   - Filter buttons show counts for each category
   - The filter state persists while navigating within the app

3. **Alerts Overview**:
   - When any alert rules are firing for your projects, an alerts badge appears on the **Alerts** action in the Home screen AppBar, showing the total number of active alerts across all projects (0–99+).
   - Individual project cards can also show an alert severity icon (Critical, High, Medium, Low, Info) for the worst alert that currently applies to that project.
   - Tap the **Alerts** action in the AppBar to open the **Alerts** screen, where you can see which projects have alerts, review rule details, and tap through to a project's transaction list.
   - Alerts are re-evaluated when the app comes to the foreground and periodically in the background.

4. **Project Selection Mode**:
   - Selection mode is activated **from the projects filter menu** (filter icon in the header).
   - Open the filter menu (filter icon), then choose one of:
     - **Select All**: Enters selection mode and selects all projects currently visible. To select only active projects: set filter to "Show Active" first, then "Select All". To select only closed: set filter to "Show Closed", then "Select All". To select everything: set filter to "Show All", then "Select All".
     - **Manual Selection**: Enters selection mode without pre-selecting; then tap project cards to select or deselect individual projects.
   - In selection mode, tap project cards to select/deselect them. Use the filter menu again for "View Summary", "Export Selected", or "Print Selected" (when you have at least one project selected), or "Clear Selection" to exit selection mode.
   - Selected projects can be:
     - Exported together (filter menu → "Export Selected")
     - Viewed in Summary/Charts screen together (filter menu → "View Summary")
     - Printed or shared as reports (filter menu → "Print Selected")
   - Selection mode is cleared when you clear all selections or choose "Clear Selection" from the filter menu.

5. **Navigation Actions (AppBar)**:
   - The AppBar displays navigation actions with icons and text labels below them.
   - Actions are evenly distributed horizontally across the header.
   - Available actions:
     - **Summary**: Navigate to the summary/charts screen. In selection mode: shows the selected projects. Without selection mode: shows the currently visible (filtered) projects — i.e. active, closed, or all, depending on the filter. A small badge shows how many projects will be included.
     - **Portfolio**: Navigate to the portfolio overview screen with aggregated metrics and quick links to the Projects Table and portfolio-level Summary.
     - **Alerts**: Navigate to the Alerts screen. A badge on this action shows the total number of active alerts.
   - All actions are disabled when there are no projects in the database.

6. **Navigation Header Actions**:
   - **Create Project**: Add button (+) to create a new project
   - **Menu** (three dots):
     - **Import Data**: Import projects or combined data from a JSON file
     - **Export All**: Export all projects to a JSON file
     - **Print All**: Open the Print and Share Reports screen for all projects
     - **Manage Categories**: Access category management
     - **Manage Currencies**: Access currency management
     - **Manage Alert Rules**: Access alert rule management (create, edit, restore built-in rules, export/import rules)
     - **Interface Language**: Change the app language (locale)
     - **Settings**: Configure which metrics are shown on project cards, Summary charts, Portfolio screen, and Print & share reports, and adjust valuation date and exchange-rate related settings
     - **Upgrade**: (Free version only) Open the upgrade screen to unlock higher project limits
     - **Reset Data**: Reset data (delete all projects and/or restore categories, currencies, and alert rules to defaults)

### Viewing Projects on Home Screen

1. When the app starts, you'll see the Home screen with active projects
2. Projects are displayed as cards with essential information
3. Use filter buttons to switch between Active, Closed, or All projects
4. Tap any project card to view its detailed transaction list
5. Use the filter menu to enter selection mode and select multiple projects for bulk operations
6. Use the AppBar actions to navigate to other major screens
7. Use the navigation header menu for data import and management functions

**Note**: The project list automatically refreshes when projects are created, updated, or deleted elsewhere in the app.

---

## Projects Management

### Creating a Project

1. Navigate to the **Home** screen (entry point)
2. Tap the **"+"** button in the navigation header to create a new project
3. Fill in the project form:
   - **Name**: Unique project name (required)
   - **Description**: Optional project description
   - **Category**: Select a category from the list (required)
   - **Currency**: Select the currency for this project (required, cannot be changed after creation)
   - **Target IRR**: Set the target Internal Rate of Return as a percentage (optional)
   - **Initial Balance**: Optionally set an initial balance, which will automatically create an investment transaction. If you provide an initial balance, the **Project Start Date** field appears and is required — set the date for that automatic investment transaction
4. Tap **"Save"** to create the project

**Note**: The project currency is immutable after creation. Choose carefully as it cannot be changed later.

### Updating a Project

1. Navigate to the **Home** screen and find the project you want to edit
2. Tap on the project card to open the transaction list
3. On the transaction list screen, tap the project name in the project data card at the top to open the project form (edit mode)
4. Modify any of the following fields:
   - **Name**: Can be changed if the project has no transactions
   - **Description**: Can always be updated
   - **Category**: Can always be changed
   - **Target IRR**: Can always be updated
   - **Active**: Switch shown in edit mode only. **ON** = active project (default after creation). **OFF** = closed project. When you toggle to **OFF**, the **Closed Date** input appears and is mandatory — set the date when the project was closed (must be later than the latest transaction or NAV record date). To reactivate a closed project, toggle **Active** back to **ON**; the Closed Date is then cleared automatically.
5. Tap **"Save"** to update the project

**Note**: Currency cannot be changed after project creation.

### Deleting a Project

1. Open the project you want to delete (tap its card to open the transaction list)
2. Tap the project name in the project data card to open the project form (edit mode)
3. Scroll to the bottom and tap the **"Delete"** button
4. Confirm the deletion in the alert dialog
5. If the project has transactions, you'll see a warning message indicating how many transactions will be deleted

**Warning**: Deleting a project will also delete all associated transactions and NAV records. This action cannot be undone.

### Closing a Project

1. Open the project you want to close (tap its card to open the transaction list)
2. Tap the project name in the project data card to open the project form (edit mode)
3. Toggle the **"Active"** switch to **OFF**
4. The **"Closed Date"** input appears and is mandatory — set the date when the project was closed.
5. Tap **"Save"**

**Note**: The closed date must be later than the date of the latest transaction or NAV record in the project.

### Reopening a Project

1. Open the closed project you want to reopen (tap its card to open the transaction list)
2. Tap the project name in the project data card to open the project form (edit mode)
3. Toggle the **"Active"** switch to **ON**
4. Tap **"Save"** — the Closed Date is cleared automatically when the project is reactivated

**Note**: Closed projects are excluded from active portfolio calculations. IRR calculations for closed projects use the project close date instead of current NAV.

---

## Transactions Management

### Adding a Transaction

1. Open a project from the Portfolio or Project List
2. Navigate to the **Transactions** list for that project
3. Tap the **"New Transaction"** button or use the "+" button
4. Fill in the transaction form:
   - **Operation Type**: Select one of three types:
     - **Investment**: Money going into the project (negative cash flow)
     - **Return**: Money coming out of the project (positive cash flow)
     - **NAV**: Explicit NAV (Net Asset Value) record (see [NAV Records Management](#nav-records-management) for details)
   - **Amount**: Enter the amount (positive value, must be non-negative for NAV)
   - **Date and Time**: Set when the transaction/NAV occurred (required, must be unique per project). Future dates are allowed for planning purposes
   - **Exchange Rate**: The exchange rate at the time (automatically set based on project currency)
   - **Description**: Optional description
   - **For NAV records only**: Additional fields are available:
     - **Source**: Select source type — **Official** (statement from fund/company), **Manual** (user-entered value), or **System** (auto-generated; used for NAVs created from cash transactions)
     - **Confidence**: Select confidence level (High, Medium, or Low)
     - **XIRR Mode**: Select how NAV should be used in XIRR calculations — **Reported** (official NAV), **Estimated** (rough user guess), or **Transaction Adjusted** (auto-updated after a cash transaction)
5. Tap **"Save"** to create the transaction or NAV record

**Note**: 
- Each project can only have one transaction/NAV per date and time (no duplicates)
- When "Investment" or "Return" is selected, a cash transaction is created and an automatic NAV record is generated with default values (Low confidence, Unknown source, Not Relevant XIRR mode)
- When "NAV" is selected, only an explicit (manual) NAV record is created without a cash transaction
- Transaction and NAV dates can be set to future dates, but cannot be later than the project's closed date (if the project is closed)

### Updating a Transaction

1. Open the project containing the transaction
2. Navigate to the **Transactions** list
3. Tap the pencil button (edit icon) on the transaction card you want to edit
4. Modify any of the transaction fields:
   - **Operation Type**: 
     - For Investment/Return transactions: Can toggle between Investment and Return types
     - For NAV records: Cannot be changed (NAV records remain as NAV and cannot be converted to Investment or Return)
   - **Amount**: Can be updated for all types
   - **Date and Time**: Can be changed for all types (must remain unique per project)
   - **Exchange Rate**: Can be updated
   - **Description**: Can be updated
   - **For NAV records only**: Can also update Source, Confidence, and XIRR Mode
5. Tap **"Save"** to update the transaction or NAV record

**Note**: When a transaction is updated, the associated NAV record and successive automatic NAV records are automatically recalculated to maintain consistency.

### Deleting a Transaction

1. Tap the pencil button on the transaction card you want to delete (to open the transaction form)
2. Scroll to the bottom of the transaction form
3. Tap the **"Delete"** button
4. Confirm the deletion in the alert dialog

**Warning**: Deleting a transaction will also delete the associated NAV record and recalculate successive NAV records. This action cannot be undone.

**Note**: Some transactions may not be deletable if they are required for maintaining data integrity.

---

## NAV Records Management

### Adding a NAV Record

NAV (Net Asset Value) records can be added in two ways:

#### Method 1: Automatically via Cash Transaction
- When you create an "Investment" or "Return" transaction, an automatic NAV record is created alongside it
- No manual action required - the NAV record is generated automatically
- Automatic NAV records are created with default values:
  - **Source**: System
  - **Confidence**: High
  - **XIRR Mode**: Transaction Adjusted
- The automatic NAV amount is calculated based on the transaction and previous NAV records

#### Method 2: Manually (Explicit NAV Record)
1. Open a project
2. Navigate to the **Transactions** list
3. Tap the **"New Transaction"** button or use the "+" button
4. In the transaction form, select **"NAV"** as the Operation Type
5. Fill in the NAV form:
   - **Amount**: Enter the NAV amount (must be non-negative)
   - **Date and Time**: Set when the NAV was recorded (required, must be unique per project). Future dates are allowed for planning purposes
   - **Exchange Rate**: The exchange rate at the NAV date (automatically set based on project currency)
   - **Source**: Select the source type:
     - **Official**: Official valuation (e.g. statement from fund/company)
     - **Manual**: User-entered or estimated value
     - **System**: Reserved for auto-generated NAVs (e.g. after a cash transaction)
   - **Confidence**: Select confidence level:
     - **High**: High confidence in the valuation
     - **Medium**: Medium confidence
     - **Low**: Low confidence
   - **XIRR Mode**: Select how this NAV should be used in XIRR calculations:
     - **Reported**: Official NAV from statements
     - **Estimated**: Rough or estimated valuation
     - **Transaction Adjusted**: Auto-updated when cash transactions are added or changed
   - **Description**: Optional description
6. Tap **"Save"** to create the NAV record

**Important**: 
- NAV values are never allowed to be negative. The system enforces this at all points where NAV values are set.
- NAV records are created using the same transaction form as cash transactions - there is no separate "New NAV" button.

### Updating a NAV Record

1. Open the project containing the NAV record
2. Navigate to the **Transactions** list (NAV records are shown alongside transactions)
3. Tap the pencil button (edit icon) on the NAV record card you want to edit
4. Modify any of the NAV fields:
   - **Amount**: Can be updated
   - **Date and Time**: Can be changed (must remain unique per project)
   - **Exchange Rate**: Can be updated
   - **Source**: Can be updated (Official, Manual, or System)
   - **Confidence**: Can be updated (High, Medium, or Low)
   - **XIRR Mode**: Can be updated (Reported, Estimated, or Transaction Adjusted)
   - **Description**: Can be updated
   - **Operation Type**: Cannot be changed (NAV records cannot be converted to Investment or Return)
5. Tap **"Save"** to update the NAV record

**Note**: 
- When a NAV record is updated, successive automatic NAV records are recalculated to maintain consistency.
- NAV records cannot be converted to cash transactions (Investment or Return) - they remain as NAV records.

### Deleting a NAV Record

1. Tap the pencil button on the NAV record card you want to delete (to open the NAV form)
2. Scroll to the bottom of the NAV form
3. Tap the **"Delete"** button
4. Confirm the deletion in the alert dialog

**Warning**: Deleting a NAV record will cause successive automatic NAV records to be recalculated. This action cannot be undone.

---

## Categories Management

### Viewing Categories

1. From the **Home** screen, tap the menu button (three dots) in the header
2. Select **"Manage Categories"**
3. You'll see a list of all categories sorted by their display order

### Creating a Category

1. Navigate to the **Category List** screen
2. Tap the **"New Category"** button or use the "+" button in the header
3. Fill in the category form:
   - **Name**: Unique category name (required, minimum 2 characters)
   - **Description**: Optional category description
   - **Risk Level**: Select from predefined levels (required):
     - Low
     - Medium
     - Medium-High
     - High
     - Very High
   - **Typical Hold Years**: Optional minimum and maximum typical holding periods
   - **IRR Range**: Optional expected IRR values:
     - **Minimum IRR**: Lower bound of expected return
     - **Base IRR**: Typical/base expected return
     - **Maximum IRR**: Upper bound (must be >= minimum and base)
4. Tap **"Save"** to create the category

### Updating a Category

1. Navigate to the **Category List** screen
2. Tap on the category you want to edit
3. Modify the category fields
4. Tap **"Save"** to update

**Restrictions**:
- Protected categories (built-in categories) cannot be edited
- Category name cannot be changed if the category has associated projects

### Deleting a Category

1. Navigate to the **Category List** screen
2. Open the category you want to delete
3. Scroll to the bottom and tap the **"Delete"** button
4. Confirm the deletion

**Restrictions**:
- Protected categories cannot be deleted
- Categories with associated projects cannot be deleted (button will be disabled with an explanatory message)

### Reordering Categories

1. Navigate to the **Category List** screen
2. Use the up/down arrow buttons on each category card to change the display order
3. Changes are saved automatically

**Note**: Protected categories cannot be reordered and remain locked in position.

### Restoring Built-in Categories

1. Navigate to the **Category List** screen
2. Tap the restore button (tag icon) in the header
3. Built-in categories will be restored to their default values
4. User-defined categories are automatically reordered to appear after built-in categories

---

## Currencies Management

### Viewing Currencies

1. From the **Home** screen, tap the menu button (three dots) in the header
2. Select **"Manage Currencies"**
3. You'll see a list of all currencies sorted by their display order

### Creating a Currency

1. Navigate to the **Currency List** screen
2. Tap the **"New Currency"** button or use the "+" button in the header
3. Fill in the currency form:
   - **ID (Currency Code)**: Unique currency identifier (e.g., 'USD', 'EUR') - required
   - **Name**: Full currency name (required)
   - **Symbol**: Currency symbol (e.g., '$', '€') - required
   - **Decimal Digits**: Number of decimal places for currency display (0-10, integer) - required
   - **Exchange Rate**: Exchange rate relative to the reference currency - required (must be > 0)
4. Tap **"Save"** to create the currency

### Updating a Currency

1. Navigate to the **Currency List** screen
2. Tap on the currency you want to edit
3. Modify the currency fields
4. Tap **"Save"** to update

**Restrictions**:
- Reference currency is read-only and cannot be edited
- If the currency has associated projects:
  - Only the exchange rate can be updated
  - ID, name, symbol, and decimal digits cannot be changed

### Deleting a Currency

1. Navigate to the **Currency List** screen
2. Open the currency you want to delete
3. Scroll to the bottom and tap the **"Delete"** button
4. Confirm the deletion

**Restrictions**:
- Reference currency cannot be deleted
- Currencies with associated projects cannot be deleted (button will be disabled with an explanatory message)

### Reordering Currencies

1. Navigate to the **Currency List** screen
2. Use the up/down arrow buttons on each currency card to change the display order
3. Changes are saved automatically

**Note**: Reference currency cannot be reordered and always appears first in the list.

### Restoring Built-in Currencies

1. Navigate to the **Currency List** screen
2. Tap the restore button (cash icon) in the header
3. Built-in currencies will be restored to their default values (preserving current exchange rates)
4. User-defined currencies are automatically reordered to appear after built-in currencies

---

## Alerts and Alert Rules

The app can evaluate **alert rules** against your projects and surface **alerts** when conditions are met (e.g. no NAV update for 3 months, IRR below target, NAV drop over a period). You manage rules from **Manage Alert Rules** and view current alerts from the **Alerts** screen or the alerts widget on Home.

### Viewing Alerts

1. **From the Home screen**: When any rules are firing, an **Alerts** action in the AppBar shows a numeric badge with the total number of active alerts. Tap it to open the Alerts screen.
2. **Alerts screen**: Lists projects that have at least one firing alert. For each project you see the rule name, severity icon and color, and (for value rules) the difference from the threshold, formatted in currency, percent, or ratio as appropriate. Tap a project to open its transaction list.
3. **Project cards on Home**: Each project card can show an alert severity icon for the worst active alert that applies to that project.

Alerts are re-evaluated when the app returns to the foreground and periodically in the background.

### Managing Alert Rules

1. From the **Home** screen, tap the menu (three dots) and select **"Manage Alert Rules"**.
2. The **Alert Rule List** shows all rules (built-in and custom), sorted by display order then name.
3. **Create**: Tap the "+" button to add a new rule. Choose **Time** or **Value** type and fill in the form.
4. **Edit**: Tap a rule to open the form. Update name, code, severity, enabled state, display order, and (depending on kind) time or value parameters and category scope.
5. **Delete**: Open a rule and use the Delete button at the bottom (with confirmation).
6. **Export alert rules**: Use the menu in the Alert Rule List to export all rules to a JSON file. You can share or backup the file and import it on another device or later.
7. **Import alert rules**: Use the menu in the Alert Rule List to select a JSON file that contains alert rules (exported from this app). Missing rules are added; category links are preserved for existing categories. You can also import alert rules via **Import Data** from the Home menu — if the selected file contains alert rules (dataType is detected), they are imported along with any projects.
8. **Restore built-in rules**: Use the restore button in the header to re-seed built-in alert rules. Existing built-in rules (matched by code) are updated; new ones are added. User-defined rules are kept.
9. **Remove all rules**: When the list is not empty, a "remove all" (delete-sweep) button appears. Use it to delete every rule (with confirmation). You can then restore built-in rules if needed.

### Alert Rule Types

- **Time rules**: Fire when a project has not had a certain **event** within the last **N** days. Events:
  - **Latest NAV update**: Last time a NAV record was added or updated
  - **Latest return**: Last return (distribution) transaction
  - **Latest investment**: Last investment (capital call) transaction
  You set **Time window (days)** (e.g. 90 for 3 months).

- **Value rules**: Fire when a **metric** compared to a **threshold** (and optional reference) is true. Examples:
  - **Metric**: Cached IRR, XNPV, DPI, TVPI, RVPI, NAV, Invested, Returned, Equity, or Gap to Target
  - **Comparison**: Less than, less or equal, greater than, greater or equal, equal, or percent drop/growth vs a past period
  - **Reference**: Either a **fixed value** or **period in past** (e.g. compare current NAV to value 14 days ago for percent drop)

Rules can be **scoped** to all projects (global) or to selected categories; when scoped to categories, only projects in those categories are evaluated.

### Severity and Display

- Each rule has an optional **severity**: CRITICAL, HIGH, MEDIUM, LOW, or INFO. This affects how alerts are grouped and displayed (e.g. in the widget and on the Alerts screen).
- **Display order** controls the sort order of rules in the list; lower numbers appear first.
- **Enabled**: Disabled rules are not evaluated and do not produce alerts.

### Built-in Alert Rules

The app ships with several built-in rules (e.g. "No NAV updates for 3 months", "No NAV updates for 6 months", "NAV drop by > 5% in 14 days", "IRR - gap to target < 0"). You can enable/disable them, edit them (e.g. time window or threshold), or restore them to default definitions via **Manage Alert Rules** → restore button.

---

## Import and Export

### Exporting Projects

You can export projects in two ways:

#### Method 1: Export All Projects
1. Navigate to the **Home** screen
2. Tap the menu button (three dots) in the header
3. Select **"Export All"** to export all projects
4. The Export Projects screen will open with all project names listed
5. Optionally enter a **custom filename** for the export file (if left empty, a default filename with timestamp will be used)
6. Tap **"Export"** to generate a JSON file
7. Use the system share sheet to save or share the export file

#### Method 2: Export Selected Projects from Home Screen
1. Navigate to the **Home** screen
2. Open the filter menu and use "Select All" or "Manual Selection" to enter selection mode
3. Select one or more projects you want to export
4. Tap the **"Export"** button (or access via the filter menu)
5. The Export Projects screen will open with the selected project names listed
6. Optionally enter a **custom filename** for the export file
7. Tap **"Export"** to generate a JSON file
8. Use the system share sheet to save or share the export file

**Export File Contents**:
- All selected project data
- All associated transactions for selected projects
- All NAV records for selected projects
- Related currencies and categories (if not already in the target database)

**Notes**: 
- Export files include metadata (version, export date) for compatibility tracking
- You can specify a custom filename when exporting. If no custom filename is provided, a default filename with timestamp is automatically generated (format: `projects_export_YYYYMMDD_HHMMSS.json`)
- The export file is a JSON file that can be imported into another device or used as a backup

### Importing Projects

1. Navigate to the **Home** screen
2. Tap the menu button (three dots) in the header
3. Select **"Import Data"** or navigate to the Import screen
4. Select a JSON export file from your device
5. The app will analyze the file and identify any conflicts:
   - **Conflicting Projects**: Projects with the same name as existing projects
   - **Non-Conflicting Projects**: New projects that don't conflict
6. For each conflict, choose an action:
   - **Skip**: Don't import this project
   - **Overwrite**: Replace the existing project with the imported one
   - **Rename**: Import with a new name (you'll be prompted to enter a new name)
7. Tap **"Import"** to complete the import

**Important Notes**:
- Missing categories are automatically created from export data
- Missing currencies are automatically created for new project imports
- When replacing an existing project, the currency cannot be changed. If the imported project has a different currency, the import will fail with an error
- All project relationships (transactions, NAV history) are preserved in the import

---

## Performance Metrics and Analytics

### Viewing Portfolio Performance

1. Navigate to the **Portfolio** screen by tapping the "Portfolio" button in the AppBar on the Home screen
2. The screen displays three summary cards:
   - **All Projects Summary**: Aggregated metrics for all projects (active and closed combined)
   - **Active Projects Summary**: Metrics for active projects only
   - **Closed Projects Summary**: Metrics for closed projects only
3. Each summary card shows:
   - Project count
   - Total Invested (in reference currency)
   - Total Returned (in reference currency)
   - Weighted Target IRR (investment-weighted average of project target IRRs)
   - Portfolio XIRR (Internal Rate of Return for the portfolio)
4. Use quick action buttons to:
   - Navigate to individual projects (Home screen)
   - View detailed metrics table (Projects Table screen)
   - Refresh all metrics to get up-to-date calculations

**Note**: The weighted target IRR is calculated as an investment-weighted average of individual project target IRRs. Only projects with positive invested amounts and defined target IRRs are included in this calculation.

### Viewing Project Performance

1. Open a project from the project List (Home screen)
2. The project detail screen shows:
   - **IRR**: Internal Rate of Return (cached calculated value)
   - **XNPV**: Net Present Value in reference currency
   - **DPI**: Distribution to Paid-In ratio
   - **TVPI**: Total Value to Paid-In ratio
   - **RVPI**: Residual Value to Paid-In ratio
   - **Total Invested**: Sum of all investment transactions
   - **Total Returned**: Sum of all return transactions
   - **Current NAV**: Most recent Net Asset Value
   - **Equity**: Current equity value

### Viewing Performance Charts

1. Select one or more projects from the Home screen
2. Navigate to the **Summary** screen (charts view) by tapping the "Summary" button
3. The screen displays two types of visualizations:

#### Time-Series Line Charts

Interactive line charts showing metrics over time. The exact set of metrics depends on your **Settings → Metrics display configuration** for *Line charts (Summary)*, but can include, for example:
- **XIRR**: Internal Rate of Return over time
- **xNPV**: Net Present Value over time
- **NAV**: Net Asset Value history
- **DPI**: Distribution to Paid-In ratio over time
- **TVPI**: Total Value to Paid-In ratio over time
- **RVPI**: Residual Value to Paid-In ratio over time
- **Invested**: Cumulative invested amount
- **Returned**: Cumulative returned amount
- **Equity**: Equity value over time
- **Target IRR**: Target Internal Rate of Return
- **Target Value**: Theoretical value of invested capital if all investments grew at the project's target IRR
- **Required Return**: Additional return (in currency) needed to reach the target IRR from the current state
- **Accumulated Value**: All historical cash flows compounded to the valuation date at the target IRR

#### Interval Selection

- Choose time interval for data aggregation:
  - **Month**: Monthly data points
  - **Trimester**: Quarterly data points (default for multiple projects)
  - **Full**: All available data points
- Changing the interval recalculates and reloads all charts

#### Project Comparison Bar Charts (Multiple Projects Only)

When 2 or more projects are selected, the Summary screen also displays horizontal bar charts comparing the latest metrics across projects. The exact metrics depend on your **Settings → Metrics display configuration** for *Comparison bar charts (Summary)*, but can include:
- **XIRR**: Compare Internal Rate of Return across projects
- **DPI / TVPI / RVPI**: Compare key performance ratios
- **NAV / Invested / Returned / Equity**: Compare currency-based metrics in the reference currency
- **Target Value / Required Return / Accumulated Value / Gap to Target**: Compare target-based and performance-gap metrics
- Each bar chart shows project names on the Y-axis and metric values on the X-axis
- Helps identify top and bottom performers in the portfolio

**Notes**: 
- Time-series charts are aggregated when multiple projects are selected, showing portfolio-level metrics over time
- Comparison bar charts only appear when viewing 2 or more projects together
- For single project view, only time-series charts are displayed

### Project Table View

1. Navigate to the **Home** screen
2. Open the **Portfolio** screen from the Home AppBar, then tap **"View Projects Table"**; you can also navigate directly if a shortcut is configured
3. The table displays all projects with sortable columns:
   - **Project Name**: Sortable alphabetically
   - **Start Date**: Date of first transaction
   - **Status**: Shows "Active" or "Closed" status
   - **Target IRR**: Target Internal Rate of Return
   - **IRR**: Calculated Internal Rate of Return (XIRR)
   - **Gap to Target**: Difference between actual IRR and target IRR
   - **NPV (xNPV)**: Net Present Value in reference currency
   - **DPI**: Distribution to Paid-In ratio
   - **TVPI**: Total Value to Paid-In ratio
   - **RVPI**: Residual Value to Paid-In ratio
   - **Invested**: Total invested amount in reference currency
   - **Returned**: Total returned amount in reference currency
   - **Target Value**: Theoretical value of invested capital if all investments grew at each project's target IRR
   - **Required Return**: Additional return (in currency) needed to reach each project's target IRR from the current state
   - **Accumulated Value**: All historical cash flows compounded to the valuation date at the target IRR
   - **Equity**: Current equity value in reference currency
   - **NAV**: Current Net Asset Value in reference currency
   - A separate note below the table shows **"Metrics updated for"** with the earliest timestamp among cached metrics, indicating how fresh the numbers are

**Features**:
- Click any column header to sort (ascending/descending)
- Horizontal and vertical scrolling supported
- All numeric values use locale-specific formatting and reference currency symbols
- Sort preferences (column and direction) are remembered between sessions
- Export table data:
  - **Print**: Generate and print HTML version of the table
  - **Share HTML**: Share table as HTML document
  - **Share PDF**: Share table as PDF document

### Refreshing Metrics

1. Navigate to the **Projects Table** screen
2. Tap the **"Refresh All Metrics"** button
3. A progress indicator shows:
   - Current project being processed
   - Progress text (e.g., "Processing ProjectName (3 of 10)")
   - Visual progress bar
4. After completion, the table automatically reloads with updated metrics

**Note**: Metrics are recalculated automatically after making changes to projects or transactions. Use **Refresh All Metrics** when you return to the application after some time, as there are metrics that depend on the current date. Also recalculate metrics after data import.

### Exporting and Sharing Project Table

The Projects Table screen provides three export/share options accessible via buttons in the header:

1. **Print**:
   - Generates an HTML version of the table
   - Opens the system print dialog
   - Allows printing to paper or saving as PDF (depending on device capabilities)
   - Table includes all visible columns with formatted values

2. **Share HTML**:
   - Generates an HTML document containing the table
   - Opens the system share sheet
   - Can be shared via email, messaging apps, cloud storage, etc.
   - HTML file is standalone and viewable in any web browser

3. **Share PDF**:
   - Generates a PDF document containing the table
   - Opens the system share sheet
   - Professional formatted PDF suitable for reports and presentations
   - Can be shared via email, messaging apps, cloud storage, etc.

**Features of Exported Tables**:
- All currency values are formatted with appropriate symbols and decimal places
- Percentage values (IRR, Target IRR, Gap to Target) are properly formatted
- Date values use locale-specific formatting
- Table headers and data are styled for readability
- Reference currency is clearly indicated for applicable columns

---

## Portfolio Overview

### Portfolio Screen Features

The Portfolio screen provides a high-level overview of your entire portfolio with aggregated metrics for active and closed projects. Access it via the AppBar "Portfolio" button from the Home screen.

1. **Portfolio Summary Cards**:
   - **All Projects Summary**: Aggregated metrics across all projects. By default this includes:
     - Total count of all projects
     - Total Invested (in reference currency)
     - Total Returned (in reference currency)
     - Weighted Target IRR (investment-weighted average target IRR)
     - Portfolio XIRR (Internal Rate of Return)
     - Optional additional metrics such as NAV, Equity, Target Value, Required Return, Accumulated Value, and Gap to Target, depending on your **Settings → Metrics display configuration → Portfolio screen**.
   - **Active Projects Summary**: Same metrics, but for active projects only.
   - **Closed Projects Summary**: Same metrics, but for closed projects only.

2. **Quick Actions**:
   - **Synchronize all data**: Recalculate all cached metrics for every project (runs portfolio maintenance).
   - **View Projects Table**: Navigate to the Projects Table screen.
   - **View Portfolio Summary**: Open the Summary screen showing all projects as a portfolio-level chart.
   - **Alerts**: Navigate to the Alerts screen to review alert rule results at the portfolio level.

3. **Navigation**:
   - Access to all major app functions
   - Quick navigation to project detail screens
   - Transaction management
   - Performance analytics

### Key Metrics Explained

- **IRR (Internal Rate of Return)**: The annualized return rate that makes the net present value of all cash flows equal to zero. Also referred to as XIRR in some contexts
- **XNPV (Net Present Value)**: The present value of all cash flows, discounted at a specific rate (typically the target IRR or portfolio weighted target IRR)
- **DPI (Distribution to Paid-In)**: Ratio of distributions received to capital invested
- **TVPI (Total Value to Paid-In)**: Ratio of total value (distributions + residual value) to capital invested
- **RVPI (Residual Value to Paid-In)**: Ratio of current residual value to capital invested
- **Estimated Return**: The projected amount that would be returned if all investments grew at the target IRR rate. This metric compounds only investment cash flows forward to the estimation date using the project's target IRR. For portfolios, each project's estimated return is calculated separately using that project's target IRR, then summed together.
- **Gap to Target**: The difference between the actual IRR and the target IRR (IRR minus Target IRR). Positive values indicate performance above target, negative values indicate performance below target
- **NAV (Net Asset Value)**: The current valuation of the project
- **Equity**: The current equity value in the project (calculated as Total Invested - Total Returned + Current NAV)

---

## Reverse NAV Calculator

The **Reverse NAV calculator** is available from the **Summary** screen when viewing a **single project**. It lets you infer a synthetic NAV that would make the project's historical cash flows consistent with a **reported performance metric**:

- **By Multiple (TVPI)**: Enter a reported TVPI (Total Value to Paid-In) as a decimal (e.g. `1.18`). The app:
  - Uses **x-rated invested/returned totals** (`cachedInvested`, `cachedReturned`, `cachedNav`) to compute a synthetic NAV in the **reference currency**.
  - Uses **project-currency invested/returned totals** (`totalInvested`, `totalReturned`, `currentNav.amount`) to compute a synthetic NAV in the **project currency**.
- **By Net IRR**: Enter a reported Net IRR in percent (e.g. `9.5`). The app:
  - Builds **reference-currency cash flows** from transaction `amountXRated` (or `amount * xRate`) and solves for the synthetic NAV that makes the IRR (XIRR) match the reported value.
  - Builds **project-currency cash flows** from transaction `amount` and solves the same IRR equation in project currency.

The calculator displays:

- Current **invested**, **returned**, and **NAV** in both reference and project currencies (when applicable).
- Synthetic NAV in reference currency, the signed difference vs current NAV (e.g. `+1,000 USD vs current NAV`).
- Synthetic NAV in project currency, the signed difference vs current NAV in project currency (e.g. `-850 EUR vs current NAV`).

When you tap **Save as NAV (TVPI)** or **Save as NAV (Net IRR)**:

- The app creates a new explicit NAV record for the project:
  - `amount` is set to the **project-currency synthetic NAV**.
  - `xRate` is inferred from the **ratio** of the reference synthetic NAV to the project synthetic NAV (`xRate = syntheticNavRef / syntheticNavProj`).
  - `amountXRated` is therefore consistent with the reference-currency synthetic NAV.
- The NAV is saved with:
  - **Source**: Manual
  - **Confidence**: Low
  - **XIRR Mode**: Estimated
  - **Issued At**: The current date/time
- Alerts and project metrics are refreshed, and you are navigated back to the project’s transaction list.

### Edge Case: Opposite-Sign Differences Across Currencies

Because the calculator:

- Uses **historic per-transaction exchange rates** for the **reference-currency** view (x-rated cash flows, cached invested/returned/NAV), and
- Uses **project-currency amounts** for the **project-currency** view,

it is possible — and mathematically valid — for the synthetic NAV to be:

- **Lower** than current NAV in project currency (e.g. `-100 EUR vs current NAV`), while
- **Higher** than current NAV in reference currency (e.g. `+150 USD vs current NAV`).

This does **not** indicate a calculation error. It reflects:

- Differences between historic FX used for x-rated cash flows and the FX used when current NAVs were computed/cached.
- The fact that the reverse NAV is solved **separately** in each currency space based on its own cash-flow history.

When the synthetic NAV is saved, the inferred `xRate` ties the new NAV record’s amount and x-rated amount together consistently. Subsequent metrics (IRR, TVPI, etc.) use only those stored values, so the record remains internally consistent even if the implied FX differs from today’s market rate.

---

## Print and Share Reports

You can generate printable and shareable reports for one or more projects. Reports include project performance metrics and, optionally, a chronological transaction/NAV timeline. Use this for presentations, backups, or sharing with stakeholders.

### Opening the Print and Share Screen

- **Print All**: From the **Home** screen, tap the menu (three dots) and select **"Print All"**. The Print and Share screen opens with all projects included.
- **Print Selected**: From the **Home** screen, open the filter menu and use "Select All" or "Manual Selection" to enter selection mode, select one or more projects, then choose **"Print Selected"** from the filter menu. The Print and Share screen opens with only the selected projects.

### Report Options

On the Print and Share screen you can configure what the report includes:

1. **Performance metrics** (choose one):
   - **Basic performance**: Core metrics (e.g. IRR, invested, returned, NAV).
   - **Complete performance**: Full set of metrics, including advanced metrics (e.g. XIRR, xNPV, DPI, TVPI, RVPI, Equity, Target IRR, Gap to Target, Target Value, Required Return, Accumulated Value, and others).

2. **Show transaction list**: Toggle to include a chronological timeline of transactions and NAV records. When enabled, you can also choose **Group by project** to show each project’s timeline in its own section; otherwise the timeline is a single combined list.

3. **Include alerts**: Optional toggle to include project-level alert summaries (rule names, severities, and key value differences) in the report.

4. **Generate Report**: Tap **"Generate Report"** to build the report. A preview opens in a modal with the report title and content.

### Printing and Sharing the Report

After the report is generated, the preview modal offers three actions:

1. **Print**: Opens the system print dialog so you can print to a printer or save as PDF (depending on device).
2. **Share as PDF**: Generates a PDF and opens the system share sheet so you can send the file (email, messaging, cloud storage, etc.).
3. **Share as HTML**: Saves the report as an HTML file and opens the share sheet. The file can be opened in any web browser.

Reports use the current app language for labels and locale-specific number and date formatting.

---

## Settings (Configuration)

The Settings screen (also referred to as Configuration in some locales) lets you choose which **metrics** are visible in different parts of the app, how date-dependent metrics are valued, and how exchange rates are maintained. Preferences are saved automatically and apply as soon as you leave the screen.

### Opening Settings

1. From the **Home** screen, tap the menu (three dots) in the header.
2. Select **"Settings"** (or "Configuration" depending on language).
3. The Settings screen has two tabs:
   - **Exchange Rates**: Manage exchange rates for all currencies relative to the reference currency.
   - **Metrics display**: Configure metrics calculation options and which metrics appear in each part of the app.

### Configurable Sections

1. **Metrics calculation (valuation date)**  
   - Choose how date-dependent metrics such as XIRR and xNPV are valued for single-project views:
     - **Today's date**: Metrics are calculated as of "now".
     - **Latest transaction date**: Metrics are calculated as of the date of the latest transaction or NAV record.
   - Changing this setting triggers a background recalculation of cached metrics so that all screens use the new valuation mode.

2. **Show in project card**  
   - Checkboxes for each metric. You can show or hide optional metrics on project cards on the Home screen.
   - Some metrics (e.g. Invested, Returned, XIRR, NAV) are always shown and cannot be disabled.

3. **Line charts (Summary)**  
   - **Use same selection as project card**: When ON (default), the Summary screen line charts use the same metric selection as the project card.
   - When OFF, a separate list of checkboxes appears so you can choose which metrics appear in the line charts independently of the project card.

4. **Comparison bar charts (Summary)**  
   - Same pattern as line charts: a "Use same selection as project card" switch and, when OFF, your own selection for which metrics appear in the comparison bar charts (when 2+ projects are selected).

5. **Portfolio screen**  
   - Same pattern: mirror project card or a custom selection for the metrics shown on the Portfolio overview screen.

6. **Print & share**  
   - Same pattern: mirror project card or a custom selection for which metrics appear in generated Print and Share reports.

### How It Works

- **Always-included metrics** (e.g. Invested, Returned, XIRR, NAV) appear in all sections and cannot be turned off.
- **Optional metrics** (e.g. xNPV, DPI, TVPI, RVPI, Equity, Target IRR, Gap to Target, Target Value, Required Return, Accumulated Value, and others) can be toggled per section.
- When **"Use same selection as project card"** is ON for a section, that section uses the project card selection; changing the project card checkboxes updates that section too.
- When the mirror switch is OFF, the section keeps its own selection until you change it again.
- Choices are stored on the device and persist across app restarts.

### Exchange Rates Tab

On the **Exchange Rates** tab you can:

1. View all currencies, including which one is the **reference currency** (its rate is always 1 and cannot be edited).
2. See and edit the **exchange rate** of each non-reference currency relative to the reference currency.
3. For each non-reference currency:
   - Enter a new rate using a numeric input with appropriate precision.
   - Tap **Save** to persist the new rate.
4. Validation ensures that:
   - Only positive rates are accepted.
   - Reference currency rate is read-only.

Updated exchange rates are used immediately in all currency-converted metrics (e.g. NAV, Invested, Returned, Equity, xNPV, Target Value, etc.).

---

## App Language (Locale)

The app supports multiple languages and can follow your device’s system language or use a fixed language you choose.

### Changing the App Language

1. From the **Home** screen, tap the menu (three dots) in the header.
2. Select **"Interface Language"**.
3. On the Language screen, choose one of:
   - **Use system locale**: The app uses your device’s language. If the device language is not supported, the app falls back to English.
   - **English**, **Español**, or **Русский**: Use that language regardless of device settings.

Your choice is saved and applied the next time you open the app. All UI labels, validation messages, and formatted numbers and dates use the selected language and locale.

---

## Reset Data

The app provides a reset data feature that allows you to:

1. **Delete All Projects**: Remove all projects and their associated transactions and NAV records
2. **Reset to Defaults**: Delete all projects; restore categories, currencies, and alert rules to their default built-in values

### Using Reset Data

1. Navigate to the **Home** screen
2. Tap the menu button (three dots) in the header
3. Select **"Reset Data"**
4. Choose an option:
   - **Delete Projects**: Delete all projects (only available if projects exist). Categories, currencies, and alert rules are left unchanged.
   - **Reset to Defaults**: Delete all projects and restore categories, currencies, and alert rules to defaults (built-in rules are re-seeded; user-defined rules are removed)
   - **Cancel**: Close the dialog without making changes

**Warning**: These actions cannot be undone. All project data, transactions, and NAV records will be permanently deleted.

**Note**: 
- When resetting to defaults, built-in categories and currencies are restored to their original values
- User-defined categories and currencies are deleted
- Exchange rates for built-in currencies are preserved during restore
- Built-in alert rules are restored to their default definitions; custom alert rules are deleted

---

## Tips and Best Practices

1. **Project Setup**:
   - Set up categories and currencies before creating projects
   - Choose the project currency carefully as it cannot be changed
   - Set realistic target IRR values for better tracking and estimated return calculations

2. **Transaction Management**:
   - Ensure transaction dates are unique per project
   - Future dates are allowed for planning future transactions and NAV records
   - Use descriptions to track transaction purposes
   - Remember that cash transactions automatically create NAV records

3. **NAV Records**:
   - Update NAV regularly for accurate performance metrics
   - Use appropriate confidence levels and sources for manual NAV records
   - Automatic NAV records use default values and can be updated later if needed
   - NAV values cannot be negative - the system enforces this

4. **Data Backup**:
   - Regularly export your projects as backup using "Export All" from the Home screen menu
   - You can also select specific projects and export only those
   - Export files can be imported on other devices
   - Keep export files in a safe location (cloud storage, email to yourself, etc.)

5. **Performance Tracking**:
   - Refresh metrics in the Projects Table when returning to the app after some time, after data import, or when you need metrics that depend on the current date
   - Use the Projects Table view for quick comparisons of all metrics
   - Use the Summary screen charts to track trends over time
   - For multiple projects, use comparison bar charts to identify top and bottom performers
   - Export Projects Table as PDF or HTML for reporting and presentations
   - Use **Print and Share Reports** (Print All or Print Selected) to generate custom reports with optional transaction timelines, then print or share as PDF/HTML

6. **Multi-Currency Portfolios**:
   - All calculations are converted to the reference currency
   - Exchange rates are stored per transaction/NAV
   - Update exchange rates in currency management when needed
   - The Projects Table and Summary screens display all values in the reference currency

7. **Project Lifecycle**:
   - Close projects when they are completed by setting the Active switch to OFF
   - Ensure the closed date is later than or equal to all transaction and NAV record dates
   - Use the filter on Home screen to view Active, Closed, or All projects
   - Closed projects are excluded from active portfolio calculations but included when viewing "All"

8. **Project Selection and Comparison**:
   - Use the filter menu on Home screen to enter selection mode and select multiple projects for bulk operations
   - Selected projects can be viewed together in Summary screen for comparison
   - Use "Select All" to quickly select all visible projects (filtered by Active/Closed/All)
   - Export multiple projects together for portfolio-level backups
   - Use "Print Selected" to generate reports for only the selected projects

9. **Alerts**:
   - Use **Manage Alert Rules** (menu from Home) to create time-based or value-based rules
   - Tap the alerts widget on Home to open the Alerts screen and see which projects need attention
   - Scope rules to specific categories so only relevant projects are evaluated
   - Restore built-in rules from the Alert Rule List if you removed or changed them

10. **Language and Locale**:
   - Change the app language via the menu → **Interface Language**
   - Choose "Use system locale" to follow your device language, or pick English, Español, or Русский
   - The selected language applies to all screens, numbers, and dates

11. **Settings (Configuration)**:
   - Use the menu → **Settings** to control which metrics appear on project cards, Summary charts, Portfolio screen, and Print & share reports
   - Turn "Use same selection as project card" OFF for any section to choose a different set of metrics for that section only
   - Preferences are saved automatically when you change them

---

## Troubleshooting

### Common Issues

1. **Cannot delete transaction/NAV**:
   - Check if there are dependencies preventing deletion
   - Some records may be required for data integrity

2. **Cannot change project currency**:
   - Currency is immutable after project creation by design
   - Create a new project with the desired currency if needed

3. **Metrics not updating**:
   - Use the "Refresh All Metrics" button in the Projects Table
   - Metrics recalculate automatically after data changes; use Refresh when returning to the app or after import (some metrics depend on current date)

4. **Import fails**:
   - Check that the file is a valid export file
   - Ensure currency compatibility when replacing projects
   - Review error messages for specific issues

5. **NAV value becomes negative**:
   - The system prevents negative NAV values
   - If you see this, it may indicate a data inconsistency - check your transactions

---

## Understanding Key Metrics in Detail

### Target Value, Required Return, and Accumulated Value

The app exposes three closely related target-based metrics that generalize the older "Estimated Return" concept:

- **Target Value**: Theoretical value of invested capital as of the valuation date, assuming all investments in a project (or portfolio) grew at the **target IRR**. Only **investments** (negative cash flows) are compounded forward; actual returns (positive cash flows) are ignored. This is a currency value in the reference currency.
- **Required Return**: Additional amount (in currency) that would need to be returned at the valuation date so that the project (or portfolio) achieves its **target IRR** given all historical cash flows and current NAV. It answers “how much more do we need to receive to hit target IRR?”.
- **Accumulated Value**: All historical cash flows (investments and returns) compounded to the valuation date at the **target IRR**, excluding hypothetical exit NAV flows. It represents a single “value at the valuation date” consistent with that IRR.

**Key characteristics**:
- Each project uses its own target IRR for these calculations; portfolio values are sums of per-project results.
- The **valuation date** for single-project views follows your Metrics setting (today vs. latest transaction/NAV date); portfolio views use a portfolio-level "as of" date (typically today).
- Closed projects are not projected beyond their close date; valuation and cash flows are capped at `closedAt`.



