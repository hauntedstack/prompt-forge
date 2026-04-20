# Prompt Forge

A 5-pass prompt optimization engine that combines 7 techniques into a single pipeline — iterating until your prompt scores 5/5 across all quality dimensions.

Prompt Forge is a [Claude Code skill](https://docs.anthropic.com/en/docs/claude-code) that takes any prompt and runs it through decomposition, bracketing, adversarial validation, and compression. It doesn't just rewrite your prompt — it scores it, attacks it, and proves the final version is better.

## How it works

You give it a prompt. It returns an optimized version with a full scorecard showing exactly what improved and why.

### The 5 Quality Dimensions

Every prompt is scored 1–5 on each dimension:

| Dim | Name | 1/5 | 5/5 |
|-----|------|-----|-----|
| K1 | **Clarity** | Ambiguous intent, LLM has to guess | Unambiguous intent, zero interpretation room |
| K2 | **Specificity** | No constraints, generic output | Precise requirements: format, length, tone, scope |
| K3 | **Structure** | Flat text block, no organization | Optimally organized for LLM cognition |
| K4 | **Robustness** | Fails on edge cases | Explicit edge case handling, graceful degradation |
| K5 | **Efficiency** | Redundant, bloated, wastes tokens | Every sentence carries unique value |

**Target: 5/5 across all dimensions. 25/25 total.**

### The 4-Pass Pipeline

**Pass 1 — Decomposition** breaks your prompt into components (critical, reinforcing, redundant, decorative), models how the LLM will actually process it, and gives an initial K1–K5 score.

**Pass 2 — Bracketing & Prioritization** generates the worst plausible output (floor) and the optimal version (ceiling), analyzes every gap between them, then builds a prioritized constraint stack (P0/P1/P2).

**Pass 3 — Optimization & Validation** writes the optimized prompt, then attacks every change with steelman counter-arguments. Changes that don't survive adversarial review get reverted or adjusted.

**Pass 4 — Blind Spot Check & Compression** hunts for misfire risks, missing perspectives, and temporal fragility. Then compresses to natural density — the point where every remaining sentence carries unique value.

### What you get

```
╔══════════════════════════════════════════════╗
║          PROMPT FORGE — RESULT               ║
╠══════════════════════════════════════════════╣
║  K1 CLARITY:      [●●●●●] 2/5 → 5/5        ║
║  K2 SPECIFICITY:  [●●●●●] 1/5 → 5/5        ║
║  K3 STRUCTURE:    [●●●●●] 3/5 → 5/5        ║
║  K4 ROBUSTNESS:   [●●●●●] 2/5 → 5/5        ║
║  K5 EFFICIENCY:   [●●●●●] 3/5 → 5/5        ║
║                                              ║
║  TOTAL: 11/25 → 25/25                       ║
╚══════════════════════════════════════════════╝
```

Plus the optimized prompt, a changelog, and a constraint satisfaction report.

## The 7 Integrated Techniques

Prompt Forge combines these prompt engineering techniques into a single pipeline:

1. **Prompt Ablation Analysis** — component-level decomposition and classification
2. **Recursive Audience Modeling** — models how the LLM processes the prompt
3. **Generative Bracketing** — floor/ceiling generation for quality calibration
4. **Constraint Priority Stack** — hierarchical requirement management (P0/P1/P2)
5. **Adversarial Claim Decomposition** — steelman validation of every change
6. **Orthogonal Critique Injection** — blind spot detection from unoptimized angles
7. **Convergent Self-Distillation** — compression to natural information density

## Installation

### As a Claude Code skill

```bash
git clone https://github.com/hauntedstack/prompt-forge.git ~/.claude/skills/prompt-forge
```

Or copy `SKILL.md` directly into any skill directory that Claude Code reads from.

### As a standalone reference

The entire engine lives in a single file: [`SKILL.md`](./SKILL.md). You can read it, adapt the methodology, or use the pass structure as a checklist when optimizing prompts manually.

## Usage

Once installed, the skill triggers when you ask Claude to optimize, improve, or evaluate a prompt. You can also say "prompt forge" or "5/5 prompt" directly.

After the initial run, you can iterate:

- `focus K4` — re-run passes 3–4 with extra weight on robustness
- `more robust` — re-run passes 2–4 with expanded edge case analysis
- `shorter` — re-run pass 4 with aggressive compression
- `explain [change]` — get a deeper rationale for a specific change

## Design principles

- **Full pipeline, always.** All 4 passes run every time. No shortcuts.
- **Show your work.** Every pass produces visible output so you can follow the reasoning.
- **Preserve intent.** The optimized prompt does what you *meant*, not what you *wrote*.
- **Score honestly.** 5/5 means the dimension cannot be improved without changing the task itself.
- **Adversarial integrity.** Steelman arguments must be real counter-arguments, not strawmen.
- **Compression ≠ shortening.** The goal is density (value per token), not brevity.
- **Lift the floor.** If improving one dimension weakens another, prioritize the weakest.

## Contributing

See [CONTRIBUTING.md](./CONTRIBUTING.md) for guidelines.

## License

[MIT](./LICENSE) — use it however you want.
