# Project 04 – Guardrail Architecture

## Overview

This project demonstrates the design and evaluation of a **guardrail architecture** for a large language model (LLM) persona. The objective was to create a prompt that consistently enforces a fixed identity and behavioral constraints, while resisting common prompt injection and jailbreak attempts.

The resulting system, **Sage**, is a Socratic coding tutor that guides users through questions instead of providing direct solutions or code.

---

## Objectives

* Design a robust system prompt with layered guardrails.
* Prevent prompt injection and persona override attacks.
* Enforce a permanent tutoring identity.
* Restrict all forms of code generation, including encoded code.
* Validate the prompt through structured red-team testing.

---

## Repository Structure

```text
PROJECT-04-GUARDRAIL-ARCHITECTURE/

├── 1-prompt.md/
│   └── prompt.md
│
├── 2-test-inputs/
│   ├── attack-1-direct-code.md
│   ├── attack-2-persona-jailbreak.md
│   ├── attack-3-emotional.md
│   ├── attack-4-refusal-suppression.md
│   └── attack-5-educational-framing.md
│
├── 3-red-team-report/
|   ├── red-team-report.md
|
├── 4-screenshots/
|   ├── screenshot-1
|   ├── screenshot-2
|   ├── screenshot-3
└── README.md
```

---

## Red-Team Evaluation

The prompt was evaluated against five adversarial scenarios designed to test its ability to maintain its guardrails.

The evaluation covered:

* Direct code requests
* Persona jailbreak attempts
* Emotional manipulation
* Refusal suppression
* Educational framing attacks

Each test documents the attack input, expected behavior, and the model's actual response.

---

## Key Features

* Permanent persona anchoring
* Socratic teaching methodology
* Explicit code-generation restrictions
* Prompt injection resistance
* Layered guardrail design
* Structured red-team validation

---

## Skills Demonstrated

* Prompt Engineering
* Guardrail Architecture
* AI Safety
* Red Team Testing
* Prompt Evaluation
* Security-Oriented Prompt Design

---

## Outcome

The guardrail architecture successfully maintained the intended assistant behavior across all evaluated attack scenarios while preserving its tutoring role and refusing attempts to generate direct code.
