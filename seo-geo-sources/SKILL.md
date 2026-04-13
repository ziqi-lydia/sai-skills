---
name: seo-geo-sources
description: SEO blog research workstation. Automates pre-writing source collection - AI visibility audits, competitor article analysis, user question mining, expert quote gathering, social media trend scanning, and data/statistics harvesting. Outputs structured Blog Writing Resources to Google Sheets.
---

# SEO/GEO Blog Sources Collector

The research phase of blog production. Before writing any SEO/GEO article, this skill systematically collects all the raw materials: competitive intelligence, user questions, expert quotes, hard data, social proof, and trend signals. Outputs everything to a structured Google Sheets template ready for the writing phase.

## When to Use
- "Research sources for a blog about [keyword]"
- "Prepare blog writing resources for [topic]"
- "Do a competitive analysis for [keyword] before I write"
- "Find data, quotes, and user questions about [topic]"

## Required Context (ask if not provided)
- **Primary keyword(s)**: The target search query
- **Blog title** (if decided): Or the skill helps refine it
- **Target audience**: Who is this article for?
- **Output sheet**: Existing Blog Writing Resources sheet, or create new

## Platform Integrations
| Platform | Usage |
|---|---|
| Google Search | SERP analysis, competitor articles, PAA questions |
| ChatGPT (Browser) | AI visibility check - what does ChatGPT cite? |
| Perplexity (Browser) | AI visibility check - what does Perplexity cite? |
| AnswerSocrates | User question mining |
| Twitter/X (Browser) | Viral content, expert opinions, trend signals |
| Reddit (Browser) | User pain points, authentic discussions |
| YouTube (Browser) | Video content trends, title format research |
| LinkedIn (Browser) | Expert quotes, industry insights |
| Google Sheets | Output: Blog Writing Resources template |
| Google Docs (optional) | Full research brief export |

## Default Output Sheet
**Spreadsheet ID**: `1imXTbVhmDlg23ao9695Py1RZYnWxH3qyAiquSxvflxo`

## Safety Rules
- **Read-only on all platforms** - no posting, commenting, or engaging
- **Wait 5-8 seconds between page loads** on social platforms
- **Maximum 20 Google searches per session**
- **Maximum 10 social media searches per platform per session**
- **Respect robots.txt** - don't scrape blocked pages

## Research Pipeline (6 Modules)

### Module 1: AI Visibility Audit

Check if the topic/brand is cited by AI engines:

```javascript
// 1a. ChatGPT
var page = await browser.newtab("https://chatgpt.com")
await page.wait({ waitTime: 3 })
// Ask: "What are the best [keyword] tools/solutions in 2026?"
// Record: which products/brands are cited, verbatim

// 1b. Perplexity
await page.goto({ url: "https://perplexity.ai" })
// Ask same query, record citations with source URLs

// 1c. Google AI Overview
// Search keyword on Google, check AI Overview panel
// Record what's featured
```

**Output fields:**
- AI engines checked (ChatGPT, Perplexity, Google AI)
- Products/brands cited by each
- Is Sai/Simular mentioned? (Y/N)
- Citation gap analysis (what Sai does that nobody mentions)

### Module 2: User Question Mining

Collect real questions people ask about this topic:

```javascript
// 2a. AnswerSocrates
await page.goto({ url: "https://answersocrates.com/" })
// Enter keyword, collect question clusters
// Target: 15-20 questions organized by type (what, how, why, which, can)

// 2b. Google PAA (People Also Ask)
// From SERP, expand PAA boxes
// Collect 8-10 additional questions

// 2c. Reddit search
await page.goto({ url: "https://www.reddit.com/search/?q=" + encodeURIComponent(keyword) + "&sort=relevance&t=year" })
// Find top 5 discussion threads
// Extract actual user questions and pain points from comments
```

**Output fields:**
- 15-20 user questions (become H2 heading candidates)
- Pain points from Reddit (authentic user frustrations)
- Question categories (definition, comparison, how-to, troubleshooting)

### Module 3: Competitor Article Analysis

Deep-dive top SERP results:

```javascript
// Search Google for the keyword
// Open top 5 organic results
for (var i = 0; i < 5; i++) {
  // For each competitor article, extract:
  var analysis = {
    url: "...",
    title: "...",
    wordCount: 0,       // estimate from page length
    h2Headings: [],     // all section headings
    toolsCovered: [],   // which products they review
    hasComparisonTable: false,
    hasFAQ: false,
    hasMethodology: false,
    uniqueAngles: [],   // what makes this article different
    weaknesses: []      // what's missing that we can cover
  }
}
```

**Output fields:**
- Top 5 competitor URLs with titles
- Common H2 patterns across competitors
- Tools/products every competitor mentions
- Content gaps (what nobody covers - our opportunity)
- Format analysis (table? FAQ? video embed? word count?)

### Module 4: Data & Statistics Harvesting

Find hard numbers to cite in the article:

```javascript
// Search for industry reports and statistics
var dataQueries = [
  keyword + " statistics 2025 2026",
  keyword + " market size report",
  keyword + " survey results",
  keyword + " benchmark data"
]
// For each query, find 3-5 citable data points
// Record: the statistic, the source, the year, the URL
```

**Output fields:**
- 8-12 citable statistics with sources
- Market size / growth data
- User adoption / usage numbers
- ROI / efficiency improvement data
- Source credibility rating (Gartner/Forrester = high, blog = low)

### Module 5: Expert Quote Collection

Find quotable opinions from industry experts:

```javascript
// 5a. Twitter/X search for expert opinions
await page.goto({ url: "https://x.com/search?q=" + encodeURIComponent(keyword) + "&f=top" })
// Find tweets from verified/high-follower accounts
// Record: name, credentials, quote, engagement metrics

// 5b. LinkedIn search for thought leaders
await page.goto({ url: "https://www.linkedin.com/search/results/content/?keywords=" + encodeURIComponent(keyword) })
// Find posts from industry leaders
// Record: name, title, company, key insight
```

**Output fields:**
- 5-8 expert quotes with full attribution
- Expert name, title, company, credentials
- The quote (verbatim or paraphrased)
- Source link (tweet/post URL)

### Module 6: Social Media Trend Scan

Identify viral content and trending angles:

```javascript
// 6a. Twitter/X - viral posts (past 3 months)
// Search keyword + variations, sort by engagement
// Record: hook formula, engagement metrics, content angle

// 6b. YouTube - top videos
await page.goto({ url: "https://www.youtube.com/results?search_query=" + encodeURIComponent(keyword) + "&sp=CAMSAhAB" })
// Sort by view count, analyze top 5 video titles
// Record: title format, view count, publish date

// 6c. Reddit - trending discussions
// Find most upvoted threads in past 3 months
// Record: topic angle, sentiment, common complaints
```

**Output fields:**
- Top 5 viral social posts with engagement data
- Viral hook formulas that worked
- Trending content angles
- Audience sentiment summary
- YouTube title patterns that get views

## Output Format

### Google Sheets Structure
```
Row for each keyword researched:
Column A: Target Audience / Segment
Column B: Primary Keywords
Column C: AI Visibility Report
Column D: User Questions (AnswerSocrates + PAA + Reddit)
Column E: SERP Intent Classification
Column F: Top Competitor Articles (URLs + analysis)
Column G: Statistics & Data Points
Column H: Expert Quotes
Column I: Social Media Trends
Column J: Recommended Blog Title
Column K: Recommended H2 Structure
Column L: Content Gaps (our unique angle)
```

### Optional: Research Brief (Google Doc)
For complex articles, export a full research brief document:
```javascript
var brief = await google.docs.createDocument({
  title: "Research Brief: " + keyword + " - " + new Date().toISOString().slice(0,10)
})
// Insert all 6 modules' findings in a structured document
```

## Quality Checks
Before marking research complete:
- [ ] AI visibility checked on 3+ engines
- [ ] 15+ user questions collected
- [ ] 5+ competitor articles analyzed
- [ ] 8+ data points with sources
- [ ] 5+ expert quotes with attribution
- [ ] Social trends from 3+ platforms
- [ ] Content gaps identified (unique angle for our article)
- [ ] All data recorded in the output sheet
