# autoblogger

A Claude Code plugin for generating V0 (initial draft) blog posts for the **Building Nubank** engineering blog.

> **V0 = initial draft.** All output requires human review and editing before publication. The plugin generates a starting point, not a finished product.

## Installation

**Local development:**
```bash
claude --plugin-dir /path/to/autoblogger
```

**From a marketplace (once published):**
```bash
/plugin marketplace add <org>/<repo>
/plugin install autoblogger@<marketplace-name>
```

## Prerequisites

The following MCP servers must be configured in your environment:

- **Google Workspace MCP** — for reading Google Docs/Slides and exporting to Google Docs
- **Atlassian MCP** — for reading Confluence pages (optional)

## Skills

### `/autoblogger:write-post` (end-to-end)

Runs the entire pipeline in one shot:

```
/autoblogger:write-post "How we use time deltas to improve transaction model generalization"
```

Accepts plain text topics, Google Doc URLs, Confluence links, GitHub PR URLs, or any combination. Produces all intermediate files and exports a Google Doc.

### Step-by-step skills

For more control, run each phase individually:

| Skill | Input | Output |
|-------|-------|--------|
| `/autoblogger:refine-idea` | Topic text + source URLs | `drafts/<slug>-brief.md` |
| `/autoblogger:draft-story` | Brief file path | `drafts/<slug>-outline.md` |
| `/autoblogger:generate-post` | Outline file path | `drafts/<slug>-v0.md` |
| `/autoblogger:export-to-docs` | V0 draft file path | Google Doc |

**Example step-by-step workflow:**

```
/autoblogger:refine-idea "How we use LOTO for model explainability" https://docs.google.com/document/d/1abc.../edit
# Review and edit drafts/loto-explainability-brief.md

/autoblogger:draft-story drafts/loto-explainability-brief.md
# Review and edit drafts/loto-explainability-outline.md

/autoblogger:generate-post drafts/loto-explainability-outline.md
# Review and edit drafts/loto-explainability-v0.md

/autoblogger:export-to-docs drafts/loto-explainability-v0.md
# Google Doc created for team review
```

## Blog Post Conventions

- **1000-1500 words** per post
- **"We" voice** (team/company), never "I"
- **Problem framing first**, then solution
- **Figures in every section** (generated as placeholders)
- **Numbered citations** [1] with APA-format References section
- **No discussion of credit (Hyoga)** as an application

## Project Structure

```
autoblogger/
├── .claude-plugin/plugin.json     # Plugin metadata
├── skills/
│   ├── refine-idea/SKILL.md       # Phase 1: Topic → brief
│   ├── draft-story/SKILL.md       # Phase 2: Brief → outline
│   ├── generate-post/
│   │   ├── SKILL.md               # Phase 3: Outline → draft
│   │   └── style-guide.md         # Writing style rules
│   ├── export-to-docs/SKILL.md    # Phase 4: Draft → Google Doc
│   └── write-post/SKILL.md        # All phases end-to-end
└── examples/example-post.md       # Reference post for tone calibration
```
