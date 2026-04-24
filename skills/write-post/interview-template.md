# Interview-to-Post Template (Internal Reference)

> This template is your internal checklist for guiding the conversation in Phase 0.
> **Do NOT show section names, numbers, or this structure to the user.**
> Use the guiding questions naturally in conversation to draw out the story.

## Quick Guidelines

- **Scope:** One concept per post. If too broad, suggest a multi-part series.
- **Length:** 1,000-1,500 words. If significantly over, look for split points.
- **Tone:** Neutral, human, transparent. Technical depth balanced with accessibility.
- **Visuals:** Strong bias for figures. Use `[FIGURE: ...]` placeholders.
- **Confidentiality:** Never discuss credit (Hyoga) as an application. Obfuscate as "internal benchmarks" or "general recommendation problems."

## Story Sections

### 0. The Hook (50-150 words)

**Goal:** Frame the problem and explain why it matters upfront.

**Guiding question:** Give me a summary of your solution. In a few sentences, what was the crisis or opportunity, how did we fix it, and what was the immediate result?

**Strategy:** Give the reader the summary and impact immediately so they understand the value of reading further.

### 1. The Trigger / "The Why" (100-300 words)

**Goal:** Define the technical or business stakes and the initial context.

**Guiding question:** What was the specific moment, data point, or system limitation that made us realize the current way of doing things was no longer enough?

**Strategy:** This moves the post from "We built X" to "We solved Y."

### 2. The Nubank Constraint / "The Hard Part" (100-300 words)

**Goal:** Explain what makes this uniquely difficult at Nubank's scale or within our culture.

**Guiding question:** Why couldn't we just use an off-the-shelf solution or a standard industry practice? What makes this uniquely difficult at Nubank's scale or culture (e.g., 100M+ users, Clojure, Datomic, sequential data)?

**Strategy:** Establish expertise by describing the "conflict" that makes a generic solution insufficient.

### 3. The Core Logic / "The How" (400-600 words)

**Goal:** Provide the "meat" of the post: methodology, architecture, and iterations.

**Guiding question:** Could you explain your solution in simple terms, but with analytical or technical detail?

**Strategy:** This is the core of the post. Provide depth beyond high-level pillars, but maintain a readable narrative. Use subheadings and bullet points for complex frameworks.

### 4. Learnings and Trade-offs (150-250 words)

**Goal:** Provide authenticity and real-world perspective.

**Guiding question:** What did you believe at the start that turned out to be wrong? What was the biggest trade-off you had to accept?

**Strategy:** Moves the narrative beyond PR into real-world learning, pivots, and unexpected hurdles.

### 5. Impact and Future Reflection (150-200 words)

**Goal:** Highlight the success and invite further discussion.

**Guiding question:** Now that this is live, what is the main metric of success? (This can be internal: "we deliver faster", "internal efficiency improved".)

**Strategy:** Tie the work back to Nubank's mission of fighting complexity. End with an invitation for technical discussion or a look at what's next.

## Authorship & Acknowledgements

- **Authors:** Those who make a substantial contribution to writing and content. Usually 1-3 people.
- **Author order:** Decided by contribution. Agree before starting.
- **Acknowledgements:** No limit. Err on the side of giving credit. Include people who helped with data, infra, pipeline efficiency, organizing the work, etc.
