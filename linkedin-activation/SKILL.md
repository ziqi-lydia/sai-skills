---
name: linkedin-activation
description: Boost LinkedIn visibility by engaging with posts from target people (hiring managers, recruiters, industry leaders). Leaves thoughtful, non-generic comments to build recognition before outreach.
---


# LinkedIn Activation (Engagement for Visibility)

Increase a user's LinkedIn visibility by strategically commenting on posts from hiring managers, recruiters, or industry contacts at target companies. The goal is name recognition before direct outreach.

## When to Use
- "Find posts from hiring managers at [companies] and comment on them"
- "Help me engage with recruiters at my target companies"
- "I want to be more visible to people at [Company] before I apply"
- "Comment on 10 posts from people in [industry] today"

## Required Context (ask if not provided)
- **Target companies or people**: Who should we engage with?
- **User's expertise areas**: So comments reflect genuine knowledge
- **User's voice/tone**: Casual? Professional? Technical? (default: smart-casual, direct)
- **Volume**: How many posts to comment on (default: 10)

## Safety Rules
- **Maximum 15 comments per session**. More than that risks looking spammy.
- **Wait 2-5 minutes between comments** to appear natural.
- **NEVER post generic comments** like "Great post!" or "Thanks for sharing!" or "This is so insightful!" These hurt more than they help.
- **Always show proposed comments to user for approval** before posting.
- **Don't comment on posts older than 48 hours** (low visibility, looks odd).
- **Vary comment length and style** across posts.

## Workflow

### Step 1: Find Target Posts

Search for recent posts from people at target companies:
```javascript
var page = await browser.newtab("https://www.linkedin.com/search/results/content/?keywords=" + encodeURIComponent("[company or person name]") + "&datePosted=%22past-24h%22&sortBy=%22date_posted%22")
await page.wait({ waitTime: 3 })
var snap = await page.snapshot()
```

Alternative approach: Visit target people's profiles directly and find their recent posts:
```javascript
await page.goto({ url: "https://www.linkedin.com/in/[slug]/recent-activity/all/" })
```

### Step 2: Evaluate Posts for Engagement Value
For each post found, assess:
- **Relevance**: Is the topic something user can genuinely comment on?
- **Recency**: Posted within 24-48 hours?
- **Engagement potential**: Does the post invite discussion?
- **Poster's relevance**: Is this person useful to user's job search / network?

Skip posts that are:
- Pure promotional content with no substance
- Reposted content (engage with originals instead)
- Celebratory posts where genuine comment is hard ("Excited to announce...")
- Posts with 500+ comments already (user's comment will be buried)

### Step 3: Draft Comments
Each comment should follow ONE of these patterns (vary across posts):

**Pattern A: Add a data point or example**
"This resonates. At [previous company], we saw [similar result] when we [specific action]. The key difference was [insight]."

**Pattern B: Ask a thoughtful question**
"Interesting take on [topic]. Curious how you think about [related angle]? I've been thinking about this in the context of [user's domain]."

**Pattern C: Respectful disagreement or nuance**
"Mostly agree, but I'd push back slightly on [point]. In my experience with [domain], [alternative perspective]. Would love to hear if you've seen that too."

**Pattern D: Connect dots the poster didn't**
"This connects to something I've been noticing in [adjacent area]. [Brief insight]. Wonder if that's a trend or just my bubble."

**Comment rules:**
- 2-5 sentences (not one-liners, not essays)
- Reference something SPECIFIC from the post (prove you read it)
- Add genuine value (your own experience, a question, a different angle)
- Match the user's natural voice and expertise level
- NO hashtags in comments (looks try-hard)
- NO self-promotion (don't mention your product/company unless directly relevant)

### Step 4: Present for Approval
```
## Proposed Comments (10 posts)

1. **[Poster Name]** ([Role] at [Company]) - Posted 3 hours ago
   Topic: [brief summary]
   Post: "[first 100 chars of post]..."
   Comment: "[your drafted comment]"

2. **[Poster Name]** ...
```

### Step 5: Post Comments with Spacing

After user approves:
```javascript
for (var i = 0; i < approvedComments.length; i++) {
  // Navigate to the post
  await page.goto({ url: posts[i].url })
  await page.wait({ waitTime: 3 })
  var snap = await page.snapshot()
  
  // Find comment box, click it
  // Type the comment
  // Click Post/Submit
  
  // Wait 2-5 minutes between comments
  var delay = 120 + Math.floor(Math.random() * 180)
  console.log("Waiting " + Math.round(delay/60) + " minutes before next comment...")
  await page.wait({ waitTime: delay })
  
  console.log("Posted " + (i+1) + "/" + approvedComments.length)
}
```

### Step 6: Report Results
```
## Engagement Summary

- Comments posted: 10/10
- Companies covered: Stripe (3), Anthropic (3), Notion (2), Figma (2)
- People engaged: 4 hiring managers, 3 recruiters, 3 team leads
- Posts engaged: 7 original posts, 3 article shares

Recommended next step: Send connection requests to these people in 2-3 days, after they've likely seen your comment.
```

## LinkedIn UI Notes
- Comment box appears after clicking "Comment" icon below a post
- After typing, look for "Post" or "Submit" button (may vary by UI version)
- If post is in feed view, might need to click into full post view first
- Some posts have comments collapsed; click "Load more comments" to verify yours posted
