# Building Nubank Engineering Blog — Style Guide

> **These rules define the target standards for V0 drafts. All output requires human review, editing, and approval before publication.**

This guide encodes the writing conventions of the "Building Nubank" blog, extracted from 9 published posts by the AI-Core team. Apply every rule when generating a V0 draft.

## Voice & Tone

- **Always use "we"** (team/company voice). Never "I". Never passive where "we" fits.
- Professional but accessible — like explaining to a smart colleague from a different team.
- Comparable tone: Stripe, Uber, Airbnb engineering blogs.
- Show genuine enthusiasm for technical elegance without being breathless.
- NOT academic ("heretofore", "it should be noted that"). NOT casual ("super cool", "we hacked together").

**DO:** "We hypothesize that by building foundation models from bank transactions, we can understand our customers beyond the capabilities of existing methods."
**DON'T:** "It is hypothesized that the construction of foundational architectures may yield superior customer comprehension."
**DON'T:** "So we basically threw together a model and it worked great!"

## Structure (in order)

1. **Title**: Descriptive, technical, specific. Example: "Giving Foundation Models a Notion of Now" — not "How We Made Models Better"
2. **Subtitle**: One sentence summarizing the key insight the reader will gain
3. **Author**: "Author: [Name]" or "By [Names]"
4. **Introduction** (~150-200 words): Frame the problem. Connect to Nubank's mission or customer impact. Reference previous posts if part of a series. End with a roadmap of what the post covers.
5. **Technical sections** (3-5 sections, each ~200-300 words): The core content. Each section has a descriptive heading. Follow the Progressive Explanation Pattern (below).
6. **Conclusion** (~100-150 words): Summarize key takeaways. Connect back to customer value. Preview future work or next post in series.
7. **Acknowledgements** (optional): Credit contributors beyond the authors.
8. **References**: Numbered [1], [2], etc. APA format. Sourced from Google Scholar.

**Total word count: 1000-1500 words.**

## Progressive Explanation Pattern

Every technical concept follows this progression:
1. **Simple statement** (1-2 sentences): What is it?
2. **Intuition or comparison** to something familiar: Why does it matter?
3. **Technical detail**: How does it work? Architecture, implementation, design decisions.
4. **Quantitative results**: Specific numbers showing it works.

**Example from published post:**
> "One straightforward option for building this interface is to assign an ID to each unique transaction... However, this has two key disadvantages. Firstly, the number of possible transaction configurations is very large... Secondly, this approach also suffers from the cold start problem..."

Always compare 2-3 approaches and explain why the chosen approach wins.

## Figures

- Every major section should reference at least one figure.
- Introduce with lead-in text: "The figure below shows...", "As illustrated in the figure below..."
- Use `[FIGURE: description]` placeholders with detailed descriptions.
- Figures should be for: architecture diagrams, data flow, comparison charts, results graphs, tokenization examples, pipeline visualizations.
- Include a suggested caption: `Caption: "Figure N: description"`

## Citations & References

- Inline: numbered `[1]`, `[2]`, `[3]`
- References section at end, APA format:
  `[1] Vaswani, A., Shazeer, N., ... (2017). Attention is all you need. Advances in neural information processing systems, 30.`
- Cite: foundational papers, prior art being compared, tools/frameworks mentioned
- Typical count: 5-10 references per post
- Use Google Scholar as the source for citation formatting

## Quantitative Results

- Always include specific numbers: AUC improvement, latency, throughput, cost reduction
- Compare before/after or approach A vs. approach B
- Contextualize: "a relative improvement of 1-1.25 points on these tasks is significant enough to release a new model"
- Use tables for multi-metric comparisons
- Format: "reduced from 450ms to 12ms (37x improvement)" or "achieved a 0.1pp AUC lift"

## Series Integration

- If part of a series, reference previous posts with links: "In our previous post, 'Title,' we discussed..."
- Preview upcoming posts: "In Part N, we will explore..."
- Each post must be self-contained — valuable even without reading the series

## Content Restrictions

- **Never discuss credit (Hyoga) as an application** of models. May reference "internal benchmarks" or "general recommendation problems" instead.
- Keep word count between 1000-1500 words
- Limit authorship to 1-3 people
- Use Acknowledgements section for broader credit to contributors
- Subtly connect to Nubank's purpose: fighting complexity, empowering people, customer-centered solutions

## AI Writing Patterns to Avoid

Never use these patterns — they are hallmarks of AI-generated text:

**Punctuation & formatting:**
- Em-dashes (—) — rewrite using commas, parentheses, colons, or separate sentences
- Excessive semicolons to join clauses

**Overused words & phrases:**
- "delve", "delve into"
- "landscape" (as in "the AI landscape")
- "tapestry"
- "leverage" (as a verb — use "use" instead)
- "utilize" (use "use")
- "in terms of"
- "it's worth noting that"
- "interestingly"
- "game-changer", "game-changing"
- "cutting-edge", "state-of-the-art" (unless citing a specific benchmark)
- "revolutionize"
- "robust" (when used vaguely)
- "seamless", "seamlessly"
- "comprehensive"
- "Moreover", "Furthermore" as sentence starters (occasional use is fine, but not every paragraph)

**Structural patterns:**
- Starting multiple paragraphs with "This..."
- Lists where a paragraph would read more naturally
- Ending sections with a generic forward-looking sentence ("As we continue to...")
- "In this post, we will explore..." — just start the content

## Sentence-Level Rules

- Active voice preferred
- Paragraphs: 3-5 sentences max
- Use bullet points or numbered lists for 3+ related items
- Technical terms: define on first use, then use freely
- Code/config/commands: always in code blocks
- Mathematical notation where formulas are relevant
- Oxford comma: YES
