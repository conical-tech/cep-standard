# Cognitive Expression Profile (CEP)

**A framework for domain-specific psychological assessment via LLM-based behavioral observation**

> v0.1 — Working Draft | March 2026 | Connor Teague, Conical Tech

---

## Overview

Existing psychometric frameworks — most notably the Big Five personality traits — were designed to measure cross-situational consistency: how stable a person's behavior is across different contexts. That design assumption is appropriate for clinical and organizational psychology. But it systematically obscures something potentially more useful: how a person's cognitive expression *varies* across domains, and what that variance reveals.

The Cognitive Expression Profile (CEP) is a psychometric framework designed specifically to capture domain-specific psychological expression through behavioral observation, with large language models as the primary assessment instrument. The central hypothesis is that trait expression varies across domains in systematic, meaningful ways — that this variance is not measurement noise but signal — and that the structure of that variance provides insight that cross-situational frameworks cannot.

This repository contains the theoretical framework, falsification criteria, and open specification for CEP. It serves as the intellectual priority record for the framework and the reference implementation built by [Conical Technology, LLC](https://conical-tech.com).

---

## The Core Hypothesis

The framework rests on four empirically testable claims:

1. Domain-specific trait expression is systematically measurable from structured debate behavior.
2. A person's trait profile in one domain is predictive of (but not identical to) their profile in other domains — within-person cross-domain correlation is greater than chance but less than 1.
3. The delta between a person's profiles across domains is not random; it has structure that reflects meaningful psychological characteristics (expertise, identity investment, domain familiarity).
4. The cross-domain variance model is more predictive of domain-specific behavior than any domain-agnostic personality profile.

Claims 1 and 2 are sufficient to validate the measurement approach. Claims 3 and 4 are the theoretical contributions that distinguish CEP from a simple per-domain Big Five replication.

### Falsification Criteria

The framework is falsified if any of the following are demonstrated:

- Domain-specific trait scores inferred from debate transcripts do not correlate with self-reported domain-specific behavior at above-chance levels.
- Within-person cross-domain trait correlations are not significantly greater than between-person correlations.
- Cross-domain delta scores are uncorrelated with any meaningful external criterion (domain expertise level, identity investment, observed behavioral outcomes).
- Domain-agnostic Big Five profiles predict domain-specific behavior as well as or better than domain-specific CEP profiles.

---

## Why Large Language Models Make This Tractable Now

Cross-domain assessment has always been conceptually interesting but practically difficult. Traditional psychometric assessment requires either lengthy questionnaires administered repeatedly across contexts or trained observer coding of naturalistic behavior in multiple domains — both impractical at scale.

Large language models change this through two distinct contributions delivered by the same technology:

**As the assessment instrument.** The CEP methodology requires a structured debate with a capable interlocutor that can generate domain-relevant challenges, ask genuine deepening questions, and adapt in real time. This was not possible with rules-based systems or classical NLP.

**As the measurement pipeline.** Behavioral signals previously invisible to automated analysis — argument structure under challenge, values-claims vs. evidence-claims ratio, novel framing introduction rate, confidence language shifts, synthesis vs. replacement behavior — can now be extracted reliably because LLMs reason about the semantic structure of arguments, not just surface features. Classical NLP could parse sentences; LLMs can identify that a person replaced their position rather than synthesized a challenge.

A third enabling condition is inference economics. Running this kind of assessment at consumer scale requires large model inference at a cost point that makes it viable as a product. That became true in 2023.

---

## Framework Architecture

### Domain Layer

CEP operates at the domain level. A domain is a coherent area of knowledge and practice in which a person can hold and defend positions. The framework requires a canonical domain taxonomy to ensure comparability across assessments and implementations.

Initial taxonomy:

| Tier | Scope | Examples |
|------|-------|---------|
| 1: Validation | Broad population accessibility, low stakes, high variance | Personal finance, physical fitness, nutrition, outdoor recreation, technology adoption |
| 2: Expansion | Higher identity investment, more contextual | Career development, interpersonal relationships, creative practice, civic engagement |
| 3: Professional | Domain-specific expertise required | Reserved for later, higher-stakes applications |

Tier 1 was selected because people hold genuine, defensible opinions in these domains, stakes are low enough that authentic expression is likely, and variance in engagement styles is high and easily observable.

### Trait Schema

CEP v0.1 uses a modified Big Five schema as initial scaffolding — not as a theoretical commitment, but as a defensible starting point with 50 years of validation literature that allows direct comparison to existing research.

The Big Five scaffolding is acknowledged to be imperfect for this purpose: the dimensions were designed to measure cross-situational stability (orthogonal to the variance CEP wants to capture) and were constructed via factor analysis of adjective ratings rather than behavioral observation.

**CEP v1.0** will incorporate LLM observable dimensions derived from factor analysis of behavioral signals extracted from a large assessment dataset. The transition from v0.1 to v1.0 is the primary theoretical contribution of the research program — discovering the natural dimensional structure of domain-specific cognitive expression as revealed by language.

### LLM-observable Behavioral Dimensions

Regardless of the trait schema used for scoring, the following behavioral dimensions are directly observable from debate transcripts and instrumented from the first implementation. These are the raw signal from which trait scores are derived and the empirical basis for the v1.0 factor analysis:

| Dimension | What it measures |
|-----------|-----------------|
| Argument structure stability | Does structure remain consistent under challenge, or shift? |
| Values-claims vs. evidence-claims ratio | Does the person ground positions in values or evidence? |
| Novel framing introduction rate | Does the person introduce new conceptual frames, or elaborate existing ones? |
| Confidence calibration | Does expressed confidence track argument strength, or is it invariant? |
| Synthesis vs. replacement behavior | When challenged, does the person integrate or replace their position? |
| Uncertainty acknowledgment | Does the person mark genuine uncertainty, or express performed certainty? |
| Revision depth | When updating a position, is the revision structural or surface restatement? |
| Adversarial engagement quality | Does the person engage with the strongest version of opposing arguments? |

These dimensions do not map cleanly onto Big Five and are not intended to. They are instrumented alongside Big Five-derived scores so their structure can be analyzed independently.

### The Cross-Domain Model

The CEP for any individual consists of:

- **Per-domain profile** — trait scores and behavioral dimension scores for each assessed domain.
- **Cross-domain model** — how profiles vary across domains: the mean profile (center of gravity), variance structure (which dimensions vary most vs. least), and correlation structure (which domains cluster together vs. are most dissimilar).
- **Confidence envelope** — uncertainty estimates for each score, updated Bayesian-style as more assessment data accumulates.

The cross-domain model is the primary novel contribution over simple per-domain personality assessment. The hypothesis is that the structure of intra-person domain variance is itself informative in ways no individual domain profile captures.

---

## Assessment Methodology

### Structured Debate as Assessment Mechanism

The core assessment mechanism is structured debate: a person defends and challenges positions on domain-relevant questions in conversation with one or more AI interlocutors. Selected over questionnaire-based assessment because:

- **Behavioral authenticity** — debate behavior is harder to consciously manage than questionnaire responses; argument structure and revision behavior emerge naturally under challenge.
- **Rich behavioral signal** — a debate transcript contains multiple instances of each behavioral dimension; a questionnaire generates a single point estimate.
- **Ecological validity** — people naturally argue about topics they care about; the format aligns with how domain-specific opinions are actually expressed.
- **Scalability** — structured debate with AI interlocutors scales without human observers.

### Debate Structure

Each domain assessment consists of seven steps:

1. **Topic selection** — 2-3 debate topics presented; person selects one. Topics calibrated to elicit genuine opinion, not factual questions with correct answers.
2. **Independent position statement** — person states initial position without interlocutor input. Non-negotiable; prevents anchoring to the interlocutor's framing.
3. **Challenge round** — AI interlocutor presents the strongest case against the person's position.
4. **Response round** — person responds to the challenge. Primary signal-generating phase.
5. **Deepening** — interlocutor asks for values articulation: "Why does that matter to you?" or "What would change your mind?" Elicits values-claims vs. evidence-claims signal.
6. **Second challenge** — a second, different challenge assesses revision behavior under continued pressure.
7. **Synthesis prompt** — interlocutor asks the person to characterize the strongest version of the opposing view. Generates adversarial engagement quality signal.

> Challenge intensity is tuned to produce genuine engagement rather than defensive shutdown. The goal is productive discomfort. This calibration is domain-dependent and a key implementation variable.

### Inference Pipeline

The inference pipeline takes a debate transcript as input and produces:

- Behavioral dimension scores (continuous scale for each dimension in Section 3.3)
- Big Five-mapped trait scores (via learned or hand-specified mapping)
- Bayesian update (integrated with prior assessments, weighted by assessment quality signals)
- Confidence estimates (uncertainty propagated through the update)

The inference pipeline is an implementation detail, not part of the CEP specification. The spec defines the output format, not the derivation.

### User Correction

All inferences are shown to the user with the reasoning behind them. The user can correct any inference. Corrections feed back into the Bayesian update as high-confidence signals.

The design philosophy: the user is the ground truth; the assessment is an approximation that should converge on the user's self-understanding over time, not resist it. This is also a methodological decision — self-report and behavioral observation often diverge, and tracking both (and the delta) is itself informative.

---

## The CEP Open Standard

### Specification vs. Implementation

CEP is an open standard. The C-CEP (Conical Cognitive Expression Profile) is Conical Technology, LLC's reference implementation. The distinction matters:

- **Portability** — a user's profile should be exportable to any platform that implements the CEP spec. The user owns their profile; no single implementation has exclusive access.
- **Scientific credibility** — an open standard invites scrutiny, replication, and contribution. A proprietary framework is a product; an open standard is an institution.

### What the Spec Defines

| In scope | Out of scope |
|----------|-------------|
| Domain taxonomy (canonical names, scope, boundary conditions) | Assessment methodology |
| Trait schema and dimension definitions (versioned) | Inference implementation |
| Profile data format (JSON schema: per-domain scores, cross-domain model, confidence envelopes, metadata) | Storage architecture |
| Portability requirements (export format, cryptographic signing, consent metadata) | Update rules |
| Compliance checklist | Derived artifacts computed from the profile |

Derived artifacts computed from a CEP profile are implementation-specific and outside the scope of the specification.

### Versioning

| Increment | Meaning |
|-----------|---------|
| Major (1.x → 2.x) | Changes to dimension definitions that break backward compatibility |
| Minor (x.1 → x.2) | Additive changes (new domains, new optional dimensions) — backward compatible |

The transition from Big Five scaffolding to LLM-observable dimensions constitutes a major version increment and is the primary research milestone for v2.0.

---

## Intellectual Priority Record

This document asserts intellectual priority for the following, as of the date of first commit:

1. **The domain-variance hypothesis** as a testable psychometric framework — that Big Five systematically obscures meaningful intra-person cross-domain variance, and that this variance is more informative about domain-specific behavior than cross-situational profiles.
2. **Structured debate as a psychometric assessment instrument** — specifically, the use of position defense and challenge response behavior to infer domain-specific cognitive expression via large language model analysis.
3. **The LLM-observable behavioral dimensions** defined above as a proposed alternative dimensional structure to Big Five for domain-specific assessment.
4. **The CEP data format** as an open standard for portable, user-owned psychological profiles.
5. **C-CEP** as a reference implementation of the CEP standard, including the assessment pipeline, Bayesian update mechanism, and cross-domain model.

### Prior Work

| Work | Relationship to CEP |
|------|-------------------|
| Big Five (Costa & McCrae, 1992+) | Scaffolding for v0.1. CEP addresses a question Big Five was not designed to answer. |
| Multi-agent debate research (Du et al., 2023; arXiv:2305.14325) | Validates structured AI debate for eliciting diverse, well-reasoned responses. Empirical support for assessment mechanism. |
| Within-person variability research (Fleeson, 2001; Mischel & Shoda, 1995) | CEP builds on this tradition, distinguished by LLM-based methodology, cross-domain comparative framework, and explicit focus on variance structure. |
| W3C DID specification | C-CEP identity architecture uses standard DID methods. No priority claim on identity infrastructure. |

### Open Questions

The following are unresolved and constitute the primary research program:

- What is the natural dimensional structure of domain-specific cognitive expression? (Central empirical question for v1.0)
- How many debate rounds are required for stable trait inference? (Hypothesis: 2-3 per domain)
- What is the optimal challenge intensity calibration?
- How does the cross-domain model change with increasing expertise?
- What are the ethical implications of portable psychological profiles?

---

## Intended Applications

The C-CEP functions as a coordination primitive for AI agent systems. Its primary role is not to represent the user to others, but to enable agents to calibrate their collaborative behavior toward the user in service of a stated goal. The result is assistance that moves beyond generic interaction into genuine cognitive collaboration, calibrated to how a specific person reasons in a specific domain.

**Single-agent context.** A C-CEP enables an agent to dynamically adopt complementary cognitive stances across the arc of a collaborative session — stress-tester, synthesizer, lateral thinker, domain anchor — shifting as the task evolves. This distinguishes CEP-driven assistance from existing personalization approaches, which adapt communication style but not cognitive role.

**Multi-agent context.** The same coordination logic distributes complementary roles across a collection of agents. Multi-agent presentation is a social interface design decision, not a technical requirement — humans process distinct voices differently than a single shifting voice, and for tasks where the user benefits from feeling genuine tension between perspectives, distinct voices make cognitive diversity legible in a way that maps naturally onto how humans navigate disagreement.

**Research dataset.** Operating the C-CEP system at scale produces a longitudinal dataset of domain-specific cognitive profiles with repeat assessments over time. This is a byproduct of the product, not a primary application, but it forms the empirical basis for the v1.0 framework revision: factor analysis of LLM-observable behavioral dimensions across this dataset will reveal the natural dimensional structure of domain-specific cognitive expression.

All research use is subject to explicit opt-in consent, differential privacy protections, and data minimization. Raw transcripts are not retained.

---

## Repository Structure

```
/
├── README.md                  # This document — framework overview and priority record
├── framework/
│   └── (forthcoming)   # Full framework document
├── spec/
│   └── (forthcoming)          # CEP JSON schema and domain taxonomy
└── research/
    └── (forthcoming)          # Validation study design and preliminary findings
```

---

## License

The CEP specification is released under [Creative Commons Attribution 4.0 (CC BY 4.0)](https://creativecommons.org/licenses/by/4.0/). You are free to implement, adapt, and build on the specification with attribution.

The C-CEP reference implementation is proprietary to Conical Tech.

---

*This document was first committed March 2, 2026 and constitutes the intellectual priority record for the CEP framework. Connor Teague, Conical Tech.*
