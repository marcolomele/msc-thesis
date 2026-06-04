# Style Guide —

---

## General Style Rules

### Voice & Tone

**First person plural throughout written work.** All technical reports and thesis use "we" exclusively — never "I," never the passive impersonal. "I" only appears in oral scripts. This is a strong rule with zero exceptions across all written samples.

**Invested but not emotional.** The tone is confident and purposeful, not distant. There is visible investment in the subject, especially in closings ("The beauty of advanced fields like Dynamic Graph Neural Networks lies in their ability to make us reconsider the most basic foundations of our knowledge" — Sample 4, Conclusion). This investment never slides into enthusiasm or informality within written sections.

**Hedged claims, unhedged structure.** Claims about data or results are consistently hedged; the *structure* of the argument (what the paper does, why it matters, what sections contain) is stated flatly. "We hypothesize that the importance of house features is heterogeneous across space" (Sample 1, Intro) versus "In this paper, we first explore... We then detail..." (Sample 2, Intro) — no hedging on the roadmap, full hedging on empirical claims.

**Formal register throughout, with one permitted loosening:** the closing paragraph of a conclusion may include a philosophical or reflective sentence that steps outside formal register. This is deliberate and consistent across multiple samples.

---

### Sentence & Paragraph Structure

**Average sentence length (written): 18–25 words.** Sentences are complex but not baroque. Single-clause declarative sentences (10–14 words) appear at paragraph openings and for emphasis: "Real estate is one of the few markets that can truly impact the economy of a country" (Sample 1, Intro). Extended compound sentences (30+ words) appear mid-paragraph when listing observations or conditions.

**Paragraph length: 3–5 sentences.** Almost never a 1-sentence paragraph in written work (exception: transitional summaries in Results). Paragraphs in Methods/Experiments tend toward 4–6 sentences; Introduction paragraphs tend toward 3–4.

**Paragraph opening pattern:** The first sentence establishes context, makes a claim, or introduces a named concept. It is never a citation, never a question (in written work), and rarely begins with "However." The topic of the paragraph is immediately apparent.

**Paragraph closing pattern:** Closes with interpretation, implication, or a forward pivot. "This is analogous to tuning FastSAM such that it segments details rather than follow object contours" (Sample 3, Results) — closes an observational paragraph with an analogy. "The latter element makes the bulk of the estimation, and it is called denoising matching" (Sample 5, oral) — names the key term last.

**Paragraph assembly — the generative sequence.** Every paragraph follows a four-step logic, in order: (1) open with a claim, named concept, or concrete observation the paragraph will develop — this sentence is never a citation, never a question, and never "We can see that..." — it stakes a position; (2) develop through 2–4 sentences that move by logical connectors ("because," "although," "while," "but," "yet," "However," "Moreover," "Crucially") rather than by em dashes or colons — each sentence advances, none merely restates; (3) ground the claim with an example, a cited result, an equation reference, or an analogy; (4) close with interpretation, implication, or a forward pivot toward the next idea.

**Paragraph splitting at argumentative breaks.** When a paragraph contains two distinct argumentative moves — for example, describing what a class of models does and then stating the author's motivated response to it — split into two paragraphs. The hypothesis, claim, or response that follows a conceptual description always starts a new paragraph rather than being appended to the descriptive one.

**Semicolons over colons for consequence clauses.** When a sentence ends with a claim and the next clause is a direct consequence or elaborating example rather than a formal enumeration, use a semicolon: "the cost of failure is not merely technical; a robot that cannot identify..." A colon implies a list follows; a semicolon implies a closely related elaboration. The distinction matters — do not use a colon where a semicolon is correct.

**"but instead" as a reframing connector within a sentence.** When the second clause of a sentence names the actual thing that replaces what the first clause just negated ("not X; their Y is the objective"), prefer ", but instead" over a semicolon to signal the reframe: "not optimised for any single downstream objective, but instead their generality is the objective." Reserve semicolons for additive elaboration; use "but instead" when the second clause is the corrective.

**Inter-paragraph logic.** Paragraphs chain by tension, not by sequence. The connector between paragraphs names the complication the previous paragraph left open and launches the response: "However," "To address this," "To this end," "In contrast," "Nevertheless." Pure additive chaining — "Moreover" paragraph after "Moreover" paragraph — signals a list rather than an argument and should be avoided. Aim for a consistent and engaging narrative flow.

**"However," is the primary contrast pivot.** Used consistently at the start of sentences to mark a limitation, contrast, or complication. "Moreover," is the additive counterpart. "In contrast," is used for direct side-by-side comparisons (especially in Results). Consider also using "conversely" and "oppositely" when "however" becomes repetitive. Use "Nonetheless" when a weakness, drawback, or negative point is counter-balanced, softened, or reduced by another point.

**Enumeration in prose uses (i)...(ii)...(iii)... format** within a single sentence or short paragraph, not bullet points: "Namely, (i) that slope random effects across different features are uncorrelated, (ii) that..." (Sample 1, Method). Future work sometimes uses a numbered list format instead of prose, but body paragraphs never use bullets.

**Connector-rich prose over schematic punctuation.** Sentences flow through logical connectors — subordinating conjunctions ("because," "although," "while," "since"), coordinating conjunctions ("and," "but," "yet"), and transitional phrases — rather than relying on em dashes or colons to impose mid-sentence structure. When the impulse is to write "[claim] — [elaboration]", rewrite with a subordinate clause or begin a new sentence. When the impulse is to write "[claim]: [elaboration]", rewrite with "namely," "specifically," "that is," or a relative clause. Both punctuation marks interrupt rhythm and substitute typographic scaffolding for prose craft; both are prohibited mid-sentence.

---

### Vocabulary & Word Choice

**Favored words and phrases (strong evidence across ≥2 samples):**
- "To address this limitation" — Sample 1 Method, Sample 2 Intro, Sample 3 Intro
- "To this end" — Sample 1 Intro, Sample 4 Intro (BSc), Sample 6 oral
- "leverage" — Sample 2 Abstract, Sample 2 Intro, Sample 2 Method
- "nuanced" — Sample 3 Conclusion, Sample 5 oral
- "fundamentally" — Sample 6 oral (×2), Sample 5 oral
- "crucially" — Sample 2 Method, Sample 6 oral
- "specifically" — Sample 3 Data, Sample 6 oral
- "formally" — Sample 4 Experiment, Sample 6 oral
- "principled" — Sample 6 conclusion
- "empirically" — Sample 6 conclusion

**Hedging vocabulary:** "suggest," "indicate," "highlight," "appear to," "may," "could," "reflect." These are the preferred softeners. Hedging is almost always post-verb, not pre-claim: "suggesting that properties with more rooms tend to have additional bedrooms" (Sample 1, Data). Never leads with "perhaps" or "it might be that."

**Action verbs for research operations:** "inspect," "uncover," "investigate," "demonstrate," "expose," "validate," "devise," "restrict," "formalize," "conjecture." These are active and precise.

**Structural signal phrases:** "In this section, we..." / "In this work, we..." / "In particular," / "Specifically," / "That is," / "In other words," — used consistently for signposting and unpacking.

**Words not used:** "firstly," "secondly," "thirdly" (replaced by "First,... Then,... Finally,..."). "Very" and "quite" as degree adverbs are absent — quantitative precision or "significantly" is used instead. "Important" does appear, but "very important" does not.

**Technical terms:** Introduced with definition or context at first use, then used bare. Never re-defined. LaTeX formatting (\texttt{} for variable names, \textbf{} for key terms in abstracts) used in written documents.

---

### Precision & Craft

**Concrete and direct over safe and generic.** Every sentence earns its place; no sentence restates what a preceding one already established. This preference is visible structurally — paragraphs rarely exceed five sentences and waste none of them on throat-clearing.

**Precise word selection; the thesaurus is a tool, not a decoration.** Rarer or more specific words are used when they are the *right* word, not for ornamentation. Examples from the samples: "labour-intensive" rather than "time-consuming" (Sample 4, Theory); "idiosyncrasies" (Sample 6, oral ×2); "spurious" (Sample 6, oral); "heterogeneous" (Sample 1, Intro); "isomorphic" (Sample 7, BSc). The rule is precision, not complexity — a simpler word wins if it is equally precise.

**Lexical load drops as conceptual load rises.** In the most technically dense passages, the plainest accurate verb is preferred over the vivid or rare one, so the reader's effort goes to the concept rather than the diction. Vivid or uncommon verbs are downgraded to neutral equivalents when the surrounding material is already demanding — observed directly in the author's edits: "dispensed with recurrence" → "introduced a novel paradigm"; "are marred by" → "are corrupted by"; "scales this recipe" → "elevate this architecture"; "owning no pixels" → "referring to no pixels". The rarer word is reserved for the moment where it is the only precise option or where the passage is otherwise light. This is the operational form of the precision rule: precision first, and where two words are equally precise, the simpler one wins by a wider margin the harder the topic ("preferring simple and straightforward over complex since topic is already difficult").

**Analogies are structural load-bearers in long-form writing.** In oral work and in thesis-length writing, analogies are not one-line ornaments but extended explanatory frameworks: the Chinese Restaurant Franchise (Sample 6), the compass metaphor for score functions (Sample 5), the CNN kernel analogy for GCN (Sample 4). Each is developed for several sentences and then referred back to. In shorter technical reports, analogies are briefer but still present ("This is analogous to tuning FastSAM such that it segments details..." — Sample 3).

**Introductions and conclusions step up to a higher level of abstraction.** These sections deliberately exit the technical frame and share a view of the field, its direction, and its implications. This is the intentional "context couch" — giving the reader a broader vantage before and after the technical content. In introductions, this takes the form of a universalizing opening claim; in conclusions, a philosophical or broader-impact closing sentence. The pattern is strong and consistent: "Network systems are ubiquitous in the real world" (Sample 4, Intro); "The beauty of advanced fields like Dynamic Graph Neural Networks lies in their ability to make us reconsider the most basic foundations of our knowledge" (Sample 4, Conclusion); "Will machine learning keep getting better at tasks that demand human intelligence — and yet grow more distant from how we understand the world?" (Sample 5, oral closing). This is not academic hedging softening a conclusion — it is a deliberate authorial stance.

---

### Technical Content & Equations

**Every equation is followed by a component-by-component explanation in prose.** No equation stands without a parsing paragraph. Pattern: equation → "Here, [symbol] is [definition]. It is [added/multiplied/...] to [X] to [reason]." (Sample 4, Method — GCN propagation rule). The explanation covers all novel symbols in the same paragraph.

**Footnotes are rare, and reserved for an important aside that would break prose flow if inlined.** The default remains to fold a necessary note into the sentence or to cut a tangential one. A footnote earns its place only when the note genuinely matters yet inlining it would interrupt a dense argument, such as a notation reconciliation an expert needs but that would derail the sentence carrying the main idea. The theory draft keeps exactly one, reconciling the student–teacher temperature notation with the chapter's reserved-symbol convention ($\tau$ for the video frame index). The test is whether the note is both important and flow-breaking; if it is merely tangential, cut it, and if it fits the sentence, inline it. A footnote is the exception, never a place to park detail.

**Each concept or symbol is explained exactly once, in a single block.** When an equation is parsed, every component receives its definition together with its consequences and considerations in one place, and that explanation is not restated later. Subsequent passages may build on the concept, but only through an explicit reference that names the prior point and the connection being drawn — never by repeating the original explanation. Cross-paragraph redundancy is treated as a defect: if a later argument needs an earlier concept, it points back to it rather than re-deriving it. This is the author's stated working rule ("each element in the equation has its associated concept stated only once, along with its consequences and considerations, in a single block; my writing style would allow for other points to build over these concepts, but only with a reference and what enables the connection").

**Analogy before or alongside formal treatment.** The formal definition is grounded with an intuitive counterpart: "a kernel is slid over the graph, and the embeddings are formed as a weighted sum of the embeddings of the neighbours" precedes the GCN formula (Sample 4, Method). In oral work, analogy comes first and is more extended (restaurant franchise metaphor for HPYP, Sample 6).

**Named behaviors get quotation marks at first use.** When an observation deserves a name, it is coined in quotation marks: "over-specification" and "easy distraction" (Sample 3, Results). These names are then used bare thereafter.

**Historical framing for introducing methods.** New models are introduced with their origin: "Inspired by CNNs, Kipf et al. (2016) introduced Graph Convolutional Networks (GCN)" (Sample 4, Method). The intellectual lineage matters.

**Quantitative results include the value parenthetically inline.** "The largest standard deviation is observed for Distance (0.3484), indicating significant spatial variability" (Sample 1, Results). The number sits in parentheses immediately after the subject, not in a separate sentence.

---

### Citations & References

**Inline name + year for embedded citations:** "Kipf et al. (2016)" directly in the sentence. LaTeX \cite{} used alongside or as sole citation form depending on context.

**Citations are embedded into the narrative, not stacked.** No sequences of [citation1][citation2][citation3] without surrounding text. Each citation has a corresponding sentence of context.

**Prior work is named with motivation before description.** "Motivated by its relevance, part of our analysis will inspect the tradeoff..." (Sample 3, Related Works) — the reason precedes the citation. This is consistent.

**Personal/contextual reference is permitted.** "This was confirmed later in 2024 by Dynamical regimes of Diffusion Models, a work of professor Mezard from our faculty" (Sample 5, oral) — citations in oral work include social or institutional context.

**Both inline name+year and \cite{} are used concurrently in written work.** Sample 7 confirms this: "DeepWalk by B. Perozzi et al. (2014)... \cite{Perozzi2014}" — the author and year appear in prose and the \cite{} tag follows in the same sentence. The name+year is not a replacement for \cite{}; it is a narrative embedding of the citation. The \cite{} is always added as well to link the citation to the references list.

**Citation management in Latex** All citations are stored inside `references.bib`; the \cite{} command is called for all citations to track numbering and allow cross-referencing during the text. Following highest standards of academic writing.

**Year integration for publication verbs.** When a citation is introduced via a verb that describes the act of publishing or assembling ("published by," "introduced by," "assembled by," "proposed by"), the year is integrated as "in [year]" rather than appearing parenthetically: "published by Grauman et al. in 2024 \cite{...}" not "Grauman et al. (2024) \cite{...}". When the author name appears in a more neutral role ("Grauman et al. (2024) show that..."), the parenthetical form is retained.

**"By the time of writing" for SOTA citations.** When citing the current state-of-the-art result in a fast-moving field, prefix the sentence with "By the time of writing," to acknowledge temporal uncertainty: "By the time of writing, Hu et al. (2025) achieve the strongest published results..." This hedge is used once per SOTA claim, not on every citation.

---

### Prohibited Constructions

**Paragraph openings:**
- Never open a paragraph with a citation — every paragraph opens with a claim, named concept, or concrete observation the author owns.
- Never open with "We can see that..." — state results directly.
- Never open the abstract with a question.

**Banned phrases:**
- "It is important to note that" / "It is worth noting that" / "It should be noted" / "Needless to say" — bureaucratic padding; remove entirely.
- "As mentioned above" / "As discussed previously" / "as mentioned previously" — implies the reader cannot remember.
- "In today's rapidly evolving world" or any colloquial temporal framing ("In this day and age," etc.).
- "This paper aims to" — replace with "In this paper, we..." or "we address X by doing Y."
- "The rest of this paper is organized as follows:" as a standalone sentence — roadmapping is embedded in the Introduction's closing paragraph as flowing prose.
- "Obviously," "Clearly," "Of course," — never used to wave past complexity.
- Sentence-opening "So," in written text — reserved for oral scripts only.
- "Firstly / secondly / thirdly" — always "First / Then / Finally" or "(i) / (ii) / (iii)."
- "Very [adjective]" / "extremely [adjective]" — use precise quantifiers or "significantly."

**Voice and register:**
- Passive voice as the default for describing what the researchers do ("it was found that" instead of "we found"). Passive is reserved for cases where the agent is genuinely irrelevant, or when describing what was done in prior work.
- "I" in written work — strictly "we" throughout.
- Exclamation marks in written work.
- Scare quotes on established terms — quotation marks reserved for coined phenomena at first introduction.

**Structure:**
- Bullet points within body paragraphs.
- Stacked citations without surrounding text — no `\cite{A}\cite{B}\cite{C}`.
- Mid-sentence em dash "—" to insert a parenthetical, contrast, or elaboration — rewrite as a subordinate clause or a new sentence.
- Mid-sentence colon ":" to introduce an elaboration or example — rewrite with "namely," "that is," or a relative clause. A colon is only acceptable after a grammatically complete sentence that formally opens an enumeration.
- **Hyphenated compound adjectives** — never use. Decompose into prose equivalents: "multi-view" → "multiple view"; "safety-critical" → "where safety is critical"; "training-free" → "without training" or "free of any training"; "task-specific" → "specific to the task"; "fine-tuned" → restructure the sentence; "long-term" → restructure. This applies to all compound adjectives that are conventionally hyphenated in ML writing — the rule is unconditional.

**Typographic emphasis:**
- `\textbf{}` (bold) is never used in body text — not for key concepts, not for emphasis, not for first use of a term. Bold is prohibited entirely in prose.
- `\textit{}` (italics) is used exclusively for: (a) proper names of datasets and systems at first introduction (*Ego-Exo4D*, *egocentric*, *exocentric*); (b) direction labels (*Ego2Exo*, *Exo2Ego*); (c) the single defining instance of a core technical term being introduced (*embedding*, *Transformer*), after which the term appears bare. It is never used to emphasise an abstract concept or claim.

---

## Figure Placement

**Figures appear after the paragraph that most directly describes their content.** A figure illustrating the task challenge follows the paragraph that defines and explains that challenge — not the opening paragraph of the section, and not before the reader has the conceptual context to interpret it. A figure illustrating the pipeline follows the paragraph about limitations that motivate the pipeline, immediately before the paragraph that describes the pipeline itself. The rule is: first prose, then figure, then the prose that uses the figure as evidence.

**Dataset framing before task framing.** When introducing a benchmark or dataset, frame it as enabling a class of tasks before narrowing to the specific task addressed: "The dataset enables many novel computer vision tasks, including cross-view object correspondence: given..." This avoids making the paper's own contribution the grammatical subject at the moment of introducing the resource.

---

## Section-Specific Rules

### Abstract

**Length:** 3–5 sentences. Dense, no filler.

**Arc:** Context/problem statement → named gap or limitation → our approach → key outcome. Sample 2 Abstract is the clearest exemplar: ICD-11 context → sparse descriptions + inaccessible proprietary models → Llama3 solution → "significantly improving completeness and semantic density."

**Bold key terms at first use** (\textbf{} in LaTeX). Typically the central dataset/model name and the core problem are bolded. Not everything is bolded — only what a skimming reader must register.

**Closing sentence states the outcome with "significantly" or a comparable strong qualifier.** Not hedged — the abstract's last line is a positive claim about what was achieved.

**No citations in the abstract.** Not seen in any sample.

**Never opens with "In this paper."** Opens with the domain/problem context first.

---

### Introduction

**Opens with a broad, often universalizing claim.** The opening sentence places the problem in the largest relevant context: "Real estate is one of the few markets that can truly impact the economy of a country" (Sample 1); "Network systems are ubiquitous in the real world" (Sample 4); "Egocentric and exocentric visual perspectives provide complementary information" (Sample 3, bolded at opening).

**Key terms bolded at first mention** (\textbf{}) — this is specific to Introduction and Abstract; body sections do not use inline bold for emphasis.

**Contrast pair structure for motivating the problem.** When two perspectives or approaches are being contrasted (ego vs. exocentric, traditional ML vs. representation learning), they are presented as a pair with "but" or "Conversely" making the trade-off explicit.

**"To address this limitation" as the pivot to our contribution.** Appears in Samples 1, 2, and 3. After the problem is established, this phrase (or a variant) launches the contribution paragraph.

**Roadmapping paragraph uses "In this paper/section, we first... Then... Subsequently... Finally..."** This is the closing move of the Introduction. It is written as flowing prose, not a bullet list. The verbs are specific: "explore," "detail," "outline," "evaluate."

**Hypothesis stated explicitly with "We hypothesize that..."** when applicable (Sample 1).

---

### Related Work

Two structural scales are in use depending on document scope. A BSc thesis uses **subsection-per-cluster** (each cluster of related methods gets its own named subsection). A technical report uses **paragraph-per-cluster** (each paragraph covers one method family). In both cases, the internal logic of each unit is identical.

**Prior work is named Author + Year in prose, not relegated to a citation tag alone.** "DeepWalk by B. Perozzi et al. (2014)" (Sample 7, BSc); "Feng et al. (2024) demonstrate..." (Sample 7, Technical Report); "Lee et al.'s ICD2Vec (2023) introduces..." (Sample 7, Technical Report). The author name is part of the sentence, not a footnote.

**Each paper is described then immediately limited.** The pattern is: [method name + author + year] → [what it does] → [what it cannot do]. "Feng et al. (2024) demonstrate that... TF-IDF and cosine similarity still effectively match medical narratives... but their approach lacks contextual awareness, limiting semantic precision" (Sample 7, Technical Report). The limitation is not held for a later paragraph — it lands in the same sentence.

**"Conversely" marks a finding that runs counter to the preceding paper's direction.** "Conversely, Klotzman (2024) finds embedding-based methods superior to large language models (LLMs) like GPT-4 in automatic ICD coding" (Sample 7, Technical Report). Used sparingly — only when the contrast is genuinely directional, not just different.

**Subsection openings state the cluster's shared property before naming papers.** "Modern approaches incorporate more information about the structure of the graph. Additionally, they target downstream inference tasks..." (Sample 7, BSc) — the defining characteristic of the cluster is established first; then the papers are introduced as examples within it.

**A synthesis paragraph closes Related Works with "Overall, existing research underscores..."** "Overall, existing research underscores the importance of specialized biomedical embeddings but does not comprehensively address the limitations of ICD-11's description sparsity..." (Sample 7, Technical Report). This closing sentence names what the existing literature achieves and what it leaves open — directly motivating the paper's contribution. It is the final sentence of the section, not a topic sentence.

**High-impact concrete example used to close the final subsection in long-form Related Work.** "In February 2020, a team of researchers from MIT used GNNs to discover halicin, a novel antibiotic capable of killing the world's worst disease-causing bacteria" (Sample 7, BSc). This is a deliberate hook — a real-world achievement that signals why the field matters before transitioning to the thesis's own work. The forward reference follows: "which we will explore in detail in the next chapter."


---

### Background & Theory

**Historical framing to introduce a method or paradigm.** "Over the last decade, Graph Neural Networks (GNNs) have emerged..." (Sample 4, Intro); "Inspired by CNNs, Kipf et al. (2016) introduced..." (Sample 4, Method). Places new content in an intellectual timeline.

**Theory subsections open from the task, not from abstraction.** Where the Introduction opens on a universalizing claim, a theory subsection opens by descending from the concrete goal to the thing that must be built first, often as an explicit dependency chain. The author's rewrite of a theory opener replaced the abstract "Every component of our pipeline operates on the same primitive object, namely a fixed-length vector..." with the task-grounded "In order to segment objects across videos we need to segment objects in individual frames, and in order to segment objects in images, we need to represent them numerically." The grand statement is reserved for the Introduction and Conclusion; inside the technical chapters the opening earns its place by motivating the immediate next step.

**A single running example is carried through a technical subsection.** Rather than a fresh illustration per concept, one concrete example is introduced at the start of a subsection and revisited at each new mechanism, so the reader tracks one thread instead of many. The author asked for exactly this when drafting the Transformer subsection ("use running example for entire sub-section of transformer to ground and concretise explanations"): a single translation sentence is introduced with the architecture, then reused to explain attention, multi-head attention, and positional encoding in turn. This is the subsection-scale counterpart to the structural analogies described above.

**Builds from abstract to specific, then from formal to interpretable.** The conceptual description precedes the equation, or follows immediately after, with a plain-language equivalent.

**"The fundamental idea remains..."** or similar phrasing used when extending a known method. Signals continuity with prior work before introducing the new component.

**Subsections within theory are named and introduced with a motivation sentence.** "In traditional machine learning, representations are constructed through feature engineering... However, this process is labour-intensive..." (Sample 4, Theory) — the motivation for the new approach is stated before the approach is named.

**Uses previous examples from within the paper** ("In our previous example...") to ground abstract definitions — signals awareness of the reader's existing mental model from earlier in the document.

---

### Methodology

**Opens with a roadmap sentence for the section.** "In this section, we formalize the Exo→Ego correspondence task, summarize the O-MaMa architecture, and detail the methodological explorations ran during our study: feature pre-extraction, finetuning analysis, and feature extractor backbone substitution." (Sample 3, Method). This mirrors the introduction-level roadmap but at section scope.

**Pipeline structure: First... Then... Next... Finally....** All method descriptions follow this sequential enumeration. No step is unnamed.

**Each design choice is justified inline.** "for fair comparison," "to investigate O-MaMa's capabilities," "which allows us to define clearly four groups of interest" — the reason follows the decision in the same sentence.

**Assumptions are listed explicitly when the method requires them.** Listed as "(i)... (ii)... (iii)..." in a single paragraph with "Namely," or "Specifically," as the opener (Sample 1, Method).

**Passive voice appears more here than elsewhere** — but only for describing what is done to data, not for describing what the researchers do. "we apply text preprocessing" vs. "lowercase conversion, special character removal, and whitespace normalization" — the sub-steps can be stated as noun phrases rather than agent-led sentences.

---

### Experiments

**Prior work named with full author and year** before describing the experimental setup, establishing the lineage: "the global agriculture trade problem, introduced by L. Jiang et al. (2023)" (Sample 4, Experiment).

**Context-setting narrative precedes formalization.** Two or three sentences explaining the domain story (what trade is, why it matters) before the formal mathematical setup ($\mathcal{G}$, observations, edges, weights).

**Mathematical formalization of the problem** uses defined notation: "each observation $o_t = ((i,j), t, \omega_t) \in \mathcal{O}$ indicates that..." (Sample 4, Experiment). Every symbol in the formalization is named.

**Scope restriction stated upfront with reason.** "We restrict our study to the Health scenario... as it provides 20+ hours of video of consistent scenes with a relatively low variety in object form and high quality annotations" (Sample 3, Data). The restriction comes before the positive description of the data.

**Mission statement for experiments in oral work** (and likely in written): "Our mission is clear: we want to [goal 1]; [goal 2]; and finally, [goal 3]." (Sample 6, oral). This structured declaration of experimental goals may translate into a written paragraph that enumerates the three things the experiments are designed to show.

---

### Results

**Named-phenomenon approach for emergent or unexpected findings.** Observations that are non-trivial get a coined name in quotation marks: "over-specification" and "easy distraction" (Sample 3, Results). After naming, each phenomenon is explained mechanistically, often with an analogy ("This is analogous to tuning FastSAM such that it segments details rather than follow object contours").

**Observation → value → interpretation structure.** "The largest standard deviation is observed for Distance (0.3484), indicating significant spatial variability in its impact on house prices" (Sample 1, Results). Always in this order.

**"We inspect," "We uncover," "We observe"** are the preferred verbs for describing what the analysis found. "We show" is less common; "we find" does not appear in the written samples.

**Visual + quantitative pairing.** Results are confirmed both with figures ("Looking at situations of high divergence... we uncover two emergent behaviors") and with quantitative values. Neither stands alone.

**"In contrast" for cross-method comparison.** "In contrast, contextual and medical-trained models like PubMedBERT, BioBERT, and GatorTron produce more consistent results" (Sample 2, Results). Signals that the comparison is being made explicit.

**"Interestingly" flags unexpected or noteworthy findings.** Used sparingly and with discipline — only when the finding is genuinely notable.

**Conjectures are offered for results that cannot be fully explained.** "We provide three conjectures that explain such results" (Sample 3, Results). These are labeled as conjectures, not conclusions.

---

### Discussion

**Discussion is integrated into Results, not separated.** Sample 8 contains no standalone Discussion section — interpretation follows immediately after each observed result within the Results section. Subsections are organized by analysis condition (baseline reproduction, pre-extraction, finetuning analysis, robustness), and each closes with an interpretive synthesis of that condition's findings.

**"Strikingly" for results that contradict prior expectations; "Interestingly" for results that are notable but not contradictory.** "we encounter a striking result: finetuning leads to a decrease in IoU" (Sample 8, Technical Report) — the colon after "striking result" is a consistent construction, where the label precedes the fact. Compare "Interestingly, training with negative sampling proved to be the most effective technique" (Sample 8, BSc) — unexpected but not paradoxical. Do not use both interchangeably.

**Named-set approach for multiple concurrent findings.** "Three criticalities emerge from our analysis: strong overfitting to the training set, no improvement (and often degradation) in validation IoU, and critical under performance..." (Sample 8, Technical Report). The set is named and enumerated in one sentence; the following sentences then address each element in order. This mirrors the three-part structure visible throughout the samples.

**After naming the set, distribute resolution explicitly.** "The first two issues are addressed in the next section. The latter is likely a consequence of our methodology..." (Sample 8, Technical Report). Each item in the named set receives a disposition — either resolved here, resolved later, or explained with a conjecture.

**Conjectures are numbered and mechanism-led.** "First... Second... Finally..." — each conjecture begins with its number and immediately states the mechanism, not just the observation: "the contrastive loss function corrupted the feature representation by over-emphasizing the minimal object variety of the train set. This lead to space collapse and spurious learning..." (Sample 8, Technical Report). The mechanism is the substance of the conjecture.

**Borrowed terms are sourced.** "Borrowed from the Natural Language Processing field, it refers to the degradation of previously learned capabilities when a model is fine-tuned on a new task" (Sample 8, Technical Report — on "catastrophic forgetting"). When a term from another field is imported, the field of origin is named in the sentence where the term is first used.

**Interpretive closing of each subsection is hedged.** "likely explained by a coincidental resemblance between our dataset and the finetuning dataset" (Sample 8, Technical Report). Final interpretations within Results/Discussion subsections are qualified — "likely," "may," "suggest" — not asserted flatly. This is in direct contrast to the abstract's closing sentence, which is unhedged.

**Prior work is described, then its key metric reported, then compared.** "TGN consistently outperformed the DGNNs, surpassed the GCN thanks to the added temporal dimension, and achieved the second-lowest Mean Squared Error (MSE) in all but one instance, where it was the best" (Sample 8, BSc). The comparison is structured as a progression: best-to-worst ordering of what the model beat, ending with the notable exception.

**Note on informality:** "Anyhow" appears once (Sample 8, Technical Report) as a connector. This is an isolated instance and likely a slip in register rather than a deliberate style choice — it should not be replicated.

---

### Conclusion

**"Before concluding" or "In conclusion" as the opening move.** "Before concluding, we would like to identify future research opportunities..." (Sample 1, Conclusion). The meta-signal is explicit.

**Acknowledges limitations before future work.** The gap is named ("a clear research gap, as one baseline forecasting method outperformed the GNNs" — Sample 4) before future directions are offered.

**Future work is enumerated (i)/(ii)/(iii) or as a compact list.** "This limitation points to three directions for future work: (i) integrate graph neural networks... (ii) utilize cutting-edge foundation models... (iii) explore scaling laws..." (Sample 2, Conclusion). Always named, never vague.

**"On the one hand... On the other hand..."** used to balance two classes of future work (Sample 1, Conclusion).

**Closing sentence is philosophical, reflective, or broader-impact.** "The beauty of advanced fields like Dynamic Graph Neural Networks lies in their ability to make us reconsider the most basic foundations of our knowledge" (Sample 4, Conclusion). This is the one place where the register softens and the authorial voice becomes visible. Consistent across Samples 1, 2, 4.

**Callback to introduction.** Oral Sample 6 closes with "Therefore, independent samples are not always islands" — a direct callback to the talk's title/opening question. This pattern is likely present in written conclusions too.

---

## Recurring Patterns & Quirks

**"To address this limitation" is almost a tic.** It appears across Samples 1, 2, and 3, always as the pivot from problem statement to contribution. It should be considered the default transition phrase.

**"To this end" closes motivation and opens action.** Appears in Samples 1, 4 (BSc), and 6 (oral). Used at the paragraph level when transitioning from "why we need X" to "here is how we do X."

**Three-part structure for experimental contributions.** The paper consistently reports contributions in groups of three: three phenomena, three future directions, three experimental goals, three architectural components. This is not a superficial list — each element is developed. Consider this a structural instinct.

**Human cognition as contrast.** Multiple conclusions and discussions compare machine behavior to human perception or reasoning: "we humans are able to infer object positions even when complete occlusion appear, because of our ability to track space and time conjunctively" (Sample 3, Conclusion); "Think about transformers: they learn to generate natural language by encoding word co-occurrence via self-attention, which is akin to how we can discern links between words" (Sample 5, oral). This is a recurring rhetorical move for situating ML results.

**Analogies are structural, not decorative.** The Chinese Restaurant Franchise, the compass metaphor for score functions, the CNN kernel analogy for GCN — these analogies are developed enough to carry an explanation for several sentences. They are not one-line comparisons.

**Rhetorical question at the closing of oral work.** "Will machine learning keep getting better at tasks that demand human intelligence — and yet grow more distant from how we understand the world? If so, what does that say about human intelligence?" (Sample 5, oral). This technique does not appear in written work — but the philosophical impulse behind it may express as a reflective closing sentence in written conclusions.

**"The key [idea/difference/intuition] is..."** used to flag the most important design point in a complex explanation. Appears in Methods and oral scripts.

---

## Voice Authenticity Markers

These are small-scale linguistic choices that recur across multiple samples and document types. A generic model would not generate them and would likely correct them away. Preserve them when generating text for this project.

**"Intuitively," as a systematic informal-explanation opener.** This word appears six or more times in the BSc thesis alone, always as the sentence opener that flags the informal restatement of a formal definition or equation: "Intuitively, nodes that are similar to each other will have embeddings with high proximity in the embeddings space" (BSc thesis, Ch2); "Intuitively, the RNN moves along the input sequence and generates each output based on the current input" (BSc thesis, Ch5). It also appears in oral scripts (Sample 5). It is never decorative — it always marks the transition from formal to human-readable. Do not substitute "Conceptually," or "In other words."

**Explicit deferred-content transparency.** Rather than silently omitting derivation steps or deferring topics, the author consistently names the omission and points to where the content lives: "A series of steps to alleviate the computational cost...that we will not cover lead to the following approximated model" (BSc thesis, Ch3); "We will return to RNNs in Chapter 5" (BSc thesis, Ch5); "a topic we leave for future works" (Sample 8, technical report); "which I discuss in the Appendix" (Sample 5, oral, ×2); "More details are in the appendix" (Sample 6, oral). This habit of naming deferred content — rather than simply moving on — appears in every document type. In generated text, when a derivation is being skipped or a topic deferred, name the omission explicitly. Keep the pointer short and outward, naming where the content lives ("which we describe in the next section"), and drop the self-referential scaffolding that frames the act of writing rather than the content. The author's edits cut exactly this scaffolding: "For completeness we note one further pretext task..." and "We do not develop MAE further here, but flag it now because..." were both removed in favour of a direct statement that still carries the forward pointer. The transparency is about where the content is, not about the decision to defer it.

**"At their/its core"** for identifying the essential mechanism of a system before formal treatment. "At their core, GNNs are a node embedding function that applies neural architectures on graph-structured data using the neighbourhood aggregation strategy" (BSc thesis, Ch3); "At its core, a DPM learns to minimize the difference between the forward ground-truth denoising step and the model's approximate denoising step" (Sample 5, oral). Appears in both written and oral work. Do not replace with "fundamentally" or "essentially" — those are used elsewhere in the samples with different force.

**"In all but one [X]"** for restrained qualification of near-universal results. Appears across at least two separate documents: "achieved the second-lowest Mean Squared Error (MSE) in all but one instance, where it was the best" (BSc thesis, Ch5); "one baseline forecasting method outperformed the GNNs in all but one test condition" (Sample 1, technical report on Melbourne housing). The construction is the preferred way to describe a near-universal result that has exactly one exception — rather than "almost always" or "with one exception."

---

---

## Notes on Oral vs. Written Voice

**Sentence length drops sharply in oral work.** Written sentences average 18–25 words; oral sentences average 8–12 words. Many oral sentences are fragments: "Here is the architecture." "Visually, we cement two ideas." This brevity is functional — it matches slide pacing.

**Oral scripts use direct audience address; written work never does.** "You see," "Think about transformers," "Let's consolidate the point visually," "Consider this visualisation" — all oral only. Written work has no second-person.

**"I" appears only in oral work.** "this Christmas I generated a new haircut" (Sample 5) — personal anecdote grounding a technical concept. Written work is strictly "we."

**Personal stakes are visible in oral closings.** "Studying diffusion models has been thought-provoking, mostly because at first I found them un-intuitive" (Sample 5). Written closings express the intellectual significance without personal disclosure.

**Present tense for historical narration in oral work.** "In 2015, a group of Stanford and Berkeley researchers propose Diffusion Probabilistic Models" (Sample 5). Historical present makes oral delivery more dynamic. Written work uses past tense for prior work.

**"Visually," as a slide-transition signal.** Appears repeatedly in Sample 5 to mark the shift from verbal description to diagram. In written work, this becomes "Figure X depicts..." or "see Appendix Fig. Y for..."

**Elements from oral voice that should bleed into the thesis:**
1. **Analogy-first structure.** The oral work builds intuition before formalism more aggressively than the written work. The thesis should retain this rhythm — analogy or motivation, then formula, then explanation.
2. **Short declarative sentences for emphasis.** Oral scripts use them freely; written work uses them more sparingly. A thesis can afford more of them, particularly at paragraph openings and at the end of key arguments.
3. **Explicit goal declaration.** "Our mission is clear: we want to [A]; [B]; and [C]." The oral version of this is more direct than any written equivalent. A single sentence version of this structure is appropriate in experiment sections.
4. **The rhetorical closing question impulse.** In written form, this becomes a reflective closing statement — not a question, but a statement that opens toward the reader's own interpretation.

---

## Confidence Level

**High for general rules; medium-high for section-specific rules.**

The general rules (voice, transition phrases, hedging vocabulary, sentence structure, enumeration format, equation handling, precision of word choice) are strongly supported across all eight samples. The recurring phrases ("To address this limitation," "To this end," "we leverage") are confirmed across document types. The Precision & Craft rules are partly evidenced from the samples and partly from direct authorial statement — treat the stated preferences as authoritative even where textual evidence is thin.

**Sections now at medium-high confidence (upgraded from low):**

- **Related Work** — two full samples (BSc subsection-format and technical report paragraph-format) establish paragraph-level rules and the synthesis closing convention.
- **Discussion** — two samples (BSc embedded, technical report embedded) confirm the integrated structure, the "Strikingly" vs "Interestingly" distinction, named-set approach, and mechanism-led conjectures.
- **Citation style** — resolved: inline name+year + \cite{} are used concurrently in written work, not alternatively.

**What still reduces confidence from high to medium-high:**

- **Abstract conventions** — bold-term convention confirmed in only two samples, both LaTeX. Cannot confirm whether this applies in non-LaTeX contexts.
- **Discussion as a standalone section** — all samples integrate it with Results. If a chapter-level Discussion section is required by the thesis format, the conventions for that format are unconfirmed.
- **Introductions and conclusions at highest abstraction** — the "context couch" behavior is confirmed by samples and stated explicitly by the author, but its exact length and placement (how many sentences, where within the section) could vary. One or two more intro/conclusion samples would sharpen this.

**What would most improve this guide:**

1. One more Abstract and Conclusion from a project not yet sampled.
2. An example of a standalone Discussion chapter (if the thesis format requires one separate from Results).
3. Any written section where a result is entirely negative — to confirm how failure is framed when there is no partial success to report.
