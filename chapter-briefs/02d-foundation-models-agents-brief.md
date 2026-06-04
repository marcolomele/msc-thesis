# Chapter Brief: 2d — LLM Agents

---

**Notation discipline:** Use the notation contract in `theory-brief.md` §0 verbatim.
Honour the canonical/local rule. Do not reinvent symbols. Reuses $\attn$/$\mhattn$ (2a)
and the autoregressive distribution $p_\theta(x_i\mid x_{<i})$; **defines the canonical
ReAct loop symbols** (thought/action/observation) referenced by Chapter 4.

You are helping me draft Subchapter 2d of my master's thesis. The project
knowledge files contain my style guide, scope, writing samples, and thesis
outline. Read them before drafting.

**Chapter**: 2d — LLM Agents
**Target length**: ~2 pages
**Arc**: C — Our-use first (frame around how the pipeline uses Qwen as a ReAct agent, then ground the mechanisms)
**Deep treatment**: definition of Large Languag Model, definition of Agent, ReAct interleaved Thought→Action→Observation loop; tool-use / function-calling as a typed JSON action space; multi-turn agent design with history pruning (the pipeline's exact loop). Important note here is that model used in final version of pipeline is Qwen 3.5 35B.
**Surface only**: Qwen3.5 architecture (GQA/RoPE/RMSNorm/SwiGLU, one para); scaling laws + RLHF/DPO (one para — why instruction tuning gives reliable JSON)
**Skip**: LLM pretraining mechanics beyond naming next-token + cross-entropy; Toolformer self-supervision detail (contrast in one line vs the supervised schema used here), previous versions of Qwen
**Connect back to**: 2a.1 (attention — causal mask, KV-cache), 2c.3 (SAM3 agentic interface — the planner this chapter formalises)
**Connect forward to**: 4 (Block 3 implements this ReAct loop: segment_phrase → SAM3 masks → examine_each_mask verification, ≤15 generations with pruning + duplicate rejection)

**Local notation to harmonise a posteriori** (footnote originals on first use):
Qwen GQA/RoPE/RMSNorm specifics; ReAct trace symbols; tool-schema JSON notation.

**[ADD] macros introduced here**: none new.

**Theory content + bibtex keys**: paste from `thesis-theory-topics.md`
§ "Chapter 2d — Foundation Models & Agents" (2d.1–2d.3). Keys: `kaplan2020scaling`,
`hoffmann2022chinchilla`, `ouyang2022instructgpt`, `rafailov2023dpo`, `bai2023qwen`,
`wang2024qwen2vl`, `qwen35blog`, `yao2023react`, `schick2023toolformer`, `wei2022cot`.
Pipeline link: `agent_core.py` ReAct loop with Qwen3.5-35B via Ollama; failure-mode
taxonomy (repeated actions, hallucinated args, reward hacking) + recovery strategies.

Draft the full chapter now. Write in my voice as documented in the style guide.
Use LaTeX formatting throughout. After the draft, flag any decisions you made.

---

## Post-Draft Checklist

- [ ] Reuses attention/LM symbols from 2a; defines ReAct trace cleanly
- [ ] Voice matches style guide
- [ ] Every equation/loop has a component-by-component explanation in prose
- [ ] Backward links to 2a/2c + forward link to Block 3 present
- [ ] LLM pretraining not over-explained (§2 Skip); Qwen internals kept to one para
- [ ] Citations use inline name+year + \cite{key} format
- [ ] Nothing sounds like it was written by an AI
