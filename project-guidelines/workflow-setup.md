# Thesis Writing Workflow

## Stack


| Tool                     | Role                                            |
| ------------------------ | ----------------------------------------------- |
| Claude UI (this project) | Chapter directives, reference, discussion       |
| Cursor + Claude Code     | Prose drafting, LaTeX, git, compilation         |
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

### Step 2 — Draft in Cursor

Open Cursor in the `msc-thesis` repo. Paste the brief and tell Claude Code
to draft the chapter directly into `/latex/chapters/[XX-chapter-name].tex`.

Iterate in the same session until the chapter is approved:

- Flag what sounds off ("too formal here", "wrong explanation of X")
- Claude revises in place; repeat until it reads correctly
- New citations go into `references.bib` using keys from `thesis-theory-topics.md`
- Commit when a logical unit is complete

### Step 3 — Overleaf

Overleaf pulls from GitHub on demand (GitHub sync). Open the project,
pull changes, compile, and do final visual refinement — spacing, figure
placement, layout tweaks.

---

## Multi-Session Chapters (>8 pages)

For long chapters (e.g., Chapter 2 Theory at ~20 pages), split into one
directive file per subchapter:

- One directive per subchapter, each in its own `[chapter-name]-brief.md`
- Draft each subchapter in a separate Cursor session, directly into its `.tex` file
- Reference what was established in previous subchapters explicitly
in the directive's "Connect back to" field ("Chapter 2a.1 defined the
CLS token — reference that definition here rather than redefining it")

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

The repo is the source of truth. All prose drafting happens directly in `/latex/chapters/` via Cursor.  
Overleaf is the output layer only — pull from GitHub, compile, refine layout.  
Never edit directly in Overleaf and push back — keep the flow unidirectional.