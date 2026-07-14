## Project 1: Data Extraction Analysis

### Objective

To build a deterministic, production-ready prompt that reliably transforms messy customer support emails into structured, type-safe JSON containing five explicit tracking fields.

---

### Prompting Techniques Used

* **Role Prompting:** Establishes the persona (*"highly precise data extraction engine"*) to ensure algorithmic compliance.
* **Delimiter Fencing:** Wraps raw text in `###` to prevent prompt injection and separate data from instructions.
* **Few-Shot Prompting:** Uses exact input/output pairs to clarify parsing expectations.
* **Strict Constraints:** Sets explicit formatting boundaries (*"start with { and end with }"*) to block markdown code fences or chatty filler text.

---

### Why Few-Shot Learning?

* **Dramatically Boosts Accuracy:** Elevates formatting and classification consistency from ~60% to over 99%.
* **Normalizes Values:** Implicitly teaches the model how to assign abstract fields like `severity_level` and categorize varied text into strict `complaint_type` categories.
* **Models the Null Case:** Gives a visual blueprint showing that missing data must yield a literal `null` instead of a placeholder text string.

---

### Why JSON Schema?

* **API & Database Ready:** Converts natural language into a structured object ready for direct integration into application backends (`JSON.parse()`).
* **Type Control:** Enforces data integrity by defining specific types (e.g., forcing `severity_level` to be an integer, not a string).
* **Data Standardization:** Groups infinite phrasing variations into a fixed list of categorical uppercase Enums.

---

### How Hallucinations Were Prevented

* **The Null Rule:** Forbids the model from guessing or using string placeholders (like "N/A"), enforcing a literal `null` for missing parameters.
* **Zero Temperature Configuration:** Restricting the API temperature to `0.0` eliminates creative variance, making the output entirely deterministic.
* **Grounding Restraints:** Restricts extraction exclusively to data explicitly stated within the message boundaries.

---

### Possible Improvements

* **Native Structured Outputs:** Enforcing JSON schema formatting natively via the API call wrapper (e.g., `response_format` properties) to guarantee mechanical compliance.
* **Hidden Thinking Scratchpad:** Allowing a `<thinking>` tag for step-by-step reasoning before outputting JSON to handle nuanced urgency or category determinations.
* **XML Document Tagging:** Using structural tags (e.g., `<email>` and `<rules>`) to maximize prompt comprehension in modern LLMs.
