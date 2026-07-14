# Project 3: Closed-Book System (RAG Basics)

This repository contains **Project 3** of my Prompt Engineering internship at Decodelabs, focusing on **Factuality Control and Context Locking**.

This project implements a strict, closed-book Examination System designed to eliminate AI hallucinations. By turning the Large Language Model into a bounded synthesis engine, the model is architecturally prevented from using its own pre-trained memory. It answers queries using **only** an injected reference document, enforces exact paragraph-level citations, and cleanly deflects using a standardized string when information is missing.

---

## 💻 Core Concept: Open-Book vs. Closed-Book

| Feature | Open-Book (Standard AI) | Closed-Book (This Project) |
| --- | --- | --- |
| **Knowledge Source** | Global pre-training weights | **ONLY** the injected context block |
| **Hallucination Risk** | High — confidently invents facts | **Zero** — deflects securely if missing |
| **Audit Trail** | None | **Mandatory** exact paragraph citations |
| **Enterprise Use Case** | Creative writing, open brainstorming | Legal compliance, HR policy, Finance audits |

---

## 🛠️ Repository Architecture & File Structure

The project files are organized into a strict testing and deployment pipeline:

```text
├── 1_context-document/
│   └── hr-policy.md              # The injected reference knowledge base (split into [Para X] tags)
├── 2_test-inputs/
│   ├── valid-query.md            # Test query for information that exists explicitly
│   ├── trick-query.md            # Test query asking for out-of-bounds information 
│   └── distractor-query.md       # Test query embedded with irrelevant distraction contexts
├── 3_test-outputs/
│   ├── valid-query-output.md     # Verified output showcasing precise citation compliance
│   ├── trick-query-output.md     # Verified output showing exact deflection string
│   └── distractor-query-output.md# Verified output proving protection against training memory bleed
├── screenshots/
│   ├── Screenshot-1.png          # UI/Execution log validation snippet 1
│   └── Screenshot-2.png          # UI/Execution log validation snippet 2
├── prompt.md                     # The complete production-ready system template
└── README.md                     # Documentation

```

---

## ⚡ The System Blueprint (`prompt.md`)

This prompt architecture acts as an industrial-grade guardrail using XML tag insulation and negative constraint enforcement.

```text
[SYSTEM]
You are a Senior Technical Analyst operating in closed-book mode. Answer questions using ONLY the reference text in the <context> tags. You are a bounded synthesis engine, not an open-ended oracle.

STRICT OPERATING RULES:
1. CONTEXT ONLY: Answer using ONLY the facts present inside the <context> tags. Do not use your training memory under any circumstances.
2. DEFLECTION PROTOCOL: If the answer to the question is not fully supported by the provided context, output exactly this phrase and nothing else:
   "Information Not Found"
   No apologies. No explanations. No conversational preambles or guesses.
3. MANDATORY CITATION: Every sentence in your answer must end with the paragraph tag it came from. Format: [Para X].
4. NO FABRICATION: If you are even slightly uncertain whether the context supports a claim, trigger Rule 2. Fabrication is a critical failure.

<context>
[Para 1] The company was founded in 2010 and is headquartered in Lagos, Nigeria. It employs over 500 staff across 3 offices.
[Para 2] All full-time employees are entitled to 20 days of paid time off (PTO) annually, beginning after their first year of employment.
[Para 3] Remote work is permitted for all roles except those in the Operations department, subject to manager approval.
[Para 4] The performance review cycle runs twice a year, in June and December, and is tied to annual bonus eligibility.
</context>

<example>
<query>How many PTO days do employees receive?</query>
<response>Full-time employees are entitled to 20 days of PTO annually, beginning after their first year. [Para 2]</response>
</example>

<example>
<query>What is the maternity leave policy?</query>
<response>Information Not Found</response>
</example>

<query>
{YOUR QUESTION HERE}
</query>

```

---

## 🔬 The Crucible: Evaluation Matrix

The system was evaluated against three distinct threat vector scenarios to verify total compliance:

### Scenario A: Valid Query Compliance

* **Input (`2_test-inputs/valid-query.md`):** `"How many times a year are performance reviews held?"`
* **Expected Output (`3_test-outputs/valid-query-output.md`):** Mentions June and December, appended explicitly with `[Para 4]`.

### Scenario B: Out-of-Bounds Trick Query

* **Input (`2_test-inputs/trick-query.md`):** `"What is the CEO's name?"`
* **Expected Output (`3_test-outputs/trick-query-output.md`):** `Information Not Found` (Zero conversational chatter).

### Scenario C: Distractor Injection Defense

* **Context Extension:** `[Para 5] The iPhone 15 features an A16 chip and USB-C connectivity.`
* **Input (`2_test-inputs/distractor-query.md`):** `"When was Apple founded?"`
* **Expected Output (`3_test-outputs/distractor-query-output.md`):** `Information Not Found` (The model successfully suppresses its training knowledge that Apple was founded in 1976).

---

## 🎯 Verification Checklist (How We Know It Passed)

* [x] **No Bleed:** Model never uses pre-trained memory to fill knowledge gaps.
* [x] **Literal Deflection:** Output matches the target string `"Information Not Found"` exactly when out-of-bounds.
* [x] **Granular Auditing:** Every factual claim successfully traces back to a verified `[Para X]` tag.
* [x] **XML Structural Isolation:** Instructions, data boundaries, and test queries remain cleanly compartmentalized.