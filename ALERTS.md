# Alerts — rules and behaviour

This note explains **what alerts are**, how **alert rules** are built, and how they interact with your projects. It complements the short overview in **[APP_FUNCTIONS.md](APP_FUNCTIONS.md)** and the JSON details in **[IMPORT_DATA_FORMAT.md](IMPORT_DATA_FORMAT.md)**.

---

## What alerts are for

Alerts help you notice **conditions across many projects** that would be easy to miss in day-to-day use: long gaps without a valuation, performance relative to a target, large NAV moves over a recent period, and similar checks. The app evaluates rules **on your device** using the data you already entered; it does not pull data from banks or brokers.

---

## What you see

- **Alerts** from the Home screen (or portfolio shortcuts) shows a **summary** of severities and a **list of projects** where at least one rule fired. You can open a project from there to review its transactions and NAV history.
- **Manage alert rules** (Home menu) is where you **define, enable, disable, reorder, export, import, restore built-in rules, or remove all rules**.

Alerts reflect **current** project state from the **last evaluation run**. They are **per project** (not a single “portfolio-wide” alert row). The app does not keep a long history of past alert runs inside the database; the list you see is the latest result.

---

## When rules are evaluated

The app refreshes alert results when it makes sense for your workflow, for example:

- When you open or return to the app (including after it was in the background).
- On a periodic interval while you use the app.
- After you change data that affects metrics (projects, transactions, NAV history, imports, resets, and similar).

You do **not** need to open the Alerts screen to “run” a check; that screen shows the most recent result.

---

## Rule kinds: time and value

Each rule is **one condition** and **one severity** if that condition is met. You can define several rules (for example a stricter and a looser NAV gap) as separate rules.

### Time rules

A **time** rule fires when **nothing relevant has happened** for a project **within the last *N* days** (counted from the evaluation date the app uses for that project).

You choose:

| What you set | Meaning |
| ------------ | ------- |
| **Event** | Which “last activity” date to watch: **latest NAV update**, **latest return** (distribution), or **latest investment** (capital call / contribution). |
| **Time window (days)** | If the event is older than this many days (or never occurred), the rule fires. |

**Young projects:** If the project has not existed long enough for the full window to pass (measured from the first transaction), the rule does **not** fire. That avoids false “no update” alerts right after you create a project.

### Value rules

A **value** rule compares a **metric** on the project to a **reference** (fixed number or a value from the past).

You choose:

| What you set | Meaning |
| ------------ | ------- |
| **Metric** | What to measure — for example XIRR, xNPV, DPI, TVPI, RVPI, NAV, invested, returned, net capital, or **gap to target** (XIRR relative to the project’s target IRR). |
| **Comparison** | How to compare the **current** metric to the **reference** — see below. |
| **Reference** | Either a **fixed value** you type, or **compare to past** (a value as of a number of **days ago**). |

**Valuation date:** Metrics used in alerts follow the same **valuation date** choice as elsewhere in the app (**today** vs **latest recorded activity**), so single-project and portfolio settings stay aligned.

#### Comparisons

- **Less than / less than or equal / greater than / greater than or equal / equal** — The rule fires when the **ordering** between the current metric and the reference is true (for example current gap to target **less than** zero means “below target”). For these modes, the important inputs are the **metric**, **reference**, and **comparison type**; the form may still ask for a numeric **threshold** field for compatibility, but the **order** comparison is what matters.
- **Percent drop** — The **threshold** is the **minimum percentage drop** from the reference to the current value that triggers the alert (for example “at least 5%” drop from the value 14 days ago). If the reference is zero or negative, the rule does not fire (to avoid meaningless comparisons).
- **Percent growth** — Same idea for **growth**; the threshold is the **minimum percentage increase** from the reference to the current value. If the reference is zero, the rule does not fire.

**Missing data:** If the current metric value is missing, the rule does not fire. For **compare to past**, if the app cannot compute a reference value for the chosen period, the rule does not fire.

**NAV and past periods:** When comparing NAV to a past point in time, the app uses the same kind of metric history as your charts, including flows and NAV entries in the relevant window, so the “past” reference is consistent with the rest of the app.

---

## One alert per “family” per project (shadowing)

If several rules would fire for the **same project** and they belong to the **same family**, the app only shows the **single most severe** one.

- For **value** rules, the family is the **metric** (for example all NAV-based rules share one family).
- For **time** rules, the family is the **event** (for example all “latest NAV update” rules share one family).

So you can define overlapping rules (e.g. “no NAV for 90 days” vs “no NAV for 180 days”) without cluttering the list with the same issue twice; the stronger severity wins.

---

## Severity, display order, and enabled

- **Severity** (e.g. critical, high, medium, low, info, success) describes how important the alert is when it fires. **Success** is useful for positive signals (for example “above target”).
- **Display order** controls how rules are listed when you manage them; it does not change evaluation logic.
- **Disabled** rules are skipped entirely.

Each rule has a **code** (unique identifier) and a **name** displayed in lists. You can leave code empty when creating a rule; the app can generate one.

---

## Categories (scope)

A rule can apply **globally** or only to **selected categories**.

- **No categories selected** — the rule applies to **all** projects, including projects in categories you add later.
- **One or more categories selected** — the rule applies **only** to projects in those categories.

If you import rules that reference a category ID that does not exist on your device, those links are skipped; the rule is still imported.

---

## Built-in rules

When you **restore built-in rules**, the app installs a set of starter rules (names and defaults may vary slightly by language). You can edit, disable, or delete them like any other rule. Typical examples include:

- No NAV update for **3 months** or **6 months** (time rules).
- **NAV** percent drop or growth over **14 days** (value rules, often off by default).
- **Gap to target** negative or positive (value rules vs a fixed reference of zero).
- Optional **no returns** or **no investments** in **90 days** for some categories (time rules).

Exact wording appears in the app under **Manage alert rules**.

---

## Export and import

- **Export alert rules** produces a JSON file with only alert rules (`dataType: "alertRules"`).
- **Import alert rules** can merge rules from such a file: new codes are added; **conflicting** codes can be **skipped** or **replaced** per rule.

For field names and structure, see **[IMPORT_DATA_FORMAT.md](IMPORT_DATA_FORMAT.md)** (Alert rules section).

---

## Limitations (by design)

- Alerts **do not** send push notifications or email; they are **in-app** summaries.
- There is **no** built-in historical log of every alert that ever fired; you see the **latest** evaluation result.
- Rules are evaluated from **your** recorded cash flows and NAV — not live market prices from external feeds.

If you need the **math** behind the metrics themselves (IRR, multiples, gap to target, etc.), see **[METRICS.md](METRICS.md)**.
