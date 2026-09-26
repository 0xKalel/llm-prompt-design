# llm-prompt-design

The **pointer-skill pattern** from [RavenClip](https://ravenclip.com), a production SaaS that writes, voices and publishes short news videos unattended: how to make a long, hard-won doctrine actually get read by coding agents.

The artifact here is **[`skill/SKILL.md`](skill/SKILL.md)**, verbatim from the running project. The 879-line doctrine it points at stays private — it is RavenClip's playbook, and every line of it was paid for with a production failure. This repo publishes the *shape* that keeps such a document alive, and the transferable ideas behind it.

## The ideas that transfer

**Laws, not rule piles.** The writer prompt once had ~9 sections of specific bans, and quality work was whack-a-mole: every new failure got a new rule, and the model can't reason from rule #27. The fix: three laws (TRUE, DENSE, SPOKEN) sharp enough to *generate* the bans. When a new failure appears, the maintenance rule is "sharpen a law, never append a ban."

**One prototype exemplar per banned family — exactly one.** Exemplars are the strongest steering signal, and also the most expensive one; a family of banned clichés gets its single most representative member, and earned exemplars are pinned verbatim by tests so an edit can't silently weaken them.

**Artifact truths, not pipeline narration.** The prompt states truths about the artifact being produced, never how the pipeline works. The model doesn't need to know there is a pipeline.

**Remove antagonists instead of adding counterweights.** A prompt fighting itself — one line pushing toward the exact ending another line bans — doesn't need a third line arbitrating. It needs the antagonist deleted. The best example: an enum's `instruction()` text was quietly pushing half of all scripts toward the filler ending the writer laws were fighting. An antagonist can hide in an enum.

**Static vs dynamic, because prompt size is the cost lever.** Agent instructions are static so providers can cache the prefix; per-channel context rides the user message. The measured reality that forced this discipline: implicit provider-side caching was empirically dead here, so every token is paid full price on every call.

**Schema-as-prompt.** Field descriptions in the output schema are prompt surface too — sometimes the cheapest place to steer the model.

## The pointer-skill pattern

The skill solves the reading problem:

- Its **description** names the files, symbols and phrases that should trigger it ("prompt", "banned family", "the model keeps saying X"), so the agent loads it exactly when it's about to edit prompt surface.
- Its **body carries only the load-bearing rules** — the three laws and the five maintenance rules that edits usually violate — and *points* at the canonical doc, section by section, for everything else.
- The canonical doc stays the single source of truth. The skill never duplicates it beyond the survival minimum, so the two can't drift apart.

The doctrine's real problem was never writing it — it was that nobody (human or agent) reads 879 lines before a quick prompt edit. The skill is what makes the doc real.

A skill that summarizes would rot. A skill that points, with just enough teeth to stop the common mistakes, keeps a big document alive.

## Using it in your own project

Copy the shape, not the content: keep one canonical doc per hard-won domain, and give your agent a pointer skill whose description matches the moments that doc should interrupt. The laws themselves are RavenClip's; yours have to be paid for by your own failures — that's rather the point of the doctrine.

---

Part of how I ship with agents — specs in, human review before anything lands. More at [0xkalel.github.io/how-i-work](https://0xkalel.github.io/how-i-work/), and the guard-hook half of the setup is at [claude-code-guard-hooks](https://github.com/0xKalel/claude-code-guard-hooks).
