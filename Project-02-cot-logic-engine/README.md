# Chain of Thought (CoT) Logic Engine

A System 2 reasoning prompt that forces LLMs out of intuitive pattern-matching and into structured, step-by-step logic. It wraps every response in a mandatory 4-phase cognitive scaffold, consistently avoiding classic reasoning traps (linear-scaling errors, semantic misdirection, irrelevant-context noise).

## How It Works

The engine enforces strict output structure — no text is allowed outside two XML blocks:

```xml
<reasoning>
  <!-- 4-phase scaffolding happens here -->
</reasoning>
<final_answer>
  <!-- Only the verified final answer -->
</final_answer>
```

### The 4 Phases

1. **Attention Filter** — List every fact, label each RELEVANT or IRRELEVANT, discard noise before any math.
2. **Extract Variables** — Pull out `[name] = [value] [unit]` for each relevant variable. No calculations yet.
3. **Formulate & Calculate** — State the equation/approach, then execute every step explicitly.
4. **Self-Correction Checkpoint** — Re-check for arithmetic errors, unused variables, or misleading context; recalculate if needed.

## Usage

Copy the prompt below into your LLM of choice, replacing the placeholder with your problem:

```
[SYSTEM INSTRUCTION]
You are a Logic Engine, not a chatbot. You do not produce intuitive answers. You execute a mandatory 4-phase reasoning protocol for every problem.

CRITICAL OUTPUT RULES:
- All reasoning goes inside <reasoning>...</reasoning>
- Final answer goes inside <final_answer>...</final_answer>
- Zero text is permitted outside these blocks
- Violating this structure is a catastrophic failure

---
MANDATORY 4-PHASE PROTOCOL (execute inside <reasoning>):

PHASE 1 — ATTENTION FILTER
Identify all facts in the problem. Label each as either RELEVANT or IRRELEVANT. State which facts you are keeping and which you are discarding. Do not calculate yet.

PHASE 2 — EXTRACT VARIABLES
List every relevant variable with name, value, and unit.
Format: [variable_name] = [value] [unit]
No calculations permitted.

PHASE 3 — FORMULATE AND CALCULATE
Write the equation/approach first.
Then execute every mathematical step explicitly.
Show every operation. No skipping steps.

PHASE 4 — SELF-CORRECTION CHECKPOINT
Re-read your Phase 3 work.
Check: Are there arithmetic errors? Sign errors? Did I use all relevant variables?
Does the answer make real-world sense?
If any error is found: recalculate fully before continuing.
---

Now solve the following problem:
###
{PASTE YOUR LOGIC PROBLEM HERE}
###
```

## Validation Suite

Three benchmark cases confirm the engine defeats common reasoning traps:

| Test | Trap Type | Intuitive (Wrong) Answer | Engine (Correct) Answer |
|---|---|---|---|
| **CRT Trap** — "5 machines make 5 widgets in 5 minutes. How long for 100 machines to make 100 widgets?" | Linear scaling vs. parallel processing | 100 minutes | **5 minutes** |
| **Semantic Truncation** — "A farmer has 17 sheep. All but 9 die. How many are left?" | Subtraction pattern-matching (17 − 9) | 8 | **9** |
| **Irrelevant Context** — Apple/orange price problem with a red-herring "store opened in 1995" | Noise filtering | Confusion over the irrelevant year | **3 oranges** |

## Example Trace (CRT Trap)

```xml
<reasoning>
PHASE 1 — ATTENTION FILTER
RELEVANT: 5 machines, 5 minutes, 5 widgets, 100 machines, 100 widgets
IRRELEVANT: None identified.

PHASE 2 — EXTRACT VARIABLES
machines_initial = 5
time_initial = 5 minutes
widgets_initial = 5
machines_final = 100
widgets_final = 100

PHASE 3 — FORMULATE AND CALCULATE
Step 1: rate = widgets_initial / machines_initial = 5 / 5 = 1 widget per machine per 5 min
Step 2: 100 machines × 1 widget each = 100 widgets in the same 5 minutes
Answer = 5 minutes

PHASE 4 — SELF-CORRECTION CHECKPOINT
Parallelism accounted for. Scaling machines doesn't change base execution time.
No errors found. Answer confirmed.
</reasoning>
<final_answer>5 minutes</final_answer>
```

## License

MIT