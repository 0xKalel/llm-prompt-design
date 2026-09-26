---
name: prompt-design
description: The rules governing Ravenclip's LLM prompts. Read BEFORE editing anything in app/Ai/Agents/, app/Ai/Script/, app/Ai/Blueprint/, the prompt-feeding enums, agent instructions(), the writer/scene-director/enrichment prompts, banned families, exemplars, quality gates, or the retention critic. Triggers on: prompt, writer prompt, scene director, banned family, exemplar, hook, script generation rules, "the model keeps saying X".
---

# Prompt design (pointer skill)

The canonical document is **`docs/prompt-design.md`** — read the sections that
match your change before editing. This skill exists so that doc actually gets
read; it carries only the load-bearing rules.

## The three laws (writer prompt = identity + laws + mechanical contract)

- **TRUE** — say only what the source states (attributed analysis, no invented
  crowds, framing from spine/claims never detached facts, outside-reporter
  voice, under-claim when in doubt).
- **DENSE** — every sentence teaches the viewer something new from the source
  (this one law *generates* no-repetition, no-filler, no-restated-hook,
  no-shrug-endings).
- **SPOKEN** — one person telling one story; lines continue each other; the
  last line is the strongest concrete thing the source leaves — stated, not
  asked — and nothing comes after the answer.

## Maintenance rules (the ones edits usually violate)

1. **Sharpen a law, never append a ban.** A new failure mode means a law wasn't
   sharp enough — fix the law (§12 of the doc). Rule piles don't generalize;
   the model can reason from a law, not from rule #27.
2. **One prototype exemplar per banned family — exactly one** (§2). Earned
   exemplars stay verbatim and tests pin them (§7).
3. **Artifact truths, not pipeline narration** (§3): the prompt states truths
   about the artifact, never how the pipeline works.
4. **Static vs dynamic** (§8 + CLAUDE.md): agent `instructions()` are STATIC —
   no per-call/channel/article data — so providers can cache the prefix. The
   scene director's per-channel block goes in the USER message via
   `SceneDirectorAgent::channelContextBlock()`. Prompt SIZE is the real cost
   lever (implicit Gemini caching is empirically dead here).
5. **Remove antagonists instead of adding counterweights** (§4); laws must
   never promise freedom the mechanics revoke (§5).

## Where things live

- Writer/script agents: `app/Ai/Agents/`, `app/Ai/Script/` (curated verticals
  extend `CuratedSceneDirector`), blueprint/selection: `app/Ai/Blueprint/`.
- Schema-as-prompt (`adds` field): doc §6. Gemini schema array-bound limit —
  see the memory of the same name before widening arrays.
- Hook/retention: doc §14–16 (promise-proof-progression-payoff, retention
  critic). Visual prompts: §17 (one look, no screens, motion only).
- After editing prompts: `script:critique {script}` on a fresh generation, and
  run the tests that pin the earned exemplars (grep `tests/` for the exemplar
  text you touched) before calling it done.
