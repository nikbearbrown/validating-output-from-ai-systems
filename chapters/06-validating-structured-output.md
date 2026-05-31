# Chapter 6 — Validating Structured Output

*A schema proves the shape is right; it cannot prove the values are true*

---

A finance-automation team built a pipeline that read scanned invoices and emitted a clean JSON record: vendor, invoice number, line items, subtotal, tax, total. They were disciplined about structure. They defined a Pydantic model, validated every model output against it, and rejected anything that did not parse. Their dashboard showed 99.7% "validation pass rate," and they reported the pipeline as validated. Records that passed flowed straight into the accounts-payable system.

The pass rate was real and it was measuring the wrong thing. One record that sailed through had `subtotal: 1840.00`, `tax: 147.20`, and `total: 1987.20` — and the invoice's actual total, printed on the page, was `$1,897.20`. The model had transposed two digits. The record was *structurally perfect*: every field present, every type correct, the JSON well-formed. Pydantic had nothing to object to, because Pydantic checks shape and type, and the shape and type were flawless. The error was *semantic* — a wrong value in a valid slot — and the structural gate is blind to it by construction. Another record passed with a `vendor_id` that referenced a vendor not in their system; another had `tax` larger than `subtotal`. All schema-valid. All wrong.

The team made no structural error. They made a **validation** error: they treated Gate 1 (does it parse?) as if it were Gate 2 (are the values right?). Those are different gates with different guarantees, and conflating them is exactly how a 99.7% pass rate ships a wrong invoice.

Structured output is the cleanest place in this book to see the thesis in miniature, because it splits exactly along the ground-truth line. Shape is a solved, deterministic problem. Values are where ground truth thins out — and where you have to say honestly what you can and cannot check.

---

## Gate 1: structure is a solved, deterministic problem

Structural validity is the easy half, and it is genuinely easy because a schema is a contract and a parser is a judgment-free oracle. You define the shape once and the parser enforces it forever.

![A single decode step reads left to right. The full candidate-token row passes through a grammar finite-state-machine mask. Grammar-violating tokens are blocked and zeroed; only grammar-valid tokens survive, and a sampling arrow draws only from those into the output slot, making invalid tokens impossible to sample.](../images/06-validating-structured-output-fig-02.png)
![Constrained decoding masks every grammar-violating token at each step, so an invalid token is impossible to sample.](images/06-validating-structured-output-fig-02.png)
*Figure 6.2 — Constrained decoding masks every grammar-violating token at each step, so an invalid token is impossible to sample.*

```python
from pydantic import BaseModel, field_validator
from datetime import date

class LineItem(BaseModel):
    description: str
    quantity: int
    unit_price: float

class Invoice(BaseModel):
    vendor_id: str
    invoice_number: str
    issue_date: date
    line_items: list[LineItem]
    subtotal: float
    tax: float
    total: float

# Deterministic gate: parses or raises. Zero judgment.
invoice = Invoice.model_validate_json(model_output)
```

Pydantic (Python), Zod (TypeScript), and JSON Schema (language-agnostic) all do the same essential thing: parse the output against a typed contract and raise on mismatch. Always do this. It is the structured-output instance of the deterministic floor — cheap, unambiguous, non-negotiable.

But there is a stronger move than checking *after* generation. Checking after the fact tells you a structural failure happened after it happened; **constrained decoding** prevents it from happening at all. Willard & Louf (2023), "Efficient Guided Generation for Large Language Models" (arXiv:2307.09702, the basis of the open-source Outlines library), reformulate constrained generation as transitions over a finite-state machine: given a schema, you precompute an index so that at each decoding step only tokens consistent with the schema have non-zero probability. An invalid token is literally impossible to sample. The output is *guaranteed* schema-valid because no other output can be produced.

This is why **API-level structured output beats prompt-only**, and the distinction has a name: *enforcement versus instruction*. "Please respond in JSON" is an instruction — a request the model usually honors and sometimes does not. Token-masking at decode time is enforcement — a constraint the model cannot violate. OpenAI's published figures make the gap concrete: plain JSON mode carries a 5–10% structural failure rate while Structured Outputs with `strict: true` is under 0.1% [verify — vendor-reported figures for specific model versions, not independent measurements]. Treat the numbers as model-specific and vendor-sourced; treat the principle — enforcement is categorically more reliable than instruction — as the durable lesson.

One misconception to kill directly: "I asked for JSON and parse it, so structure is handled." Asking is instruction; the failure tail is real. If you have constrained decoding or an API `strict` mode, use it — the structure becomes a guarantee, not a hope.

---

## Gate 2: structure passed — are the values right?

Here is the half the invoice team missed. A schema constrains *shape and type*. It says `total` is a float. It cannot say `total` equals `subtotal + tax`, that `tax` is non-negative, that `vendor_id` refers to a vendor that exists, or that the number on the record matches the number on the page. Those are *semantic* properties, and they need a second gate.

Semantic validity is a spectrum of mechanizability, not a single thing.

![Five stacked check-type tiers read top to bottom by decreasing mechanizability. Range, enum membership, referential integrity and cross-field consistency are mechanizable, above a divider. Below it, factual-value correctness is non-mechanizable and hands off to factual validation or human review.](../images/06-validating-structured-output-fig-03.png)
![Semantic checks form a mechanizability spectrum: range, enum, referential, and cross-field are automatable; factual-value correctness is not.](images/06-validating-structured-output-fig-03.png)
*Figure 6.3 — Semantic checks form a mechanizability spectrum: range, enum, referential, and cross-field are automatable; factual-value correctness is not.*

**Range checks** — mechanizable. `unit_price > 0`, `0 <= tax_rate <= 1`, `issue_date <= today`. A Pydantic `field_validator` or a few lines after parsing.

**Enum membership** — mechanizable. `status in {"paid", "pending", "void"}`. The schema can encode this directly.

**Referential integrity** — mechanizable *if you have the reference data*. `vendor_id` exists in the vendor table; the SKU is in the catalog. A lookup, not a judgment.

**Cross-field consistency** — mechanizable. `total == subtotal + tax` within a cent; line-item sum equals subtotal; ship date after order date. Arithmetic and logic you control.

**Factual correctness of a value** — generally *not* mechanizable here. "Is `$1,897.20` the right total?" requires comparing to the source document. "Is this the correct medication dose?" requires a domain expert. The schema cannot reach truth.

```python
from pydantic import model_validator

class Invoice(BaseModel):
    # ... fields as before ...

    @model_validator(mode="after")
    def semantic_checks(self):
        if self.tax < 0 or self.subtotal < 0:
            raise ValueError("negative monetary field")
        if abs(self.total - (self.subtotal + self.tax)) > 0.01:
            raise ValueError(f"total {self.total} != subtotal+tax {self.subtotal + self.tax}")
        line_sum = sum(li.quantity * li.unit_price for li in self.line_items)
        if abs(line_sum - self.subtotal) > 0.01:
            raise ValueError(f"line items {line_sum} != subtotal {self.subtotal}")
        return self
```

This single validator would have caught the transposed-digit invoice — `total != subtotal + tax` — and a referential-integrity lookup would have caught the phantom `vendor_id`. Note what it still would *not* catch: an invoice where `subtotal`, `tax`, and `total` are mutually consistent but *all three* are wrong because the model misread the page. That is the cell where ground truth lives in the source document or in a human's eyes, not in any rule you can write.

A second misconception to kill: "It validated, so the data is good." It validated *structurally*. A schema-valid record can carry a fabricated citation, a negative price, a non-existent foreign key, or a plausible-but-wrong number. Build the second gate explicitly — and accept that its top rung often has no mechanical validator and must hand off to Chapter 4 or to a human.

---

## The two-gate pipeline, and the retry trap

Assemble the picture. Raw model output enters a left-to-right pipeline with two distinct gates.

![Raw model output passes Gate 1, a solid-edged deterministic structural check, then Gate 2, a dashed-edged semantic check whose interior shades from solid mechanizable checks to a dashed non-mechanizable region. Each gate has a reject exit; accepted records reach a terminal.](../images/06-validating-structured-output-fig-01.png)
![The two-gate pipeline: a solved deterministic structural gate, then a semantic gate where ground truth thins out.](images/06-validating-structured-output-fig-01.png)
*Figure 6.1 — The two-gate pipeline: a solved deterministic structural gate, then a semantic gate where ground truth thins out.*

**Gate 1 — Structural (deterministic, hard-edged).** Schema validation via Pydantic / Zod / JSON Schema or constrained decoding. Pass means shape and types valid. Enforcement at the API level: under 0.1% failure with `strict: true` versus 5–10% prompt-only [verify]. This is the solved cell.

**Gate 2 — Semantic (mixed, dashed-edged).** Value checks: ranges, enums, referential integrity, cross-field consistency, and — at the top, often non-mechanizable — claim and value truth. Pass means values plausible and, where checkable, correct. This is where ground truth thins out.

And then the trap. When Gate 1 fails, the obvious move is to feed the error back and ask the model to try again — a **retry loop**. Retry loops are useful and you should use them. But be precise about what they are: a retry loop is **resampling, not enforcement.** It draws another sample and hopes it parses. It raises your *apparent* success rate without *guaranteeing* anything, and it is categorically weaker than constrained decoding, which makes the failure impossible in the first place.

![Two stacked panels. Top: a model output hits a validator; a fail arrow loops back to resample up to N tries, then exits to escalation when exhausted. Bottom: a model passes through an inline grammar mask straight to a guaranteed-valid output, with no loop. The difference is presence versus absence of the resampling cycle.](../images/06-validating-structured-output-fig-04.png)
![Retry is resampling — a loop that may exhaust and escalate — while constrained decoding guarantees a valid first sample with no loop.](images/06-validating-structured-output-fig-04.png)
*Figure 6.4 — Retry is resampling — a loop that may exhaust and escalate — while constrained decoding guarantees a valid first sample with no loop.*

```python
def extract_with_retries(model, prompt, schema, max_retries=3):
    for attempt in range(max_retries):
        out = model.generate(prompt)
        try:
            return schema.model_validate_json(out)
        except ValidationError as e:
            prompt = f"{prompt}\n\nYour previous output failed validation: {e}\nReturn valid output."
    raise RuntimeError("exhausted retries — escalate, do not silently accept")
```

The danger is reporting "99.9% of requests eventually returned valid output" as if it were a reliability guarantee. It is a statement about resampling persistence. If you have constrained decoding, you do not need the loop for *structure* at all — the first sample is guaranteed valid. The retry loop's honest job is handling *semantic* failures, where you genuinely want the model to reconsider given a real error signal. And when retries are exhausted, *escalate* — never silently accept the last malformed sample.

A third misconception: "Retries make it reliable." Retries make it *eventually parseable*, which is resampling. Counting "succeeded within N tries" toward a reliability claim conflates the two. Enforcement belongs in your reliability number; retry-loop persistence does not.

---

## Structure is not free: the reasoning tax

One more honest caveat, because it complicates the "always enforce structure" advice. Constraining the output format is not cost-free for the *content* inside the format. Tam et al. (2024), "Let Me Speak Freely? A Study on the Impact of Format Restrictions on the Performance of Large Language Models" (arXiv:2408.02442), observed a significant decline in LLM *reasoning* ability under strict format constraints — forcing a model to emit JSON *while it is solving* a reasoning problem can degrade the quality of the answer, not merely its packaging.

The mechanism is plausible: naive constrained decoding can push generation toward tokens that are locally schema-valid but globally low-probability — the model wants to write out a reasoning step, the grammar demands the next JSON key, and the constraint wins, truncating the very thinking that would have produced the right value.

The mitigation is the same shape as Chapter 5's reason-then-execute: **reason first, format second.** Let the model think in free text — where its reasoning is unconstrained — and only then emit the structured object. A two-step call (reason in prose, then a second constrained call that formats the conclusion) often recovers the lost quality while still giving you a guaranteed-valid structure at the end. You get the enforcement *and* the reasoning, at the cost of an extra call.

A fourth misconception: "Enforcing structure can only help." Enforcing structure helps the *shape* and can hurt the *content* if it constrains the model mid-reasoning. The fix is sequencing — reason unconstrained, then format — not abandoning enforcement.

---

## Exercises

**Exercise 6.1 — Understand / Analyze.** A pipeline reports "99.7% schema-validation pass rate" and labels the output "validated." (a) State exactly what that number does and does not certify. (b) Give three distinct semantic errors that would pass this gate, one each from: cross-field consistency, referential integrity, and factual-value correctness. (c) For the factual-value one, explain why no rule you write in the schema can catch it and where the validation must move.

**Exercise 6.2 — Apply, produce something.** Define a Pydantic or Zod model for a data type with at least six fields and at least one nested list. Add: (i) constrained or `strict`-mode structural enforcement if your API supports it, else post-hoc parsing; and (ii) a semantic-validation layer with at least one range check, one enum, one cross-field consistency rule, and one referential-integrity lookup. Feed it five model outputs — including at least one that is schema-valid but semantically wrong. Produce a table: per record, Gate-1 result, Gate-2 result, and which specific check fired.

**Exercise 6.3 — Evaluate.** You are told two pipelines both hit "99.9% success." Pipeline A uses constrained decoding (`strict: true`); Pipeline B uses prompt-only JSON mode with up to 5 retries. (a) Explain why these two 99.9% numbers are not comparable. (b) State which belongs in a reliability SLA and which does not, and why. (c) Describe a failure scenario that B's number hides and A's does not.

**Exercise 6.4 — Evaluate / Apply.** Design an experiment to test the Tam et al. format-tax claim on one reasoning task you care about. Specify: the task, the metric for content correctness (not just schema validity), the two conditions (format-during-reasoning vs. reason-then-format), and what result would make you adopt the two-step approach in production. Run it if you can and report the content-correctness difference, not just the parse rate.

---

## What would change my mind

A demonstration that constrained decoding imposes *no* measurable content-quality cost on realistic reasoning tasks for current frontier models — that the Tam et al. reasoning drop has been engineered away by better grammar-aligned decoding that respects the model's true distribution, such that "reason first, then format" buys nothing over "format throughout." If that held, the reasoning-tax caveat would collapse and the advice would simplify to "always enforce structure, end to end," with no sequencing dance. I would also revise the Gate-2 pessimism if automated semantic validation extended reliably past ranges, enums, referential, and cross-field into *factual-value* correctness for general domains — but that would mean Chapter 4's factual-validation problem had been solved, which is a much larger claim than this chapter makes. `[verify: state of grammar-aligned decoding quality-tax as of reader's date]`

---

## Still puzzling

**Quantifying the format-restriction tax.** Tam et al. opened the question; the magnitude across models and task types is unsettled, and so is how much careful schema design or two-step prompting recovers.

**Automating semantic validation past the easy checks.** Range, enum, referential, and cross-field are mechanizable; general cross-document consistency and domain plausibility remain largely bespoke, hand-written per pipeline.

**Constrained-decoding distribution distortion.** Making grammar-guided generation respect the model's *true* probability distribution — rather than locally-valid-but-globally-unlikely tokens — is an active research problem.

**Schemas as a false sense of safety.** How often do reviewers wave through schema-valid output without any semantic check, exactly as the opening team did? This is the automation-bias problem, and it is more behavioral than technical.

---

## References

- Willard, B. T., & Louf, R. (2023). *Efficient Guided Generation for Large Language Models.* arXiv:2307.09702. — the Outlines library; finite-state-machine constrained decoding.
- OpenAI (2024). *Introducing Structured Outputs in the API.* openai.com. — vendor-reported 100% schema eval, <0.1% vs 5–10% JSON-mode failure for `gpt-4o-2024-08-06`. `[verify — vendor figures, model-version-specific]`
- Tam, Z. R., et al. (2024). *Let Me Speak Freely? A Study on the Impact of Format Restrictions on the Performance of Large Language Models.* arXiv:2408.02442.
- Pydantic documentation. https://docs.pydantic.dev/
- Zod. https://zod.dev/
- JSON Schema. https://json-schema.org/
- Liskov, B., & Wing, J. (1994). A behavioral notion of subtyping. *ACM TOPLAS* 16(6). — type contracts; the substitution principle behind schema-as-contract.
- Backus, J. W. (1959). The syntax and semantics of the proposed international algebraic language. *ICIP.* — Backus–Naur Form, the grammar notation constrained decoders operate over.
- Constrained-decoding distortion: CRANE (arXiv:2502.09061) and grammar-aligned decoding (arXiv:2405.21047). `[verify exact quality-loss magnitudes per paper before quoting]`

---

**Tags:** structured-output, schema-validation, pydantic, zod, json-schema, constrained-decoding, outlines, structured-outputs, enforcement-vs-instruction, semantic-validation, two-gate, retry-resampling, format-tax, reason-then-format
