# Production-Grade AI Systems

**Standards and a Lived Case Study**
*By Derrick Weil — Control Catalog v1.0*

A standards-driven definition of what "production-grade" means for AI-assisted systems, written from the perspective of the person on-call at 2AM.

📄 **[Read the book (PDF)](./Production-Grade%20AI%20Systems.pdf)**

---

## What This Is

Most writing about operating AI systems is either a checklist of best practices or a set of vendor-flavored reference architectures. This book is neither.

It is a **closed catalog of non-negotiable controls** — written in RFC-style normative language (MUST / SHOULD / MUST NOT) — that define the minimum behavior an AI-assisted system must exhibit to be considered safe to operate under real-world stress.

Every control in the catalog exists because a real system failed in a way that was slow, ambiguous, expensive, or risky to recover from. Controls that could not be justified by observed failure were discarded. The catalog is intentionally small, intentionally strict, and intentionally provider-agnostic.

## The Central Claim

> Production is not a deployment event. It is a behavioral state.
>
> A system becomes production-grade not when it returns correct answers, but when it continues to behave predictably, explainably, and recoverably under failure.

Traditional software tends to fail loudly. AI-assisted systems fail **plausibly** — they keep returning responses, keep completing workflows, and keep consuming resources while internal guarantees silently degrade. Confidence erodes before errors spike. This book defines the minimum set of constraints required to prevent that.

## Who This Is For

This book is written for people responsible for AI systems *after* deployment:

- **Senior engineers and architects** designing AI-assisted systems that will run beyond a demo
- **Site reliability engineers and platform teams** inheriting AI workloads and asking "how do I operate this?"
- **Security engineers** evaluating whether an AI system is safe to expose
- **Technical decision-makers** who need a precise definition of "production-grade" that can't be softened in a meeting

It assumes you have operated production systems and know what it feels like when things go wrong at 2AM. It does not assume you have built AI systems before.

## What "Production-Grade" Means Here

Production-grade does **not** mean accurate, fast, cheap, or popular. Those qualities may matter, but none of them guarantee survivability.

A production-grade AI system, as defined in this book, is one that:

- makes failure visible rather than silent
- bounds the blast radius of incorrect behavior
- preserves enough evidence to diagnose what happened
- attributes cost and responsibility explicitly
- allows humans to intervene without improvisation

Production-grade is a **threshold, not a gradient**. A system either satisfies the controls or it does not. "Mostly production-ready" is not a recognized state.

## Scope

**In scope:** AI-assisted systems that include one or more of — dynamic or AI-driven execution, retrieval-augmented generation, multi-step or long-lived state, cost-bearing workloads, or human intervention during operation.

**Out of scope:** Static chatbots, stateless inference APIs, research notebooks, offline experimentation, model training and fine-tuning, alignment research, prompt optimization.

Systems outside the scope may not require this level of rigor. Systems inside it eventually do.

## The Control Catalog

The catalog is closed. A system may only claim compliance against these controls.

### Security & Ownership
- **SEC-01 — Identity & Session Integrity.** Every operation is attributable to a stable principal whose ownership persists across retries, reconnects, and background work.
- **SEC-02 — Sandboxed Execution Isolation.** Dynamic execution occurs inside bounded, isolated environments with deterministic termination.
- **SEC-03 — Output Handling and Downstream Safety.** AI-generated outputs are treated as untrusted input and validated before influencing state or action.

### Observability
- **OBS-05 — Prompt, Context, and Cost Traceability.** AI behavior is reconstructible after the fact — inputs, retrieved context, execution steps, and attributable cost.

### AI-Specific Correctness
- **AI-02 — Retrieval Integrity and Drift Control.** Retrieval is treated as execution, not data access. Its behavior is observable, attributable, bounded, and drift is diagnosable.

### Reliability & State
- **REL-01 — Explicit State Transitions.** Progress is defined by validated state transitions, not request completion.
- **REL-04 — Bounded Retries and Degraded-Mode Behavior.** Retries are bounded per logical operation and degrade into a defined state when exhausted.

### Cost & Abuse
- **CST-03 — Per-Principal Cost Budgets and Abuse Guards.** Cost is attributable, bounded, and automatically enforced at execution time.

### Operations
- **OPS-02 — Disk, Log, and Artifact Retention Boundaries.** Storage usage and retention are explicit, enforced, and observable.
- **OPS-04 — Human-in-the-Loop Intervention and Recovery.** Operators can inspect, intervene, and recover without guesswork.

Each chapter in the book defines one control using a fixed structure: *Why This Control Exists → Observed Failure → Why It Was Hard → Correction → Standard → Failure Modes → Design Invariants → Verification → Operator's View (2AM Test)*.

## The 2AM Test

Every control chapter ends with the same question set, in operator voice. If a system cannot answer these under pressure, it has not crossed the threshold:

- *What is this system doing right now?*
- *Why is it doing it?*
- *Who owns this behavior?*
- *What happens if I stop it?*
- *What happens if I don't?*

The rest of the book is a catalog of the constraints a system needs in order to answer those questions confidently.

## How to Read It

- **If you have 5 minutes:** Read Chapter 1 ("Production Is a Behavior, Not a Deployment") and Chapter 14 ("Controls Fail Alone"). Those two chapters carry the thesis.
- **If you have 30 minutes:** Read Part I (chapters 1–3) and skim Appendix A for the catalog.
- **If you're evaluating a real system:** Read Part II (chapters 4–13), one control at a time, and apply the 2AM Test to the system in front of you.
- **If you're migrating an existing system toward compliance:** Chapter 17 covers the three realistic migration patterns and the tradeoffs involved.

## What This Book Does Not Do

Being explicit about this matters:

- It does not improve model quality, accuracy, or alignment.
- It does not optimize for latency, throughput, or cost efficiency.
- It does not prescribe tools, vendors, frameworks, or platforms.
- It does not claim the catalog is the only set of controls that matter — it claims these are the minimum.
- It does not eliminate engineering judgment. It bounds it.

## Status

Control Catalog v1.0. Chapter 15 is marked as draft content in the current PDF and will be finalized in a later revision. The catalog IDs (SEC-01, OBS-05, etc.) are stable and will not be reused.

## Feedback

Issues and discussion welcome. If you are operating a system that violates one of these controls and want to push back on whether the control is justified, that is exactly the kind of conversation this catalog was written to support.

## License

MIT — free to read, reference, quote, and build on. Attribution appreciated.
