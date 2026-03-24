---
description: >
  Export a finished blog post markdown file to a Google Doc for team review.
  Use after generate-post, when you have a V0 draft ready.
argument-hint: <path to V0 draft file, e.g., drafts/time-delta-v0.md>
allowed-tools:
  - Read
  - Bash
  - mcp__google-workspace__docs_create
  - mcp__google-workspace__docs_find
  - mcp__google-workspace__drive_findFolder
  - mcp__google-workspace__docs_move
  - mcp__google-workspace__drive_search
---

# Export to Docs: Create a Google Doc from the V0 Draft

You are exporting a finished V0 blog post draft to Google Docs for the team review process.

## Steps

1. **Read the markdown file** from the path provided in `$ARGUMENTS`

2. **Extract the title** from the first `#` heading in the file

3. **Create a Google Doc** using `docs_create`:
   - Title: `[V0 DRAFT] <extracted title>`
   - Content: Pass the full markdown content — the Google Workspace MCP handles markdown-to-doc conversion

4. **Find the blog posts folder** using `drive_findFolder` with folder name "Drafts" (or similar). If not found, try searching for the parent folder using `drive_search`.
   - If a suitable folder is found, move the doc there using `docs_move`
   - If no folder is found, leave the doc in the root and inform the user

5. **Report back** to the user:
   - The Google Doc title
   - The Google Doc URL (construct from doc ID: `https://docs.google.com/document/d/<docId>/edit`)
   - Whether the doc was moved to a folder or left in root
   - Reminder: "This is a V0 draft. It should go through the team review process: V0 review by [reviewers], then V1 review by [approvers], then submission for translation."

## Notes

- Do not modify the markdown content before creating the doc
- If the doc creation fails, report the error clearly and suggest the user check their Google Workspace MCP configuration
- The doc will contain the raw markdown rendered by Google Docs — authors may need to adjust formatting for code blocks and figures
