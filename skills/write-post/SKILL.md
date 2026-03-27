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

Read the style guide from `style-guide.md` in the `generate-post` skill directory (at `skills/generate-post/style-guide.md` relative to the plugin root) before starting. Optionally read `examples/example-post.md` for tone calibration.

## Phase 0: Gather Input

Check `$ARGUMENTS` for content. If `$ARGUMENTS` already contains a topic description and/or URLs, skip this phase and proceed directly to Phase 1.

If `$ARGUMENTS` is empty or missing both a topic and source links, greet the user and ask them to describe the post in plain text. Say something like:

> Hey! Let's write a blog post. Tell me what it's about — describe the topic, the problem, and any key results. If you have source material (Google Docs, Confluence pages, GitHub PRs, Slides, etc.), paste the links here too. You can send everything in one message or we can go back and forth.

Then **stop and wait for the user to reply**. Do NOT proceed to Phase 1 until you have a topic description from the user. Once the user responds, use their message as the input for Phase 1. If they didn't include source links, that's fine — just proceed without them.

## Phase 1: Refine Idea

**Input:** `$ARGUMENTS` (or the combined input from Phase 0) — can contain plain text topic descriptions, Google Doc/Slides URLs, Confluence URLs, GitHub PR URLs, Slack links, or Chat message links.

1. Parse the arguments and detect any URLs
2. For each URL, fetch content using the appropriate tool:
   - Google Docs: extract ID with `docs_extractIdFromUrl`, read with `docs_getText`
   - Google Slides: read with `slides_getText`
   - Confluence: read with `getConfluencePage` (use `getAccessibleAtlassianResources` first if needed for cloudId)
   - GitHub PRs: `gh pr view <url>` via Bash
   - Slack: use `slack_search_channels` to find the channel, then `slack_read_channel` or `slack_read_thread` to fetch messages. For Slack search queries, use `slack_search_public`.
   - Chat: `chat_getMessages`
3. Synthesize all source material into a structured content brief
4. If after reading the sources there are important ambiguities — e.g. multiple possible angles, unclear target audience, or missing context that would significantly change the post — use `AskUserQuestion` to clarify before writing the brief. Don't ask about minor details; use your judgment on what materially affects the draft.
5. Create `drafts/` directory if needed (`mkdir -p drafts` via Bash)
6. Write to `drafts/<slug>-brief.md`

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

## Phase 3: Generate Post

1. Read the outline from `drafts/<slug>-outline.md`
2. Read the style guide from `skills/generate-post/style-guide.md`
3. Write the complete post section by section following the outline and style guide
4. Apply the Progressive Explanation Pattern in every section
5. Insert `[FIGURE: ...]` placeholders with detailed descriptions
6. Add numbered citations and build the References section
7. Validate: word count 1000-1500, figures present, all claims cited, "we" voice throughout
8. Write to `drafts/<slug>-v0.md`

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
