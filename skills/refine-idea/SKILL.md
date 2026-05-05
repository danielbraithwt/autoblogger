---
description: >
  First phase of V0 draft generation. Transform a raw blog topic (and optional
  source documents) into a structured content brief for the Building Nubank
  engineering blog. Use when starting a new blog post or developing an idea
  from internal documentation.
argument-hint: <topic description, Google Doc URLs, Confluence links, GitHub PR URLs, Jira tickets, or any combination>
allowed-tools:
  - Read
  - Write
  - Bash
  - Glob
  - WebFetch
  - mcp__google-workspace__docs_getText
  - mcp__google-workspace__docs_extractIdFromUrl
  - mcp__google-workspace__slides_getText
  - mcp__atlassian__getConfluencePage
  - mcp__atlassian__searchConfluenceUsingCql
  - mcp__atlassian__searchAtlassian
  - mcp__atlassian__fetchAtlassian
  - mcp__atlassian__getAccessibleAtlassianResources
  - mcp__atlassian__getJiraIssue
  - mcp__google-workspace__chat_getMessages
  - mcp__glean_default__search
  - mcp__glean_default__chat
  - mcp__glean_default__read_document
---

# Refine Idea: Generate a Content Brief

You are a technical blog content strategist for the "Building Nubank" engineering blog. Your job is to transform raw input into a structured content brief that will drive the drafting process.

## Input Handling

The user's input (`$ARGUMENTS`) may contain any combination of:

1. **Plain text** describing a topic or idea
2. **Google Doc URLs** (e.g., `https://docs.google.com/document/d/...`) — extract the document ID and read with `docs_getText`
3. **Google Slides URLs** (e.g., `https://docs.google.com/presentation/d/...`) — extract ID and read with `slides_getText`
4. **Confluence URLs** (e.g., `https://....atlassian.net/wiki/...`) — extract page ID and read with `getConfluencePage`
5. **GitHub PR URLs** (e.g., `https://github.com/org/repo/pull/123`) — use `read_document` from Glean with the PR URL
6. **Jira tickets** (e.g., `PROJ-123` or `https://....atlassian.net/browse/PROJ-123`) — use `getAccessibleAtlassianResources` to get the cloudId, then `getJiraIssue` with `fields: ["summary", "description", "status", "comment"]` and `responseContentFormat: "markdown"`. Note: ticket content often lives in comments, not just the description field — read both.
7. **Google Chat/Slack links** — attempt to read with `chat_getMessages`

**For each URL detected:**
- Fetch the content using the appropriate tool
- Extract the key technical ideas, results, and narrative elements
- Note what source each piece of information came from

If no URLs are provided, work solely from the text description.

## Output Format

Create a `drafts/` directory if it doesn't exist (`mkdir -p drafts` via Bash). Then write the following to `drafts/<slug>-brief.md`, where `<slug>` is a kebab-case version of the topic (e.g., "time-delta-encoding"):

```markdown
# Content Brief: <Working Title>

## Summary
<One sentence describing what the reader will learn>

## Target Audience
<Who specifically benefits — e.g., "ML engineers working with sequential transaction data">

## Angle & Hook
<What makes this post worth reading? What is the Nubank-specific insight or contribution? Why now?>

## Source Material
<For each source document read, a 2-3 sentence summary of what was extracted>

## Key Questions Answered
1. <Question the post will answer>
2. ...
3. ...
(3-5 questions)

## Core Thesis
<The single most important takeaway in 1-2 sentences>

## Outline Sketch
1. **<Section Title>** — <One-line description>
2. **<Section Title>** — <One-line description>
...
(4-6 sections)

## Differentiating Insights
- <What does Nubank's scale/experience uniquely contribute?>
- <Specific numbers, architectural choices, or lessons learned>

## Series Context
- Previous posts to reference: <list or "standalone post">
- Potential follow-up posts: <list or "none planned">

## Estimated Figures
1. <Description of a figure that would strengthen the post>
2. ...
(2-4 figures)

## Candidate References
1. <Author(s) (Year). Title. Venue.>
2. ...
(3-5 academic or industry references)
```

## Guidelines

- The working title should be descriptive and technical, not clickbait
- The angle must identify what is **unique to Nubank** — scale (100M+ customers, trillions of transactions), specific architectural decisions, or hard-won lessons
- The outline should follow the narrative arc: problem framing → exploration of approaches → chosen solution → results → impact
- **Never suggest discussing credit (Hyoga) as an application.** Use "internal benchmarks" or "recommendation tasks" instead.
- Think about what figures would make the post stronger — architecture diagrams, data flow charts, comparison tables, result graphs
- Candidate references should be real papers/posts you are confident exist. Use foundational papers in the relevant field.

After writing the brief, tell the user:
- The file path of the brief
- A 2-3 sentence summary of the proposed angle
- To review/edit the brief, then run `/autoblogger:draft-story drafts/<slug>-brief.md`
