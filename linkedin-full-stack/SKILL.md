---
name: linkedin-full-stack
description: LinkedIn full-stack automation orchestrator. Unifies prospecting, outreach, content creation, profile optimization, and engagement activation into a single intelligent workflow with cross-skill pipeline tracking.
---

# LinkedIn Full-Stack Automation

The master orchestrator for LinkedIn growth. Instead of running individual skills separately, this skill intelligently routes your request to the right sub-skill (or combination), manages the pipeline across all activities, and provides a unified performance dashboard.

## When to Use
- "Help me grow my LinkedIn presence" (routes to profile-optimizer + creator-content + activation)
- "I need to find leads and reach out to them" (routes to prospecting + activation + outreach)
- "Run my full LinkedIn routine today" (orchestrates all sub-skills in optimal order)
- "Show me my LinkedIn pipeline status" (pulls data from all tracking sheets)

## Sub-Skills Orchestrated
| Sub-Skill | Function | When Triggered |
|---|---|---|
| linkedin-b2b-prospecting | Find and build prospect lists by ICP | Lead gen requests |
| linkedin-b2b-outreach | Personalized DM sequences + follow-ups | Outreach requests |
| linkedin-creator-content | Draft and schedule posts | Content requests |
| linkedin-profile-optimizer | Headline, summary, skills audit | Profile improvement |
| linkedin-activation | Strategic commenting for visibility | Engagement/warming |

## Platform Integrations
| Platform | Usage |
|---|---|
| LinkedIn (Browser) | All sub-skill operations |
| Google Sheets | Unified pipeline tracker / dashboard |
| Google Calendar | Content scheduling, follow-up reminders |
| Gmail | Cross-platform follow-up (LinkedIn + email) |

## Workflow A: Daily LinkedIn Routine (30-45 min)

### Step 1: Morning Dashboard Check (2 min)
```javascript
// Pull data from all tracking sheets
var dashboard = {
  newConnections: 0,
  pendingFollowUps: 0,
  postsScheduledToday: 0,
  unreadMessages: 0,
  engagementScore: 0
}
```

### Step 2: Engagement First (15 min)
Trigger `linkedin-activation`:
- Check inner circle for new posts (like + comment)
- Scan feed for high-value engagement opportunities
- Draft comments for user approval

### Step 3: Content Check (5 min)
Trigger `linkedin-creator-content`:
- Is there a post scheduled for today?
- If yes, remind user or post directly
- If no, suggest a topic based on trending conversations

### Step 4: Outreach Follow-ups (10 min)
Trigger `linkedin-b2b-outreach`:
- Check for prospects who viewed/replied since last session
- Queue follow-up messages for non-responders (5+ days)
- Present new connection accepts for first-touch DMs

### Step 5: Pipeline Update (3 min)
Update the unified Google Sheet with today's activity.

## Workflow B: Full Funnel Campaign

Execute a complete prospect-to-customer pipeline:

1. **Week 1**: `linkedin-b2b-prospecting` - Build target list (50 prospects)
2. **Week 1-2**: `linkedin-activation` - Warm up by engaging with prospects' posts
3. **Week 2**: Send connection requests (no pitch in note)
4. **Week 2-3**: `linkedin-b2b-outreach` - First-touch DMs to accepted connections
5. **Week 3-4**: Follow-up sequence (3 touches max)
6. **Ongoing**: `linkedin-creator-content` - Post relevant content to build credibility

## Workflow C: Creator Growth Mode

For users focused on building audience rather than sales:

1. `linkedin-profile-optimizer` - Optimize profile for target audience
2. `linkedin-creator-content` - 5 posts/week content calendar
3. `linkedin-activation` - 2x daily engagement with target accounts
4. Weekly analytics review from dashboard

## Unified Dashboard Schema
```javascript
// Google Sheet: "LinkedIn Dashboard"
// Sheet 1: Pipeline Overview
// Columns: Date | New Prospects | Connection Requests Sent | Accepted | DMs Sent | Replies | Meetings
// Sheet 2: Content Performance
// Columns: Date | Post Topic | Impressions | Reactions | Comments | Profile Views
// Sheet 3: Engagement Log
// Columns: Date | Person | Action (Comment/Like/DM) | Context | Response
```

## Smart Routing Logic
When user gives an ambiguous request:
- Contains "find", "list", "prospect", "ICP" -> prospecting
- Contains "message", "DM", "outreach", "follow up" -> outreach
- Contains "post", "write", "draft", "content" -> creator-content
- Contains "profile", "headline", "summary", "optimize" -> profile-optimizer
- Contains "engage", "comment", "visibility", "activate" -> activation
- Contains "routine", "daily", "everything" -> full routine (Workflow A)

## Safety Rules
- Aggregate all sub-skill safety limits (don't exceed any individual skill's limits)
- Maximum total LinkedIn actions per day: 50 (across all sub-skills)
- Always require user approval for messages and comments
- Log all actions to the unified dashboard
