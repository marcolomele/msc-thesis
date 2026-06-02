# Thesis Writing Workflow

## Stack


| Tool                     | Role                                            |
| ------------------------ | ----------------------------------------------- |
| Claude UI (this project) | Drafting prose, voice iteration, chapter directives |
| Cursor + Claude Code     | File ops, LaTeX conversion, git, compilation    |
| GitHub (`msc-thesis`)    | Single source of truth                          |
| Overleaf (GitHub sync)   | Final rendering and manual refinement           |


---

## Project Knowledge (Claude UI)

The following files live in this project and are read automatically
by Claude at the start of every drafting chat:

```
/project-guidelines/
  - scope.md               ← audience, depth, thesis-wide conventions
  - style-guide.md         ← voice, tone, section-specific rules
  - writing-samples.md     ← labeled excerpts from real writing
  - thesis-outline.md      ← chapter structure and narrative arc
  - thesis-theory-topics.md ← theory concepts, depth levels, bibtex keys
  - technical-specs.md     ← research details and findings (add when ready)

/chapter-briefs/
  - chapter-brief-template.md   ← fill one per chapter before drafting
  - [chapter-name]-brief.md     ← completed directives, one per chapter
```

---

## Per-Chapter Workflow

### Step 1 — Fill the Directive (you, ~5 mins)

Copy `chapter-brief-template.md`, rename it `[chapter-name]-brief.md`,
and fill the fields at the top:

- **Paper section** — paste the corresponding section from the submission paper
- **Target length** — how many pages to expand to
- **Arc** — the flow of the argument (A/B/C/D or custom)
- **Deep / Surface / Skip** — depth calls per concept
- **Connections** — backward and forward links to other chapters

Commit the directive to the repo before drafting begins.

### Step 2 — Draft in Claude UI (new chat in this project)

Open a new chat. Paste the entire brief file as your first message.
Claude reads the project knowledge files and drafts the chapter in your voice,
following the paper section's order of information.

Iterate in the same chat until the draft is approved:

- Flag what sounds off ("too formal here", "wrong explanation of X")
- Claude revises; repeat until it sounds like you wrote it

### Step 3 — Hand off to Cursor

Copy the approved draft. Open Cursor in the `msc-thesis` repo.
Paste the draft and tell Claude Code:

```
Save this draft to /thesis-drafts/[chapter-name].md,
convert it to LaTeX and save to /latex/chapters/[XX-chapter-name].tex,
add any new citations to references.bib using the keys in
thesis-theory-topics.md, then commit and push.
```

Claude Code handles file creation, LaTeX conversion, bib updates,
and git. You do not touch the terminal manually.

### Step 4 — Overleaf

Overleaf pulls from GitHub on demand (GitHub sync). Open the project,
pull changes, compile, and do final visual refinement — spacing, figure
placement, layout tweaks.

---

## Multi-Chat Chapters (>8 pages)

For long chapters (e.g., Chapter 2 Theory at ~20 pages), split into one
directive file per subchapter:

- One chat per subchapter, each with its own directive
- Start each chat with the same project knowledge + the subchapter directive
- Reference what was established in previous subchapters explicitly
in the directive's "Connect back to" field ("Chapter 2a.1 defined the
CLS token — reference that definition here rather than redefining it")
- Consolidate all subchapter drafts in Cursor before LaTeX conversion

---

## Git Conventions

Commit format is defined in `.claude/CLAUDE.md` — that file is the authority.

---

## Overleaf Setup (one time)

1. In Overleaf: New Project → Import from GitHub → select `msc-thesis`
2. Set `main.tex` as the root document
3. To sync: Overleaf menu → GitHub → Pull

Overleaf is the rendering and refinement layer only.
All source edits happen in the repo via Cursor.
Never edit directly in Overleaf and push back — keep the flow unidirectional.

---

## Key Principle

The Claude UI project is the writing environment.  
The repo is the source of truth. Changes to the repo are pushed only from Cursor editor.   
Overleaf is the output layer.  
These three never overlap in responsibility.