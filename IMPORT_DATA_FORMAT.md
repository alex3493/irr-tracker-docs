# Import data format

This document describes the JSON formats the app can read when **importing projects**, **importing alert rules**, or **round-tripping data** from exports. Field names match the app’s export payloads unless noted.

## Two kinds of import files

| Top-level `dataType` | Purpose | Where to import from in the app |
| -------------------- | ------- | -------------------------------- |
| **`projects`** (or legacy shape without `dataType`) | Projects, transactions, NAV history, currencies, categories | **Home → Import data** (pick a JSON file). |
| **`alertRules`** | Alert rules only | **Manage alert rules →** menu **→ Import alert rules**, or pick the same file from **Import data** (the app detects `dataType` and runs the alert-rules flow). |

- **Do not** rely on an `alertRules` array inside an old **projects** export: that embedded list is **deprecated and ignored** on import. Use a dedicated **`dataType: "alertRules"`** file from **Export alert rules** on the Alert Rules screen.
- If you open **Import data** (for projects) with a file whose root `dataType` is **`"alertRules"`**, the app will ask you to import it using the **alert rules** import path instead.

## Version compatibility

- Exports include **`version`** (app semver) and **`exportDate`** (ISO 8601).
- **Import requires the same major version** as the running app (for example `1.x` with `1.y.z`). If the major version differs, import is rejected.

## Format 1: Projects (recommended)

Include a discriminator and metadata:

```json
{
  "dataType": "projects",
  "version": "1.2.0",
  "exportDate": "2024-01-15T10:30:00.000Z",
  "projects": [],
  "currencies": [],
  "categories": []
}
```

Omitting `dataType` is still accepted if the object has a **`projects`** array (treated as projects data).

## Format 2: Legacy projects array

A JSON **array** of project objects (no wrapper) is still supported:

```json
[
  {
    "name": "Project Name",
    "categoryId": "...",
    "currencyId": "..."
  }
]
```

## Format 3: Alert rules (dedicated export)

Produced by **Manage alert rules → Export alert rules**. Not mixed with projects.

```json
{
  "dataType": "alertRules",
  "version": "1.2.0",
  "exportDate": "2024-01-15T10:30:00.000Z",
  "alertRules": []
}
```

### Alert rule objects (`alertRules[]`)

Each element has the following shape:

| Field | Type | Notes |
| ----- | ---- | ----- |
| `id` | string | **Stable rule code** (used to detect duplicates). Required for import. |
| `name` | string | Display name. |
| `kind` | `"value"` \| `"time"` | Rule type. |
| `severity` | string \| null | Optional (e.g. severity level for the UI). |
| `ruleDefinition` | object | JSON object describing the rule; its fields depend on `kind` (`time` vs `value`). |
| `enabled` | boolean \| null | Optional. |
| `displayOrder` | number \| null | Optional sort hint. |
| `categoryIds` | string[] | Categories to scope the rule to; **empty** = global. Category ids that **do not** exist on the device are **skipped** (the rule is still imported, but without those links). |
| `createdAt` | string \| undefined | Optional ISO 8601. |
| `updatedAt` | string \| null | Optional ISO 8601. |

**Importing alert rules**

- Rules whose `id` is **not** already present are **created**.
- If `id` matches an **existing** rule’s code, the app treats it as a **conflict**. You can choose **skip** or **replace** for each conflicting rule (and non-conflicting rules import without prompting).
- After import, **category links** only apply to categories that already exist on the device.

**Exporting alert rules**

- Use **Manage alert rules** only. The file contains **only** `dataType`, `version`, `exportDate`, and `alertRules`.

The most reliable reference is a file produced by **Export alert rules** in the app. You can also start from the sample below.

### Sample alert rules file

Below is a **minimal valid** file with one **time** rule and one **value** rule. `ruleDefinition` follows the same patterns as the app’s pre-installed rules. Use a **`version`** whose **major** number matches your app (see [Version compatibility](#version-compatibility)); each rule’s **`id`** must be unique among the rules you import.

```json
{
  "dataType": "alertRules",
  "version": "1.1.2",
  "exportDate": "2025-04-01T12:00:00.000Z",
  "alertRules": [
    {
      "id": "NO_NAV_UPDATE_LONG",
      "name": "No NAV update (180 days)",
      "kind": "time",
      "severity": "HIGH",
      "ruleDefinition": {
        "event": "latest_nav_update",
        "timeWindowDays": 180
      },
      "enabled": true,
      "displayOrder": 1,
      "categoryIds": []
    },
    {
      "id": "XIRR_GAP_TO_TARGET_NEGATIVE",
      "name": "Gap to target is negative",
      "kind": "value",
      "severity": "MEDIUM",
      "ruleDefinition": {
        "metric": "gapToTarget",
        "comparison": "lt",
        "threshold": 0,
        "referenceMode": "fixedValue",
        "fixedValue": 0
      },
      "enabled": true,
      "displayOrder": 2,
      "categoryIds": []
    }
  ]
}
```

---

## Project structure (`projects[]`)

Each project must conform to:

### Required fields

- `name` (string): Unique project name.
- `categoryId` (string): Category id.
- `currencyId` (string): Currency id.

### Optional fields

- `id` (string): If provided, used for conflict detection. If omitted, a new project is created with a generated id.
- `category` (object): If the category is missing on the device, it can be created from this object (and from `categories[]` at the root when provided).
- `currency` (object): Same for currencies.
- `description` (string)
- `targetIrr` (number)
- `holdYears` (number \| null): Planned hold in years; omit or `null` if unset.
- **Cached metrics** (all optional; often omitted in **current** app exports — the app recalculates metrics after import):
  - `cachedIrr`, `cachedXNpv`, `cachedDpi`, `cachedTvpi`, `cachedRvpi`
  - `cachedInvested`, `cachedReturned`, `cachedNav`
  - `cachedTargetValue`, `cachedRequiredReturn`, `cachedAccumulatedValue`
  - `metricsCachedAt` (ISO 8601)
- `cachedEquity` (number, **deprecated**): **Ignored on import.** Net capital in reference currency is **not** stored as a separate column; it is implied from **`cachedInvested`** and **`cachedReturned`** when those are present (net capital ≈ invested − returned when both are set).
- `closedAt` (string \| null): ISO 8601 when closed, or `null` if active.
- `createdAt` (string): ISO 8601 (the app may set its own creation times for imported records).
- `updatedAt` (string \| null)
- `transactions` (array): Cash-flow rows; defaults to empty if omitted.
- `netAssetValueHistory` (array): NAV rows; defaults to empty if omitted.

### Example project

```json
{
  "id": "project-123",
  "name": "Startup Investment A",
  "description": "Early stage investment in tech startup",
  "categoryId": "cat-venture-capital",
  "currencyId": "usd",
  "targetIrr": 25.0,
  "cachedIrr": 18.5,
  "cachedXNpv": 150000,
  "cachedDpi": 1.2,
  "cachedTvpi": 1.5,
  "cachedRvpi": 0.3,
  "cachedInvested": 100000,
  "cachedReturned": 120000,
  "cachedNav": 150000,
  "cachedTargetValue": 0,
  "cachedRequiredReturn": 0,
  "cachedAccumulatedValue": 0,
  "metricsCachedAt": "2024-01-15T10:00:00.000Z",
  "closedAt": null,
  "createdAt": "2023-06-01T00:00:00.000Z",
  "updatedAt": "2024-01-15T10:00:00.000Z",
  "transactions": [],
  "netAssetValueHistory": []
}
```

---

## Transaction structure (`transactions[]`)

Cash-flow rows only. **NAV** is not a transaction type in the export; NAV lives in **`netAssetValueHistory`**.

### Required fields

- `projectId` (string)
- `amount` (number)
- `xRate` (number): Exchange rate vs the **reference** currency at the transaction date.
- `transactionType` (string): **`"investment"`** or **`"return"`** only.
- `equity` (number): **Cumulative net capital** in **project currency** after this row (running invested − returned on cash flows). Despite the name, it is **not** an equity *percentage* or ownership share.
- `issuedAt` (string): ISO 8601
- `createdAt` (string): ISO 8601

### Optional fields

- `id` (string)
- `description` (string \| null)
- `updatedAt` (string \| null)

### Example transaction

```json
{
  "id": "txn-456",
  "projectId": "project-123",
  "description": "Initial investment",
  "amount": 50000,
  "xRate": 1.0,
  "transactionType": "investment",
  "equity": 50000,
  "issuedAt": "2023-06-01T00:00:00.000Z",
  "createdAt": "2023-06-01T12:00:00.000Z",
  "updatedAt": null
}
```

---

## NAV history structure (`netAssetValueHistory[]`)

### Required fields

- `projectId` (string)
- `amount` (number): Non-negative; negative values are clamped on import.
- `xRate` (number)
- `source` (string): One of **`official`**, **`manual`**, **`system`**. Invalid values are **mapped to `official`**.
- `confidence` (string): **`high`**, **`medium`**, or **`low`**. Invalid values default to **`low`**.
- `xirr_mode` (string): **`reported`**, **`estimated`**, or **`transaction_adjusted`**. Invalid values default to **`reported`**.
- `issuedAt` (string): ISO 8601
- `createdAt` (string): ISO 8601

### Optional fields

- `id` (string)
- `transactionId` (string \| null): Links to a cash transaction when this NAV is auto-generated alongside it.
- `description` (string \| null)
- `synthetic_reason` (string \| null): Optional; reserved for use by the app.
- `updatedAt` (string \| null)

### NAV options and export `version`

For export **`version` ≥ 1.1.0**, **`source`**, **`confidence`**, and **`xirr_mode`** are read from the file (after normalization).

For **older** files or missing `version`, import applies **legacy mapping**: NAV rows linked to a **transaction** are forced to **system** source, **high** confidence, and **transaction_adjusted**; **standalone** NAV rows use **official** / **reported** behavior with confidence taken from the file when valid.

### Example NAV entry

```json
{
  "id": "nav-789",
  "projectId": "project-123",
  "transactionId": null,
  "description": "Q4 2023 valuation",
  "amount": 55000,
  "xRate": 1.0,
  "source": "manual",
  "confidence": "high",
  "xirr_mode": "reported",
  "synthetic_reason": null,
  "issuedAt": "2023-12-31T00:00:00.000Z",
  "createdAt": "2024-01-01T10:00:00.000Z",
  "updatedAt": null
}
```

---

## Currency structure (`currencies[]`)

### Required fields

- `id` (string): Currency code (e.g. `usd`, `eur`).
- `name` (string)
- `symbol` (string)
- `decimalDigits` (number)
- `xRate` (number): Rate relative to the **reference** currency (reference currency uses `1`).

---

## Category structure (`categories[]`)

### Required fields

- `id` (string)
- `name` (string)
- `riskLevel` (string)

### Optional fields

- `description` (string \| null)
- `investmentProfileId` (string \| null): **Investment strategy** profile id (used for Insights and strategy alignment). When a category is **created from an import file**, a missing or invalid id is stored as **`buyout_control`** (the app default). Categories that already exist on the device are not changed by import.
- `typicalHoldYearsMin`, `typicalHoldYearsMax` (number \| null)
- `minIrr`, `baseIrr`, `maxIrr` (number \| null)

### Supported `investmentProfileId` values

Use exactly one of these string ids (as in exports). Display names match the English UI strings in the app:

| `investmentProfileId` | Strategy (English UI name) |
| --------------------- | --------------------------- |
| `core_income` | Core / stabilized income |
| `value_add` | Value-add / operational |
| `development_opportunistic` | Development / opportunistic |
| `venture_growth` | Venture / early-stage growth |
| `buyout_control` | Buyout / control PE |
| `private_credit` | Private credit / direct lending |
| `real_estate` | Real estate (asset-backed) |
| `infrastructure` | Infrastructure |

### Example category

```json
{
  "id": "cat-venture-capital",
  "name": "Venture Capital",
  "description": "Early stage venture capital investments",
  "riskLevel": "high",
  "investmentProfileId": "venture_growth",
  "typicalHoldYearsMin": 5,
  "typicalHoldYearsMax": 10,
  "minIrr": 15.0,
  "baseIrr": 25.0,
  "maxIrr": 50.0
}
```

---

## Validation (projects import)

1. The file must parse as JSON.
2. At least **one project** must be present after parsing.
3. Each project must have **`name`**, **`categoryId`**, and **`currencyId`**.
4. If present, **`transactions`** and **`netAssetValueHistory`** must be arrays.

### Project conflict resolution

When an imported project **id** or **name** matches existing data, the UI offers:

- **Skip** — Do not import that project.
- **Copy** — Import as a new project (new id; name may get a `(copy)` suffix if needed).
- **Replace** — Replace the existing project; the existing project’s id is kept where applicable.

**Replace** cannot change a project’s **currency**; a mismatch with the file causes an error.

---

## Date format

Use ISO 8601 strings, for example:

`2024-01-15T10:30:00.000Z`

---

## Complete example (projects)

```json
{
  "dataType": "projects",
  "version": "1.2.0",
  "exportDate": "2024-01-15T10:30:00.000Z",
  "currencies": [
    {
      "id": "usd",
      "name": "US Dollar",
      "symbol": "$",
      "decimalDigits": 2,
      "xRate": 1.0
    }
  ],
  "categories": [
    {
      "id": "cat-vc",
      "name": "Venture Capital",
      "description": "VC investments",
      "riskLevel": "high",
      "investmentProfileId": "venture_growth",
      "typicalHoldYearsMin": 5,
      "typicalHoldYearsMax": 10,
      "minIrr": 15.0,
      "baseIrr": 25.0,
      "maxIrr": 50.0
    }
  ],
  "projects": [
    {
      "id": "proj-1",
      "name": "My Startup Investment",
      "description": "Investment in tech startup",
      "categoryId": "cat-vc",
      "currencyId": "usd",
      "targetIrr": 30.0,
      "closedAt": null,
      "createdAt": "2023-01-01T00:00:00.000Z",
      "updatedAt": "2024-01-15T10:00:00.000Z",
      "transactions": [
        {
          "projectId": "proj-1",
          "description": "Initial investment",
          "amount": 100000,
          "xRate": 1.0,
          "transactionType": "investment",
          "equity": 100000,
          "issuedAt": "2023-01-01T00:00:00.000Z",
          "createdAt": "2023-01-01T00:00:00.000Z"
        }
      ],
      "netAssetValueHistory": [
        {
          "projectId": "proj-1",
          "description": "Q1 2024 valuation",
          "amount": 120000,
          "xRate": 1.0,
          "source": "manual",
          "confidence": "high",
          "xirr_mode": "reported",
          "issuedAt": "2024-03-31T00:00:00.000Z",
          "createdAt": "2024-04-01T00:00:00.000Z"
        }
      ]
    }
  ]
}
```

---

## Error handling (typical)

Imports may fail with messages such as:

- Invalid JSON.
- Empty or unsupported structure (wrong `dataType` for the chosen flow).
- **Version mismatch** (major version different from the app).
- Missing required project fields or wrong array types.
- Using **Import data** for projects with a file that contains **only** alert rules (see [Two kinds of import files](#two-kinds-of-import-files)).
- Project **limit** reached on the free tier when creating new projects.

---

## Notes

- Include **`currencies`** and **`categories`** in the file when projects reference ids that don’t exist yet; they are created automatically when valid.
- **`projectId`** on transactions and NAV rows ties rows to the parent project; **transaction IDs** are remapped on import when ids are regenerated.
- **`id` on alert rules** is the stable **code** used for matching rules that already exist in the app (not an internal row id you need to manage).
- Numeric fields should be JSON numbers (no quotes). Use booleans only where a field is defined as boolean (e.g. alert rule `enabled`).
- After a successful **projects** import, the app runs **portfolio maintenance** (metrics refresh, alerts, etc.). Hand-supplied cached metrics may be overwritten by recalculation.
