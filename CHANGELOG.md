# Changelog

## 2.0.0

2.0 is published as a separate skill from 1.0 (the 4-pass Norwegian optimizer), not an update to it.

### Basis
`SKILL.md` is the 5-phase generator pipeline that was used as the test baseline, plus exactly one
measured change.

### Added
- **Phase 4, item 6 — EMPTY INPUT AUDIT.** For each `[VARIABLE]` the generated prompt defines, the
  pipeline now requires the prompt to handle the field arriving empty or as a literal placeholder:
  detect it and ask, or stop with a short fill-in template, never invent the missing content. The
  prompt must also state its input trust order (what the user wrote → observable evidence → a default
  only for an input explicitly marked optional), and treat unfilled square-bracket placeholders as
  missing, not as content.

### Why (measured, not assumed)
Two independent observations showed the 5-phase generator producing prompts that, on blank input,
fabricated a project and planned it with confidence. A controlled run (frozen baseline vs. this one
change, n=6 per condition, equal tool access, blind grading) showed:

| Test case            | Baseline | 2.0 (edited) |
|----------------------|----------|--------------|
| Finish-plan          | 75%      | 100%         |
| Bank (scope leakage) | 100%     | 100%         |
| Work-profile         | 100%     | 100%         |
| /review-done         | 100%     | 100%         |

The movement is entirely in the finish-plan case: blank-input handling went 0/6 → 6/6 while the other
criteria held, and the other three cases showed no regression. Cost was ~1.0x (121k vs 125k tokens,
slightly faster). A blind, counterbalanced panel of 5 judges preferred the edited version 5/5 on both
empty-input handling and overall prompt quality.

### Limitations
- Four test cases; one grader-model family (Opus) for grading and panel; Sonnet as generator.
- The empty-input fix is validated on blank input and static review of the generated prompts; the
  generated prompts were not executed on fully-filled input.
- n=6 is small. 0/6 → 6/6 plus a unanimous panel is strong but not unbounded evidence.
