---
name: prompt-forge
description: >
  A 5-phase prompt generator pipeline that produces high-quality, structurally sound prompts for any task.
  Use this skill whenever the user asks to create, build, design, or write a prompt — or when they describe
  a task and want a prompt that handles it well. Also trigger when the user says "lag en prompt", "skriv en
  prompt", "prompt for", "make me a prompt", "prompten for X", or any variation of "I need a prompt that...".
  Trigger even when the user describes what they want to accomplish without explicitly saying "prompt" — if
  the deliverable is clearly a reusable prompt template, use this skill.
---

# Prompt Forge — 5-Phase Prompt Generator Pipeline

Build prompts that have been through requirement analysis, quality calibration, structural validation,
adversarial stress-testing, and optimal compression — not just vibed into existence.

## When the user triggers this skill

1. Ask what the prompt should accomplish (the task, the target model, the context)
2. Run all 5 phases in sequence
3. Deliver only the final compressed prompt + a brief build log

If the user provides enough context upfront, skip clarification and go straight to Phase 1.

---

## Phase 1 — Requirement Mapping (Backward Dependency Decomposition)

Before writing anything, map what the prompt *must achieve*.

```
Think about the prompt's desired outcome: [USER'S STATED GOAL]

Work backward from that outcome:

1. TERMINAL CONDITIONS — What specific things must the prompt's output satisfy?
   List each as a testable yes/no assertion.

2. DEPENDENCY TREE — For each terminal condition, ask: "What must the prompt
   explicitly instruct for this to hold?" Recurse until you reach:
   - GIVEN: The model will do this by default without instruction
   - NEEDS INSTRUCTION: Must be explicitly stated in the prompt
   - ASSUMED: Depends on the user's context — flag for the user to confirm

3. RISK NODES — Which ASSUMED dependencies carry the most risk?
   Which dependency chain is longest (= most likely failure point)?

4. ROLE CLASSIFICATION — For every element you plan to include in the prompt,
   classify it as exactly one of:
   - INPUT: the user supplies this at runtime (becomes a [VARIABLE])
   - INSTRUCTION: tells the agent what to do or how to do it
   - OUTPUT_SPEC: defines the shape, format, or quality of the result
   - CONTEXT: stable background the agent needs to operate

   If an element doesn't fit any category — e.g., a list of candidate answers,
   a pre-filtered shortlist, an example result, or an intermediate finding —
   it is SCOPE LEAKAGE. The prompt is doing the agent's job. Replace with an
   INSTRUCTION that tells the agent to produce it.

   Red flags that signal scope leakage:
   - Specific named entities (companies, products, sources) when the task is
     to find them
   - Pre-built shortlists where the task is to build a shortlist
   - Pre-filled answers where the task is to produce answers
   - "Minimum these..." enumerations that constrain what the agent considers
```

**Output of Phase 1:** A structured list of everything the prompt must contain, with priority ordering based on risk, AND every element classified by role. Scope leakage is converted to instructions before Phase 2 begins.

---

## Phase 2 — Quality Space Mapping (Generative Bracketing)

Generate two extremes to calibrate what "good" and "bad" look like for this specific prompt task.

```
FLOOR PROMPT:
Write a version of this prompt that technically asks for the right thing but
commits the most common prompt-writing failures. Label each failure inline:
[FAILURE: vague instruction], [FAILURE: no output format], [FAILURE: missing
context], etc.

CEILING PROMPT:
Write the best version you can — the result of unlimited iteration. After
writing it, name the 3-5 specific qualities that make it excellent. Not generic
qualities like "clear" — name the specific structural moves.

DELTA ANALYSIS:
What are the 3-5 most important differences between Floor and Ceiling?
Classify each as:
(a) Specificity of instruction
(b) Constraint design
(c) Output format guidance
(d) Context/role framing
(e) Error handling / edge case coverage
(f) Other — name it

Rank by impact on output quality.
```

**Output of Phase 2:** The Ceiling prompt as a working draft, plus ranked delta dimensions that define what matters most for this prompt.

---

## Phase 3 — Structural Validation (Prompt Ablation Analysis)

Take the Ceiling prompt from Phase 2 and analyze its structural mechanics.

```
Decompose the prompt into functional components (C1, C2, C3...).
Each component = one instruction, constraint, example, role, or format directive.

For each component, classify:
- CRITICAL: Output changes fundamentally without it. State what breaks.
- REINFORCING: Output degrades but remains functional. State what degrades.
- REDUNDANT: Another component already covers this. Name which one.
- DECORATIVE: No measurable effect. State why it's inert.

INTERACTION MAP:
Identify component pairs where one depends on the other's presence.
Format: "C3 + C7: C3's format constraint only works because C7 defines
the output structure it formats within."

Flag any REDUNDANT or DECORATIVE components for removal.
Flag any broken interactions where a dependency partner is weak or missing.
```

**Output of Phase 3:** A validated prompt with dead weight identified, interactions mapped, and structural integrity confirmed. Apply fixes before proceeding.

---

## Phase 4 — Adversarial Stress Test (Orthogonal Critique Injection)

Attack the prompt from angles that weren't optimized during construction.

```
List the 2-3 qualities you optimized for when building this prompt.

Now evaluate ONLY on these orthogonal dimensions — do NOT re-evaluate
the dimensions you already optimized for:

1. MISFIRE ANALYSIS: How could a model misinterpret this prompt? What's the
   most likely way it produces the *opposite* of the intended output? Name
   the specific instruction that enables the misreading.

2. ABSENCE AUDIT: What would a prompt engineering expert's first critique be?
   What edge case or failure mode did you skip because it was hard to
   instruct, not because it was irrelevant?

3. USER DIVERGENCE: Imagine three very different users pasting this prompt
   with different [VARIABLE] inputs. Which user gets the worst result?
   What about their input breaks the prompt's assumptions?

4. MODEL DIVERGENCE: How would this prompt behave differently on a weaker
   model (e.g., Haiku-class vs Opus-class)? Which instructions would be
   the first to degrade?

5. SCOPE LEAKAGE AUDIT: Does the prompt contain concrete answers, candidate
   lists, named entities, pre-filtered shortlists, or intermediate results
   that belong to the agent's runtime work? Apply the test: "If I removed
   this element, would the agent still be able to produce it themselves?"
   If yes, the element is doing the agent's job and narrows the solution
   space prematurely. Replace with an instruction that mandates the agent
   produce it, including any quality criteria the original list implied.

   Pay special attention to elements that *feel* like helpful specificity
   but actually constrain the search — e.g., "consider banks A, B, C" when
   the task is to research banks. The Ceiling prompt from Phase 2 is
   especially vulnerable here, since "best version" pressure pushes toward
   filling in answers.

6. EMPTY INPUT AUDIT: For each [VARIABLE] the prompt defines, ask what happens
   when it arrives empty, or still as a literal placeholder, at runtime. A prompt
   that silently fills the gap with assumed defaults and proceeds is broken: it
   produces confident output for a task the user never actually specified. The
   prompt must instead detect the unfilled input and either ask for it or stop
   with a short fill-in template — never invent the missing content. State inside
   the prompt the trust order for inputs: what the user actually wrote first, then
   evidence the agent can observe itself, then a default only for an input the
   prompt explicitly marks optional. Square-bracket placeholder text left unfilled
   counts as missing, never as content.

Revise to address the highest-impact finding. The revision must not break
any CRITICAL components or INTERACTION pairs from Phase 3.

If no finding rates above "minor": skip revision, proceed to Phase 5.
```

**Output of Phase 4:** A stress-tested prompt with the most dangerous failure modes patched.

---

## Phase 5 — Optimal Compression (Convergent Self-Distillation)

Compress to natural density — the point where every instruction carries unique weight.

```
ROUND 1 — COMPRESSION (target: 60% of current length):
For every cut, tag it:
[redundant] — said elsewhere in the prompt
[low-value] — true but not important enough to justify its tokens
[decorative] — no measurable effect on output

Preserve everything that would be tagged [critical] if you tried to cut it.

ROUND 2 — FIDELITY CHECK:
Compare compressed version against the Phase 4 version:
- Anything cut that shouldn't have been? Restore it.
- Anything kept that should go? Remove it.
- Any instruction distorted during compression? Fix it.

Verify all CRITICAL components from Phase 3 survived.
Verify all INTERACTION pairs from Phase 3 are intact.

ROUND 3 — CONVERGENCE TEST:
Attempt 20% further compression. If every remaining candidate cut would
remove a [critical] instruction, the prompt has converged — it's done.

Maximum 4 compression rounds total.
```

**Output of Phase 5:** The final prompt at its natural density.

---

## Delivery Format

Present to the user:

1. **The final prompt** — copy-paste ready, with `[VARIABLE]` placeholders
2. **Build log** (compact) — one sentence per phase summarizing what was found and changed:
   - Phase 1: key requirements discovered
   - Phase 2: most important quality delta
   - Phase 3: components removed or fixed
   - Phase 4: highest-impact vulnerability patched
   - Phase 5: compression ratio achieved

Do NOT show intermediate phases unless the user asks to see them.

---

## Guidelines

- Every `[VARIABLE]` placeholder must be self-explanatory. Use descriptive names like `[TARGET_AUDIENCE]`, never `[X]`.
- Maximum 4 variables per prompt. If more are needed, consolidate or provide sensible defaults.
- The final prompt must work when pasted directly — no external setup, no API access, no system prompt required.
- If the user's task is simple enough that a 2-sentence prompt would work, say so. Don't over-engineer. The pipeline is for prompts where structural quality matters.
- If the user provides a prompt they already have and wants it improved, skip Phase 1 and Phase 2 — enter at Phase 3 (ablation) with their existing prompt.
