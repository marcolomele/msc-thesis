# Chapter Brief: [Chapter Number — Chapter Name]
# e.g., "Chapter 2a — Visual Representation" or "Chapter 2a.1 — Transformer Architecture"

---

## Meta

- **Chapter type**: [Full chapter / Subchapter of X]
- **Target length**: [pages]
- **Estimated chats needed**: [1 / 2 / 3 — one per subchapter if >8 pages]
- **Position in thesis**: [e.g., "Opens the theory block" / "Third subchapter of 2a" / "Bridges theory to pipeline"]
- **Depends on**: [Chapter X must be read first / Standalone]
- **Feeds into**: [Chapter Y assumes this / Nothing directly]

---

## The Angle

[What is the specific purpose of this chapter IN THE CONTEXT OF YOUR THESIS?
Not "explain Transformers" but "establish why self-attention is the right
inductive bias for our frame-level feature extraction problem." 1-3 sentences.
This is the single most important field — it separates a thesis chapter from
a textbook chapter.]

---

## Your Research's Relationship to This Topic

[How does this concept connect directly to what you built, found, or proved?
Be concrete: name the pipeline block, the design choice, or the result that
this chapter motivates. If the reader finished this chapter and forgot
everything except this connection, what must they remember?]

---

## Story Arc

[How should this chapter flow? Choose one or define your own. Be specific
about the pivot points — where does the tone shift from background to argument?
Where does the math land relative to the intuition?]

Option A — Problem first:
  Problem with prior approaches → Concept introduced as solution →
  Intuition → Key math → Limitations → Relevance to our work

Option B — Concept first:
  Named concept + historical framing → Intuition → Key math →
  Properties that matter for us → What we use / modify / extend

Option C — Our work first:
  We needed X in our pipeline → Here is why existing approaches fall short →
  Here is the background that explains our choice → Now you understand
  the design decision

Option D — Survey / landscape:
  Define the problem space → Category 1 (brief) → Category 2 (brief) →
  Category 3 (the one we use, deeper) → Why this one for our purposes

Custom:
  [Define your own sequence. Be explicit about each beat.]

---

## Subchapter Breakdown
[Fill this out if target length > 6 pages or estimated chats > 1.
For short chapters, write "Single pass — no breakdown needed."]

| Subchapter | Title | Pages | Chat # | Key content |
|---|---|---|---|---|
| 2a.1 | [Name] | [N] | 1 | [Core concept covered] |
| 2a.2 | [Name] | [N] | 1 | [Core concept covered] |
| 2a.3 | [Name] | [N] | 2 | [Core concept covered] |

Notes on subchapter dependencies:
[e.g., "2a.3 assumes 2a.1 is written — reference the CLS token definition
introduced there rather than redefining it."]

---

## Key Concepts to Cover

[List every concept that must appear. Assign a depth level and, where
relevant, a format hint. Be precise about what "deep" means here —
full derivation, key equations only, or mechanistic prose explanation?]

- [Concept 1] — depth: deep / medium / surface
  Format: [full derivation / key equations + prose / intuition only / one paragraph / one sentence]
  Note: [anything specific about how to handle this concept]

- [Concept 2] — depth: deep / medium / surface
  Format: [...]
  Note: [...]

- [Concept 3] — depth: deep / medium / surface
  Format: [...]
  Note: [...]

---

## What to Skip or Skim

[Be explicit. Every skipped topic is a decision — own it here so the
draft doesn't second-guess it.]

- [Topic X] — skip entirely. Reason: [reader knows this / out of scope / covered in chapter Y]
- [Topic Y] — one sentence maximum. Reason: [context only, not mechanistic detail]
- [Topic Z] — cite and move on. Cite: [Author et al., year]
- [Full derivation of X] — state result only. Reader can follow citation.

---

## Key Equations or Results to Include

[List every equation or formal result that must appear. Specify display
vs. inline, and whether it needs a full component-by-component explanation
or just a label and citation.]

- [Equation / result 1]
  Format: display / inline
  Explanation: full component breakdown / label + intuition only / none
  Note: [e.g., "This is the equation our pipeline uses directly — make the
  connection explicit in the prose following it"]

- [Equation / result 2]
  Format: display / inline
  Explanation: [...]

- None needed — intuition and prose only.

---

## Figures or Diagrams to Reference

[List any figures that will appear in this chapter or that should be
referenced from /assets/figures/. If a figure doesn't exist yet,
flag it so it can be created before LaTeX conversion.]

- [Figure name or description] — exists at: [path] / needs to be created
  Caption direction: [what the caption should communicate]
  Reference placement: [before / after the relevant paragraph]

- None — no figures in this chapter.

---

## Connections to Other Chapters

[Explicit forward and backward references the draft should include.
These keep the thesis feeling like a unified argument rather than
a collection of standalone sections.]

Backward (already written — can reference):
- [e.g., "Chapter 1 introduced the correspondence problem — connect
  the attention mechanism's global receptive field to that motivation"]

Forward (not yet written — foreshadow only):
- [e.g., "Chapter 2b will introduce SAM — mention here that the image
  encoder is a ViT, so the reader is primed when it appears"]

---

## Resources to Draw From

[Papers, textbook chapters, or other sources to use. Flag which are
primary (must cite and engage with) vs. background (can cite without
deep engagement). Bibtex keys from thesis-theory-topics.md where available.]

Primary:
- [Author et al., year — Title] — bibtex: [key]
  Use for: [specific argument, equation, or result]

- [Author et al., year — Title] — bibtex: [key]
  Use for: [...]

Background / cite and move on:
- [Author et al., year] — bibtex: [key]

Do not use:
- [Source X — reason: outdated / superseded by Y / wrong framing for our purposes]

---

## Draft Prompt for Claude

[Fill this out last, after all fields above are complete. This is the
opening message you paste into a new chat to start drafting this chapter.
It should be self-contained — the chat needs no other context beyond
the project knowledge files and this field.]

---
You are helping me draft [Chapter X / Subchapter X.XX] of my master's thesis.
The project knowledge files contain my style guide, scope, writing samples,
and thesis outline. Read them before drafting.

**Chapter**: [Number and name]
**Target length**: [N pages]
**Angle**: [Paste from "The Angle" field above]
**Story arc**: [Paste chosen arc]
**Your research's relationship**: [Paste from that field above]

Key concepts to cover (with depth levels):
- [List]

What to skip:
- [List]

Key equations:
- [List]

Connections to weave in:
- Backward: [List]
- Forward: [List]

Tone notes:
- [Any deviations from standard register]

Primary sources to draw from:
- [List with bibtex keys]

Draft the full chapter now. Write in my voice as documented in the
style guide. Use LaTeX formatting throughout: display equations with
\begin{equation}, inline math with $...$, citations with \cite{key}.
After the draft, flag any decisions you made that I should review
(depth calls, skipped content, structural choices).
---

## Post-Draft Checklist
[Fill this out after receiving the draft, before approving for LaTeX conversion.]

- [ ] Voice matches style guide (read the Red Flags section)
- [ ] Angle is clear — a reader could state the chapter's thesis in one sentence
- [ ] Every equation has a component-by-component explanation in prose
- [ ] Every figure is referenced in text before it appears
- [ ] Forward and backward connections are present
- [ ] No concept is explained that scope.md says to skip
- [ ] Citations use inline name+year + \cite{key} format
- [ ] Closing paragraph pivots correctly to what comes next
- [ ] Nothing sounds like it was written by an AI