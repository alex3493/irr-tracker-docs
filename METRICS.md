# Metrics — mathematical reference

This note describes **how irrtracker defines and computes** the main performance and target-based figures from your cash flows and NAV inputs. It is written for readers who want the **math**, not the software internals. Rounding and display rules in the app may add small differences at the last decimal.

---

## Conventions

### Cash flows

- **Investments** (capital calls, contributions) enter the engine as **negative** amounts.
- **Returns** (distributions) enter as **positive** amounts.
- **NAV** at the valuation horizon is usually represented as a **positive** “exit” cash flow at the valuation date when IRR-style metrics are computed (so the timeline reflects both realized flows and remaining value).

### Time and rates

- Time between dates is measured in **fractional years** using **365.25 days per year** (astronomical year basis).
- If $d$ is the number of days between two dates, the length in years is  
  $$
  t = \frac{d}{365.25}.
  $$
- **IRR** and **target IRR** are stored and shown as **percentages** in the interface (e.g. 12 for 12%), but formulas below use **decimal rates** (e.g. $r = 0.12$) unless stated otherwise.

### Currencies

- Per-project metrics are ultimately expressed in a **reference currency** using the exchange rates you record (or system defaults) on each flow. The formulas are the same; amounts are assumed **already converted** to that basis when a formula uses a single currency.

### Valuation date

Many metrics depend on **which date** is used as the “horizon” (evaluation date). In **Settings**, you choose either **today** or **latest recorded activity** (last cash flow or NAV). **Closed** projects are always cut off at their **close date** for this purpose. The same choice applies to **single-project** and **portfolio** views unless a screen states otherwise.

---

## XIRR (money-weighted return)

**Question answered:** *What constant annualized discount rate makes the present value of all dated cash flows equal to zero?*

Let the sorted cash flows be $(A_i, t_i)$, where $A_i$ is the signed amount on date $i$ and time is measured in **years** from an arbitrary reference (the implementation uses the first cash-flow date as the base for the NPV sum). The XIRR is the value $r$ such that:

$$
\sum_i \frac{A_i}{(1+r)^{t_i}} = 0.
$$

Equivalently, with $t_i$ = years from the **first** cash-flow date to flow $i$:

$$
\text{NPV}(r) = \sum_i A_i\,(1+r)^{-t_i} = 0.
$$

- **Display:** The app shows XIRR as an **annualized percentage** (e.g. $r \times 100$).
- **Multiple solutions:** Some sequences of flows admit **more than one** rate $r$ that satisfies the equation. The app selects a **single** “most credible” rate (for example preferring economically sensible roots when net flows suggest a loss versus a gain). Very small rates near zero are treated as zero after calculation.

---

## xNPV (net present value at a chosen rate)

**Question answered:** *What is the present value of all cash flows when each is discounted at a fixed rate?*

Using the **first** cash-flow date as the base date $t=0$, and a **decimal** discount rate $r$ (e.g. target IRR):

$$
\text{xNPV}(r) = \sum_i A_i\,(1+r)^{-t_i},
$$

where $t_i$ is the time in years from the base date to flow $i$.

- If $r = -1$, the expression is undefined; the app does not compute xNPV in that case.
- The rate used for xNPV in the app is typically the **target IRR** (or a portfolio-level weighted target, depending on context).

---

## Multiples (DPI, TVPI, RVPI)

Let:

- $I$ = **total invested** (sum of the magnitudes of investment flows, or equivalent aggregate in reference currency),
- $R$ = **total returned** (sum of distributions),
- $N$ = **current NAV** (residual asset value) at the valuation date.

(If nothing is invested, $I=0$ and ratios are defined as **0** in the app to avoid division by zero.)

| Metric | Formula | Reading |
|--------|---------|--------|
| **DPI** (paid-in distributions) | $\displaystyle \text{DPI} = \frac{R}{I}$ | How much you have received back **per unit** of capital called. |
| **RVPI** (residual to paid-in) | $\displaystyle \text{RVPI} = \frac{N}{I}$ | Unrealized value still in the deal, per unit called. |
| **TVPI** (total value to paid-in) | $\displaystyle \text{TVPI} = \frac{R + N}{I}$ | **DPI + RVPI**; total economic value (realized + unrealized) per unit called. |

---

## Net capital

**Question answered:** *On a pure cash-in / cash-out basis, how much capital is still “at stake” before NAV?*

$$
\text{Net capital} = I - R
$$

computed from **investment and return transactions only** (not by adding NAV). It can be **negative** if cumulative distributions exceed cumulative calls. **NAV** is handled separately in multiples and IRR, not inside this definition.

---

## Target value

**Question answered:** *If every **investment** had compounded at the **target IRR** to the evaluation date $T$, what would their sum be?*

Only **negative** cash flows (investments) are included. For each investment with amount $A_c < 0$ on date $d_c$, let $t_c$ be years from $d_c$ to $T$. With target IRR $r$ (decimal):

$$
\text{Target value} = \sum_{A_c < 0} |A_c| \cdot (1 + r)^{t_c}.
$$

- **Returns** (positive flows) do **not** enter this sum.
- If $T$ is **before** an investment date, $t_c$ is negative and the term **discounts** that investment back to $T$.

**Portfolios:** Target value is computed **per project** with that project’s own target IRR, then **summed** across projects (not one blended rate applied to all flows).

---

## Accumulated value

**Question answered:** *If **every** cash flow (investments and returns) were compounded at the **target IRR** to date $T$, what single figure represents that compounded position?*

Hypothetical **exit NAV** lines used elsewhere for IRR are **excluded** here.

Let $A_i$ be the **signed** amount of flow $i$ (investments negative, returns positive), and $t_i$ the years from that flow’s date to $T$. First form the raw compounded sum

$$
S = \sum_i A_i\,(1 + r)^{t_i}.
$$

The app displays **accumulated value** as **$-S$** so the metric is shown in the usual **positive** currency form for this screen. Equivalently,

$$
\text{Accumulated value (displayed)} = -\sum_i A_i\,(1 + r)^{t_i}.
$$

---

## Required return

**Question answered:** *How much **additional** value at the evaluation date $T$ would make the project’s NPV at the **target IRR** equal to zero, given all historical flows (and usually including current NAV in the PV)?*

1. Compute the present value of the relevant cash flows at the target rate $r$, using the **first** flow date as the base for xNPV:

   $$
   PV = \text{xNPV}(r).
   $$

2. Let $t$ be years from that base date to $T$. The required future value $FV$ at $T$ that clears NPV is:

   $$
   FV = -PV \cdot (1 + r)^{t}.
   $$

Intuition: $PV + \dfrac{FV}{(1+r)^t} = 0$ when the only missing piece is a single value at $T$.

Depending on options, the app may report this as **additional** return needed on top of NAV already modeled in the flows, or a **total** required exit — the same structural formula applies; the cash-flow set differs.

---

## Gap to target

**Question answered:** *How far is actual performance from the target IRR?*

$$
\text{Gap to target} = r_{\text{actual}} - r_{\text{target}},
$$

where $r_{\text{actual}}$ is the **XIRR** (as a decimal or shown in percentage points consistently in the UI), and $r_{\text{target}}$ is the project’s target IRR. Positive means **above** target; negative means **below**.

---

## Portfolio “target IRR” (display)

For a **weighted average** of target IRRs across projects (used for some portfolio displays and for discounting in xNPV in portfolio context), the app uses **investment weights**:

$$
r_{\text{portfolio}} = \frac{\sum_p I_p \, r_p}{\sum_p I_p},
$$

where $I_p$ is total invested in project $p$ and $r_p$ is that project’s target IRR (decimal). Projects with no investment or no target may be omitted from this average.

This **weighted** rate is **not** the same object as **target value** for a portfolio, which sums per-project target values at **each project’s** own target IRR (see above).

---

## Summary table

| Metric | Type | Depends on |
|--------|------|------------|
| XIRR | Rate | Dated signed cash flows (+ exit NAV treatment for IRR) |
| xNPV | Currency | Same flows, fixed discount rate |
| DPI / TVPI / RVPI | Ratio | Invested, returned, NAV |
| Net capital | Currency | Invested − returned (no NAV) |
| Target value | Currency | Investments only, compound at target IRR to $T$ |
| Accumulated value | Currency | Investments + returns (excl. exit NAV line), compound at target IRR |
| Required return | Currency | xNPV at target IRR + scaling to $T$ |
| Gap to target | Percentage points | XIRR − target IRR |

---

## Disclaimer

Formulas here describe **irrtracker’s** definitions. **Fund- or manager-reported** IRR, TVPI, or NAV may differ because of fees, carry, different dates, subscription lines, or time-weighted versus money-weighted methods. Use this document to interpret **what the app computes** from the data you enter.
