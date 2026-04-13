---
name: linkedin-b2b-prospecting
description: Build targeted B2B prospect lists from LinkedIn - search by ICP criteria (role, company size, industry, location), enrich profiles, cross-reference with account lists in Google Sheets. Export structured prospect lists.
---

# LinkedIn B2B Prospecting

Build targeted prospect lists for B2B sales and marketing teams based on Ideal Customer Profile (ICP) criteria. Search LinkedIn, enrich profiles, cross-reference with existing account lists, and export structured data to Google Sheets.

## When to Use
- "Build a list of 30 [roles] at [industry/company type]"
- "Find decision makers who posted about [topic] recently"
- "I have a spreadsheet of target accounts, find the right contact at each"
- "Find [roles] at Series A-C startups"

## Required Context (ask if not provided)
- **Target role(s)**: VP Marketing, Head of Growth, CMO, etc.
- **Company criteria**: Industry, size (headcount or funding stage), location
- **Volume**: How many prospects (default: 25)
- **Prioritization**: Active posters? Recently changed roles? Specific skills?
- **Export format**: Google Sheet or in-chat table

## Platform Integrations
| Platform | Usage |
|---|---|
| LinkedIn (Browser) | Profile search, enrichment, activity scanning |
| Google Sheets | Export prospect lists, cross-reference account lists |

## Safety Rules
- **Maximum 50 profile visits per session.** Wait 10+ seconds between profile visits.
- **Maximum 3 search result pages per query.** Don't paginate aggressively.
- **Wait 5+ seconds between search page loads.**
- **Stop immediately** on any rate-limit warning or CAPTCHA.
- **Read-only** - this skill does NOT send connection requests or messages.

## Workflow A: ICP-Based List Building

### Step 1: Construct Search Queries
Build LinkedIn search URLs from user's ICP:
```javascript
var query = encodeURIComponent("[target role] [industry]")
var page = await browser.newtab("https://www.linkedin.com/search/results/people/?keywords=" + query)
await page.wait({ waitTime: 3 })
var snap = await page.snapshot()
```

For advanced filtering, use LinkedIn's filter UI:
- Click "All filters"
- Set: Locations, Current company, Industry, Company headcount
- Apply and parse results

### Step 2: Parse Search Results
From each results page (10 results per page):
```javascript
var prospects = []
// For each result, extract:
// { name, headline, location, currentCompany, profileUrl, connectionDegree, mutualConnections }
```

Paginate carefully (max 3 pages):
```javascript
var pagesNeeded = Math.min(Math.ceil(targetCount / 10), 3)
for (var p = 0; p < pagesNeeded; p++) {
  var snap = await page.snapshot()
  // Parse results, click Next
  await page.wait({ waitTime: 5 })
}
```

### Step 3: Enrich Top Prospects
For users who want detailed info, visit individual profiles:
```javascript
for (var i = 0; i < prospects.length; i++) {
  await page.goto({ url: prospects[i].profileUrl })
  await page.wait({ waitTime: 10 + Math.floor(Math.random() * 5) })
  var profileSnap = await page.snapshot()
  prospects[i].enriched = {
    currentTitle: "...",
    companySize: "...",
    experience: "...",
    recentPosts: "...",
    skills: [...],
    education: "..."
  }
}
```

### Step 4: Filter for Active Posters (if requested)
- Check each prospect's recent activity tab
- Note last post date
- Flag as "Active" (posted in last 30 days) or "Inactive"

### Step 5: Export to Google Sheets
```javascript
var sheet = await google.sheets.createSpreadsheet({
  title: "B2B Prospects - [Role] - " + new Date().toISOString().slice(0,10),
  sheets: ["Prospects"]
})
var header = [["Name", "Title", "Company", "Company Size", "Industry", "Location", "LinkedIn URL", "Active Poster", "Connection Degree", "Mutual Connections", "Notes"]]
var rows = prospects.map(p => [p.name, p.headline, p.company, p.companySize || "", p.industry || "", p.location, p.profileUrl, p.isActive ? "Yes" : "No", p.connectionDegree, p.mutualConnections || 0, ""])
await google.sheets.updateValues({ spreadsheetId: sheet.id, range: "'Prospects'!A1:K" + (rows.length + 1), values: header.concat(rows) })
```

## Workflow B: Topic-Filtered Prospecting
Search for people who posted about specific topics, then filter by ICP criteria.

## Workflow C: Account List Enrichment
User provides a spreadsheet of target companies; skill finds the right decision-maker at each company.

## De-duplication
- Track all profile URLs seen across searches
- Flag prospects appearing in multiple queries (stronger ICP fit signal)

## Output Quality Checks
- Verify each prospect's role matches the target
- Verify location matches (if specified)
- Remove obviously wrong matches
- Flag profiles with limited visibility
