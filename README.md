# Prompt Forge

A 5-phase prompt engineering pipeline that produces structurally validated, stress-tested prompts — not vibed into existence.

Prompt Forge is a [Claude Code skill](https://docs.anthropic.com/en/docs/claude-code) that runs as an automated pipeline inside your Claude session. Give it a task description, and it returns a production-ready prompt that has been through requirement analysis, quality calibration, structural validation, adversarial stress-testing, and optimal compression.

## Why this exists

Most prompts are written in one pass and hoped for the best. Prompt Forge applies an engineering methodology: decompose the problem, generate quality boundaries, validate structure, attack from blind spots, then compress to natural density. Each phase catches failures the previous one missed.

## The 5 Phases

| Phase | Name | What it does |
|-------|------|--------------|
| 1 | **Requirement Mapping** | Works backward from the desired outcome to identify every instruction the prompt must contain, flagging assumptions and risk nodes |
| 2 | **Quality Space Mapping** | Generates a floor (worst plausible) and ceiling (best possible) version, then identifies the specific deltas that matter |
| 3 | **Structural Validation** | Decomposes the prompt into functional components, classifies each as critical/reinforcing/redundant/decorative, and maps interactions |
| 4 | **Adversarial Stress Test** | Attacks from angles that weren't optimized during construction — misfire analysis, absence audit, user divergence, model divergence |
| 5 | **Optimal Compression** | Multi-round compression to natural density, with fidelity checks to ensure nothing critical was lost |

## What you get

- A **copy-paste ready prompt** with self-explanatory `[VARIABLE]` placeholders
- A **compact build log** — one sentence per phase summarizing what was found and changed

## Installation

### As a Claude Code skill

```bash
# Clone to your Claude Code skills directory
git clone https://github.com/hauntedstack/prompt-forge.git ~/.claude/skills/prompt-forge
```

Or copy `SKILL.md` directly into any skill directory that Claude Code reads from.

### As a standalone reference

The entire pipeline lives in a single file: [`SKILL.md`](./SKILL.md). You can read it, adapt the methodology to your own workflow, or use the phase structure as a mental checklist when writing prompts manually.

## Usage

Once installed as a Claude Code skill, the pipeline triggers automatically when you:

- Ask Claude to "create a prompt", "write a prompt", or "make me a prompt"
- Describe a task where the deliverable is clearly a reusable prompt template
- Say "prompt for [X]" in any language (English, Norwegian, etc.)

### Example

> "Make me a prompt that takes a messy meeting transcript and produces a structured action-item list with owners, deadlines, and dependencies."

Claude will run all 5 phases and deliver the final prompt with a build log.

### Improving an existing prompt

If you provide a prompt you already have and ask for improvements, the pipeline enters at Phase 3 (structural validation) — skipping requirement mapping and quality calibration since you already have a working draft.

## Design principles

- **Every phase earns its place.** Each phase catches a category of failure that previous phases miss.
- **Don't over-engineer.** If a 2-sentence prompt would work, the pipeline says so.
- **Maximum 4 variables.** Prompts should be self-contained and easy to use.
- **No external dependencies.** The final prompt works when pasted directly — no API keys, no system prompts, no setup.

## Contributing

See [CONTRIBUTING.md](./CONTRIBUTING.md) for guidelines on how to improve the pipeline, add phases, or adapt it for specific domains.

## License

[MIT](./LICENSE) — use it however you want.
