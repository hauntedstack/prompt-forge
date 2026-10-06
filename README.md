# Prompt Forge 2.0

A 5-phase prompt **generator** pipeline: it turns a task description into a high-quality,
structurally sound, copy-paste-ready prompt. Phases: Requirement Mapping (backward dependency
decomposition) → Quality-Space Mapping (floor/ceiling bracketing) → Structural Validation
(ablation analysis) → Adversarial Stress Test → Optimal Compression.

## Relationship to 1.0

This is **not** an update of the original [`prompt-forge`](https://github.com/hauntedstack/prompt-forge)
(now "1.0"). 1.0 is a different skill: a 4-pass Norwegian prompt **optimizer** that scores an existing
prompt across five dimensions (K1–K5) toward 25/25. 2.0 is a generator with a different structure and
purpose. They are kept as separate skills on purpose; 1.0 is untouched.

> Note: `SKILL.md` keeps `name: prompt-forge`. If you install both 1.0 and 2.0 in the same skills
> directory, rename one to avoid a name collision.

## What is measured in 2.0

2.0's `SKILL.md` is the exact artifact validated in a controlled test, not a hand-tuned rewrite.
The one change over the tested 5-phase baseline is **Phase 4, item 6 — EMPTY INPUT AUDIT**: it forces
a generated prompt to detect when its own `[VARIABLE]` fields arrive empty or as bare placeholders, and
to ask for them or stop with a fill-in template instead of silently inventing content.

Evidence for that single change (frozen baseline vs. edited, n=6 per condition, blind grading):

- Blank-input handling on the finish-plan task: **0/6 → 6/6** (baseline invented a project and planned
  it; edited asked for the missing fields).
- No regression: the three other test cases held at 100% before and after.
- Cost essentially unchanged (~121k vs ~125k tokens per run).
- A blind, counterbalanced 5-judge panel preferred the edited version **5/5** on both "handles empty
  input better" and "better-designed reusable prompt overall".

See `CHANGELOG.md` for the full detail and the limitations of the test.

## Install

Copy `SKILL.md` into a `prompt-forge/` folder under your Claude skills directory
(`~/.claude/skills/prompt-forge/SKILL.md`), or wherever your skill library is synced from.

## License

MIT (see `LICENSE`).
