---
name: receipt-scanner
description: Scan receipts, invoices, and bills from photos or PDFs. AI extracts merchant, date, amount, tax, payment method, and category. Auto-logs to Google Sheets expense tracker with monthly summaries and budget analysis.
---

# Receipt Scanner & Expense Tracker

Upload receipt photos, PDF invoices, or email confirmations. AI automatically extracts all financial details and logs them to a structured Google Sheets expense tracker with categorization, monthly summaries, and budget analysis.

## When to Use
- "Scan this receipt" (user uploads photo)
- "Log my expenses from these receipts" (batch upload)
- "Scan my email for recent purchase receipts"
- "Show me my spending summary for this month"
- "Set up an expense tracker for my business"

## Required Context (ask if not provided)
- **Receipt source**: Photo upload, PDF, email, or screenshot
- **Currency**: Default currency (USD, EUR, etc.)
- **Categories**: Custom categories or use defaults
- **Existing tracker**: Link to existing Google Sheet, or create new

## Platform Integrations
| Platform | Usage |
|---|---|
| Local Files / Uploads | Receipt photos, PDF invoices |
| AI Vision Model | OCR + intelligent field extraction |
| Google Sheets | Expense tracker, monthly summaries, budget vs actual |
| Gmail (optional) | Scan inbox for digital receipts (Amazon, Uber, subscriptions) |
| Google Drive | Archive receipt originals with searchable naming |

## Default Categories
| Category | Examples |
|---|---|
| Food & Dining | Restaurants, groceries, coffee, delivery |
| Transportation | Gas, Uber/Lyft, parking, transit, flights |
| Shopping | Clothing, electronics, household items |
| Subscriptions | SaaS, streaming, gym, magazines |
| Office & Business | Supplies, software, coworking, printing |
| Entertainment | Movies, events, games, hobbies |
| Health | Pharmacy, doctor, insurance copays |
| Utilities | Phone, internet, electricity, water |
| Travel | Hotels, Airbnb, activities, travel meals |
| Other | Uncategorized expenses |

## Workflow A: Single Receipt Scan

### Step 1: Receive and Analyze Image
```javascript
// User uploads receipt image
// Use AI vision to extract:
var receiptData = {
  merchant: "...",
  date: "YYYY-MM-DD",
  subtotal: 0.00,
  tax: 0.00,
  tip: 0.00,
  total: 0.00,
  currency: "USD",
  paymentMethod: "...",  // Visa ending 4242, Cash, Apple Pay
  items: [
    { name: "...", quantity: 1, price: 0.00 }
  ],
  category: "...",  // Auto-categorized based on merchant
  confidence: 0.95  // How confident the extraction is
}
```

### Step 2: Confirm with User
Present extracted data for verification:
```
Receipt Scanned:
  Merchant: Trader Joe's
  Date: 2026-04-12
  Items: 8 items
  Subtotal: $47.23
  Tax: $3.78
  Total: $51.01
  Category: Food & Dining (Groceries)
  Payment: Visa ...4242

Correct? (or tell me what to fix)
```

### Step 3: Log to Google Sheets
```javascript
await google.sheets.appendValues({
  spreadsheetId: trackerSheetId,
  range: "'Expenses'!A1",
  values: [[
    receiptData.date,
    receiptData.merchant,
    receiptData.category,
    receiptData.total,
    receiptData.tax,
    receiptData.paymentMethod,
    receiptData.items.length + " items",
    "Auto-scanned"
  ]]
})
```

### Step 4: Archive Original
```javascript
// Upload original receipt to Google Drive
await google.drive.uploadFile({
  localPath: receiptImagePath,
  name: receiptData.date + " - " + receiptData.merchant + " - $" + receiptData.total + ".png",
  parents: [receiptsFolderId]
})
```

## Workflow B: Batch Receipt Processing
Process multiple receipts in one session:
1. User uploads 5-20 receipt images
2. AI processes each one sequentially
3. Present summary table of all extracted data
4. User confirms/corrects
5. Batch-append all to Google Sheets
6. Batch-upload originals to Drive

## Workflow C: Email Receipt Scanning
```javascript
// Search Gmail for purchase confirmations
var receipts = await google.gmail.listMessages({
  query: "from:(receipt OR invoice OR order OR confirmation) newer_than:30d has:attachment"
})
// Parse each email for: merchant, amount, date
// Cross-reference with existing tracker to avoid duplicates
```

## Workflow D: Monthly Summary Report
```javascript
// Read all expenses for the month
var monthData = await google.sheets.getValues({
  spreadsheetId: trackerSheetId,
  range: "'Expenses'!A:H"
})
// Calculate: total spend, by-category breakdown, daily average
// Compare to budget (if set)
// Generate summary with charts data
```

**Output format:**
```
## April 2026 Expense Summary

Total Spent: $2,847.33
Daily Average: $94.91
Top Category: Food & Dining ($892.40 - 31%)

| Category | Amount | % of Total | vs Budget |
|---|---|---|---|
| Food & Dining | $892.40 | 31% | +$42.40 over |
| Transportation | $445.20 | 16% | Under budget |
| Subscriptions | $389.00 | 14% | On track |
| Shopping | $367.50 | 13% | +$67.50 over |
| ...
```

## Expense Tracker Sheet Schema
```
Sheet 1: "Expenses" (raw data)
Columns: Date | Merchant | Category | Amount | Tax | Payment Method | Items | Source

Sheet 2: "Monthly Summary" (auto-calculated)
Columns: Month | Category | Total | % of Total | Budget | Variance

Sheet 3: "Settings"
Columns: Category | Monthly Budget | Notes
```

## Safety Rules
- **Never share financial data** outside the user's own Google account
- **Always confirm extracted amounts** before logging (money accuracy is critical)
- **Flag low-confidence extractions** (blurry images, handwritten receipts)
- **Gmail scanning requires explicit user approval** via requestApproval
- **No payment processing** - this is tracking only
