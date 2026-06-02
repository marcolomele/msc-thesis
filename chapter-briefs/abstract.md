# Chapter Brief: Abstract

---

You are helping me draft [Chapter X / Subchapter X.X] of my master's thesis.
The project knowledge files contain my style guide, scope, writing samples,
and thesis outline. Read them before drafting.

- **Expand to**: 0 pages, max 4000 characters
- **Arc**: Present the context, highlight the problem, mention what competitors are doing, strike their limitations, present the project, list the strength selling points of the project, and list the contents of the thesis. 
- **Deep treatment**: No depth. Present the context, highlight the problem, mention what competitors are doing, strike their limitations, present the project, list the strength selling points of the project, and list the contents of the thesis. 
- **Surface only**: NA
- **Skip entirely**: NA
- **Connect back to**: NA
- **Connect forward to**: NA

**Paper section to expand** (follow this order of information):
\begin{abstract}
Relating objects across different viewpoints is a fundamental task for
multi-camera intelligent vision systems, complicated by drastic changes in
scale, lighting, and visibility and the absence of geometric calibration.
Prior works have achieved respectable results on EgoExo-4D, the
primary benchmark for this task, but all rely on architectures trained on
benchmark data, making them inapplicable to novel camera configurations and
opaque to inspection. Motivated by the broader hypothesis that
general-purpose foundation models composed at inference time may substitute
task-specific trained architectures on non-trivial visual tasks, a shift
already realised in language and emerging in vision, we present a fully
training-free pipeline in which natural language serves as the bridge between
viewpoints. A vision-language model describes the target object using view-
and time-independent terms; an open-set detector grounds that description to
select anchor frames in the destination view; Segment Anything Model~3 Agent
segments and propagates the mask across frames. No component is fine-tuned,
and the bridge between views is a human-readable description. We benchmark
our approach against published methods, reaching within a few percentage
points of the 2025 challenge winners, and provide a detailed ablation study
examining where foundation models succeed and where they remain limited on a
task none were designed for. Code is available on
\href{https://github.com/marcolomele/reluminati-research}{GitHub}.
\end{abstract}

---

Draft the full chapter now. Write in my voice as documented in the style guide.
Use LaTeX formatting throughout: display equations with \begin{equation},
inline math with $...$, citations with \cite{key}.
After the draft, flag any decisions you made that I should review
(depth calls, skipped content, structural choices).

---

## Post-Draft Checklist

- [ ] Voice matches style guide (read the Red Flags section)
- [ ] Structural flow matches the paper section's order of information
- [ ] Every equation has a component-by-component explanation in prose
- [ ] Every figure is referenced in text before it appears
- [ ] Forward and backward connections are present
- [ ] No concept is explained that scope.md says to skip
- [ ] Citations use inline name+year + \cite{key} format
- [ ] Closing paragraph pivots correctly to what comes next
- [ ] Nothing sounds like it was written by an AI
