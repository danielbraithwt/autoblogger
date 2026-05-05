---
description: >
  End-to-end blog post generation. Runs all 4 phases automatically: refine idea,
  draft story arc, generate full post, and export to Google Docs. Use when you
  want a complete V0 draft from a topic or source documents in one go.
argument-hint: "[optional] topic description and/or source URLs"
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
  - mcp__google-workspace__docs_create
  - mcp__google-workspace__drive_search
  - AskUserQuestion
  - mcp__plugin_slack_slack__slack_search_public
  - mcp__plugin_slack_slack__slack_read_channel
  - mcp__plugin_slack_slack__slack_read_thread
  - mcp__plugin_slack_slack__slack_search_channels
  - mcp__plugin_slack_slack__slack_search_users
  - mcp__glean_default__search
  - mcp__glean_default__chat
  - mcp__glean_default__read_document
---

# Write Post: End-to-End V0 Draft Generation

You are generating a complete V0 blog post for the "Building Nubank" engineering blog. This runs all four phases of the pipeline automatically, writing intermediate artifacts at each step.

Read the style guide from `style-guide.md` in the `generate-post` skill directory (at `skills/generate-post/style-guide.md` relative to the plugin root) before starting. Read `examples/example-post.md` (relative to the plugin root) as a tone and style reference. Match the voice, rhythm, and technical depth of this example.

## Phase 0: Gather Input & Shape the Story

Read the interview template from `skills/write-post/interview-template.md` (relative to the plugin root) at the start of this phase. This template is your **internal guide only** — never show section names, numbers, or the template structure to the user.

### Step 1: Greeting & source collection

If `$ARGUMENTS` already contains a topic description and/or URLs, skip to Step 2.

If `$ARGUMENTS` is empty or missing both a topic and source links, greet the user and ask them to describe the post. Say something like:

> Hey! Let's write a blog post. Tell me what it's about — the problem, what you built, and how it turned out. If you have source material (Google Docs, Confluence pages, GitHub PRs, Slides, Slack threads, etc.), paste the links too.

**Stop and wait for the user to reply.**

### Step 2: Read sources & guide the conversation

1. Parse the user's input (from `$ARGUMENTS` or Step 1) and detect any URLs
2. For each URL, fetch content using the appropriate tool:
   - Google Docs: extract ID with `docs_extractIdFromUrl`, read with `docs_getText`
   - Google Slides: read with `slides_getText`
   - Confluence: read with `getConfluencePage` (use `getAccessibleAtlassianResources` first if needed for cloudId)
   - GitHub PRs: use `read_document` from Glean with the PR URL
   - Jira tickets (e.g., `PROJ-123` or `https://....atlassian.net/browse/PROJ-123`): use `getAccessibleAtlassianResources` to get the cloudId, then `getJiraIssue` with `fields: ["summary", "description", "status", "comment"]` and `responseContentFormat: "markdown"`. Note: ticket content often lives in comments, not just the description field — read both.
   - Slack: use `slack_search_channels` to find the channel, then `slack_read_channel` or `slack_read_thread` to fetch messages. For Slack search queries, use `slack_search_public`.
   - Chat: `chat_getMessages`
3. Review the fetched content and the user's description against the interview template internally. Identify which areas are well-covered and which have gaps.
4. Ask natural follow-up questions to fill gaps. **Do NOT present the template or reference section names.** Instead of "Let's fill in The Trigger section", ask something like "What was the moment you realized the old approach wasn't going to cut it?" Adapt your questions:
   - If the source material is rich and covers most template areas, ask only about the gaps (e.g. learnings, trade-offs, or what made this hard at Nubank's scale)
   - If the user is starting from a vague idea, ask more questions to draw out the full story
   - Group related questions naturally — don't interrogate one topic at a time
   - Skip anything the sources already answer clearly
5. **Stop and wait** for the user's response. Continue the conversation until you have enough material to cover the template's key areas (or the user has made clear they want a different structure).

### Step 3: Synthesize a story pitch

Once you have enough material, synthesize everything into a short story pitch in plain language:

- Proposed angle and hook (what makes this interesting)
- The narrative arc: what triggered the work, what made it hard, how the team solved it, what they learned, and the impact
- Any open questions or gaps that the draft will need to work around

Present it to the user and ask if this direction feels right. The user can adjust, reorder, or take the story in a completely different direction.

**Stop and wait for confirmation before proceeding to Phase 1.**

## Phase 1: Refine Idea

**Input:** The story pitch confirmed in Phase 0, plus all source material already fetched.

1. Synthesize the confirmed story pitch and all source content into a structured content brief
2. Create `drafts/` directory if needed (`mkdir -p drafts` via Bash)
3. Write to `drafts/<slug>-brief.md`

The brief must include: working title, summary, target audience, angle/hook, source material summary, key questions (3-5), core thesis, outline sketch (4-6 sections), differentiating insights, series context, estimated figures (2-4), candidate references (3-5).

**Tell the user:** "Phase 1 complete: Content brief written to drafts/<slug>-brief.md"

## Phase 2: Draft Story

1. Read the brief from `drafts/<slug>-brief.md`
2. Expand into a detailed narrative outline with:
   - Narrative arc: problem → approaches → solution → results → impact
   - Per section: heading, 3-5 bullet points, figure placement, citation needs, transition
   - Word budget per section (must total 1000-1500)
3. Write to `drafts/<slug>-outline.md`

**Tell the user:** "Phase 2 complete: Narrative outline written to drafts/<slug>-outline.md"

### Story Review Checkpoint

Before moving on, present the outline to the user in plain language:

1. Summarize the proposed narrative arc (2-3 sentences)
2. List each section with its angle and what it covers
3. Call out the hook, key insight, and how the post ends
4. Mention any decisions you made (e.g. which approach comparison to lead with, what to cut for word budget)

Then ask:

> Does this story direction look right? Feel free to suggest changes: reorder sections, shift the angle, add or drop topics, change the hook, etc. Or say "looks good" to proceed.

**Stop and wait for the user to reply.** Do NOT proceed to Phase 3 until the user confirms or requests changes. If the user requests changes, revise the outline file, re-present the updated summary, and wait again.

## Phase 3: Generate Post

1. Read the outline from `drafts/<slug>-outline.md`
2. Read the style guide from `skills/generate-post/style-guide.md`
3. Write the complete post section by section following the outline and style guide
4. Apply the Progressive Explanation Pattern in every section
5. Insert `[FIGURE: ...]` placeholders with detailed descriptions
6. Add numbered citations and build the References section
7. **Lint pass** — review the complete draft against the "AI Writing Patterns to Avoid" section of the style guide:
   a. Search for em-dashes, flagged words ("delve", "leverage", "utilize", etc.), and rewrite them
   b. Search for significance inflation (trailing importance clauses, "pivotal", "crucial development") and remove or rewrite
   c. Search for hedging preambles ("It's important to note", "Needless to say") and cut them
   d. Check that the author's specific technical terms from the outline haven't been genericized
   e. Check paragraph length variation — if 3+ consecutive paragraphs have the same sentence count, vary them
   f. Check for boldfaced inline headers in body text and convert to normal prose or subheadings
8. Validate: word count 1000-1500, figures present, all claims cited, "we" voice throughout
9. Write to `drafts/<slug>-v0.md`

**Tell the user:** "Phase 3 complete: V0 draft written to drafts/<slug>-v0.md"

## Phase 4: Export to Docs

1. Read the V0 draft from `drafts/<slug>-v0.md`
2. Extract the title from the first `#` heading
3. Create a Google Doc titled `[V0 DRAFT] <title>` with the markdown content
4. Report the Google Doc URL

**Tell the user:** "Phase 4 complete: Google Doc created"

## Final Summary

After all four phases, print:

```
--- Autoblogger: V0 Draft Complete ---

Files created:
  - drafts/<slug>-brief.md    (content brief)
  - drafts/<slug>-outline.md  (narrative outline)
  - drafts/<slug>-v0.md       (full V0 draft)

Draft stats:
  - Word count: <N> words
  - Sections: <N> sections
  - Figures: <N> figure placeholders
  - Citations: <N> references

Google Doc: <URL>

Next steps:
  1. Review the V0 draft and make edits
  2. Submit for V0 review by the ML team
  3. After review, revise to V1 for approval
  4. Submit for translation (PT-BR, ES)
```

## Important Rules

- **Never discuss credit (Hyoga) as an application.** Use "internal benchmarks" or "recommendation tasks."
- **Word count must be 1000-1500 words** for the final post
- **Use "we" voice** throughout — never "I"
- **Every major section needs a figure placeholder**
- **All technical claims need citations**
- If any phase encounters an error, report it clearly and attempt to continue with the remaining phases
- **Do not re-run Phase 3 on a draft the user has already manually edited.** Once the user has revised the V0 draft with their own voice and edits, regenerating it will overwrite their work and flatten their voice into AI-default patterns (semantic ablation). If the user wants to regenerate, confirm they understand this will replace their edits.
