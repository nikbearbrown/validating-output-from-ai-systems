> **Voice status:** `voice-unanchored`. Style folder empty as of this draft.

---

# Chapter 4 — Validating Factual Claims

*Does the Citation Exist, and Does It Support the Claim — Two Different Questions, and Why Grounding Moves the Failure Instead of Removing It*

**Author:** Nik Bear Brown
**Editor:** Nik Bear Brown

---

## Suggested titles

1. **Validating Factual Claims: Existence Is Mechanical, Support Is Judgment**
2. **RAG Relocates the Failure: Grounding, Citation Checking, and the Retrieval Error You Inherit**
3. **Exists ≠ Supports ≠ Correct: A Layered Method for Factual Output**

---

## TL;DR

Factual claims are the output type where both halves of the book's thesis are sharpest. Where ground truth is mechanical, validation works cleanly: a citation *resolves or it doesn't*, and a citation-existence check is as deterministic as a compiler — it would have stopped the *Mata v. Avianca* fabricated-cases disaster cold (S.D.N.Y. 2023; lawyers sanctioned **$5,000** for filing fake ChatGPT-generated precedents — verified). Where ground truth is not mechanical, validation degrades: whether a real source actually *supports* the claim attached to it is a judgment, and the corpus evidence is grim — across generated citations, no model exceeded a **~0.475 existence rate**, and even among existing citations **50–90% of responses are not fully supported** [verify: future-dated preprint figures]. Hallucination is now localizable to *spans*, not whole answers (SemEval-2025 Mu-SHROOM, arXiv:2504.11975 — 14 languages, 38 LLMs, 2,618 submissions), which is what makes claim-level checking tractable. RAG reduces hallucination but does not eliminate it (Ayala & Béchard, NAACL 2024; MEGA-RAG **>40%** reduction in public health [verify]) — and crucially it *relocates* the failure: a grounded answer is only as correct as its retrieval, so confabulation becomes retrieval error. The discipline: deterministic existence checks first, judgmental support checks next, and in high-stakes domains a human expert who owns the residual. Bridge to Ch. 5: support-checking with another LLM reintroduces the fluency problem, which is the reasoning-validation problem in disguise.

---

## Learning objectives

By the end of this chapter you should be able to:

1. **(Understand)** Explain why span-level hallucination labeling (Mu-SHROOM) makes claim-level validation tractable, and why a whole-answer "is this hallucinated?" verdict is too coarse to act on.
2. **(Analyze)** Separate three distinct properties of a citation — it *exists*, it *supports* the claim, it is *factually correct* — and identify which can be checked mechanically and which require judgment.
3. **(Evaluate)** Apply a two-gate check (citation-existence, then claim-support) to an LLM answer, and find a citation that exists but does not support the claim attached to it.
4. **(Evaluate)** Explain mechanistically why RAG reduces hallucination but relocates the failure into retrieval error, and identify when a confident, well-cited answer is nonetheless wrong because retrieval failed.
5. **(Create)** Design a layered factual-validation pipeline — deterministic existence gate → judgmental support gate → human expert for the residual — appropriate to a stated domain and risk level.

## Prerequisites

Ch. 1 (Why Validating AI Output Is a Different Problem) — fluency defeats reviewers; well-formed is not correct. Ch. 2 (The Deterministic Floor) — deterministic validators run first; here the deterministic check is "does this reference resolve?" Ch. 3 (Validating Generated Code) — the layered-sieve pattern and the principle that you never let the model grade its own work, which returns in §4.5 when an LLM judges claim support. No special domain knowledge assumed; the legal and public-health examples are explained in place.

---

## 4.1 The brief that cited cases that did not exist

In 2023, lawyers representing a plaintiff in a personal-injury suit against the airline Avianca filed a brief in the Southern District of New York. It was a competent piece of legal writing: it argued the law, it quoted holdings, it cited precedent with proper internal citations — case names, reporters, page numbers, the works. The opposing counsel went to look the cases up. They did not exist. Not "were misremembered" — did not exist. The airlines named in the captions were invented; the quotations were fabricated; the internal citations pointed nowhere. The brief had been drafted with ChatGPT, which had produced fluent, properly formatted, entirely fictional law.

What turned an embarrassment into a teaching case was what happened next. Challenged by the court, the lawyers did not immediately retreat. They asked ChatGPT whether the cases were real, and ChatGPT *assured them the cases existed* and could be found on Westlaw and LexisNexis. They submitted that assurance. The cases still did not exist. Judge P. Kevin Castel sanctioned the lawyers and their firm **$5,000** (decision June 22, 2023), describing one of the fabricated analyses in the record as "gibberish." (Case: *Mata v. Avianca, Inc.*, 678 F. Supp. 3d 443 — name, facts, sanction, and judge all verified.)

This single failure contains the whole chapter, because it is two distinct failures wearing one coat. The first is **citation existence**: the cited cases do not exist, and that is a *mechanical* fact — a clerk with database access could have falsified the brief in an afternoon by checking whether each citation resolves. No judgment required; the cases are there or they aren't. The second is the **fluency trap**, the book's central claim, documented in a court record: the fabrications were filed *because they read as real.* Properly formatted legal citations carry an authority that the human reader extends to their content. The lawyers did not check existence because the prose did not look like the kind of thing that needed checking. And when they did "check" — by asking the model — they let a fluent system grade its own output, which is the exact anti-pattern from Ch. 3 reappearing in a new domain.

Notice what *would* have caught it. Not a better lawyer reading more carefully; fluency defeats careful reading, which is the whole problem. What would have caught it is a deterministic gate: resolve every citation against a real database, reject the document if any fails to resolve. That check is cheap, it requires no judgment, and it would have stopped *Mata v. Avianca* before it was filed. This chapter builds out from that gate — and then confronts the harder gate the existence check cannot replace.

---

## 4.2 Three properties of a citation: exists, supports, correct

The *Mata* case makes "the citation doesn't exist" vivid, but existence is only the first of three distinct properties, and conflating them is the source of most factual-validation errors. A citation attached to a claim can be evaluated on three separate axes:

1. **Existence.** Does the referenced document exist? Does the DOI resolve, the URL load, the case appear in the reporter, the paper appear in the index? This is *mechanical*. The answer is binary and judgment-free. It is the compiler of factual validation.

2. **Support.** Granting that the document exists — does it actually *say* what the claim attributes to it? A real paper can be cited for a finding it never reported; a real case can be cited for a holding it never made. Checking support requires reading the source and comparing it to the claim. This is *judgment*, not a lookup.

3. **Correctness.** Granting that the document exists *and* supports the claim — is the claim *true*? The source may itself be wrong, outdated, or retracted. A faithfully-cited claim can still be false. This requires domain knowledge and, sometimes, ground truth that does not exist anywhere retrievable.

These three are independent. A citation can exist and not support (the common case). It can support and be incorrect (faithful to a wrong source). It can be fabricated entirely (*Mata*). The vocabulary the field has converged on captures part of this: *correct* is not the same as *faithful* (does the answer reflect its sources?), and faithful is not the same as *existing*. The 2024-era work "Correctness is not Faithfulness" formalizes the support-vs-correctness split; the existence-vs-support split is what the citation-corpus studies measure.

And those measurements are sobering. Across roughly 17,443 generated citations in one body of work, **no model exceeded a ~0.475 existence rate** — more than half of cited references simply did not exist. And among the citations that *did* exist, **50–90% of responses were not fully supported** by the sources cited, with around 30% of individual statements unsupported. The two checks are *both* necessary because they catch different failures: existence catches the fabrications; support catches the real-citation-wrong-claim class that existence waves through.

A flag, honestly placed: the specific percentages above surfaced partly from future-dated preprints (arXiv:2602.23452 "CiteAudit"; arXiv:2603.07287) that post-date a normal cutoff and read as unverifiable. The *distinction* — existence versus support versus correctness — is well-established and citable from verified-era sources; the exact numbers are flagged `[verify]` and should not be printed as settled. Teach the three-way distinction; cite the magnitudes with caution.

> **Misconception to dislodge.** "The answer has citations, so it's grounded and trustworthy." A citation is a checkable object with three independent properties, and the presence of a citation tells you about *none* of them. The reference may not exist; it may exist and not support the claim; it may support the claim and be wrong. "It has citations" is the *start* of validation, not the end of it — and treating it as the end is exactly what happened in *Mata v. Avianca*.

---

## 4.3 Hallucination lives in spans, which is what makes checking tractable

To validate a factual answer you need to know *what to check*, and the field has made that question answerable by moving from a coarse verdict to a fine one. The old framing asked: "is this answer hallucinated?" — a binary label on the whole output. That is nearly useless for validation, because a four-paragraph answer is rarely all-true or all-false; it is mostly grounded with a few unsupported substrings woven in, and the binary label tells you nothing about *where*.

SemEval-2025 Task 3, **Mu-SHROOM** (arXiv:2504.11975), reframes detection as **span-labeling**: mark the specific substrings of generated text that are *not supported* by a reference document. The shared task ran across **14 languages** (Arabic, Basque, Catalan, Chinese, Czech, English, Farsi, Finnish, French, German, Hindi, Italian, Spanish, Swedish), over outputs from **38 LLMs**, drawing **2,618 submissions from 43 teams**. The conceptual shift matters more than the numbers: hallucination is now treated as *localized*, and localization is what makes claim-level validation possible.

Here is the connection to §4.2. If hallucination lives in spans, then validation is a per-claim operation: **decompose the answer into atomic claims, and check each one** — does its citation exist, does the cited span support it. The span-level view turns "validate this answer" (intractable, holistic, fluency-vulnerable) into "validate these twelve claims" (tractable, itemized, each with a gate). It also suggests the right *review affordance*: a UI that highlights the suspect spans so a human reviewer reads the unsupported substrings rather than the fluent whole. That is a direct counter to the fluency trap — the fluent paragraph reads as uniformly authoritative; the highlighted span says "look here, this is where the support is missing."

One honest limit the shared task itself reveals: the boundary between *fully supported*, *partially supported*, and *unsupported* is fuzzy even for human annotators. Span-label agreement on borderline "partially supported" spans is imperfect. The three-way distinction is fuzzier than the binary existence check, which is exactly why support-checking is the judgmental gate and existence is the mechanical one.

> **Misconception to dislodge.** "Either the answer is a hallucination or it isn't." Hallucination is not a property of whole answers; it lives in spans. A mostly-correct answer with two unsupported sentences is the normal case, and a whole-answer label hides the two sentences that matter. Validate claims, not answers — and highlight the spans so the reviewer's attention goes where the support is missing, not where the prose is smoothest.

---

## 4.4 RAG reduces hallucination — and relocates the failure into retrieval

The standard production fix for factual hallucination is Retrieval-Augmented Generation: instead of asking the model to answer from its parameters, you *retrieve* relevant documents first and condition the generation on them (Lewis et al., 2020). Ground the model in evidence and it confabulates less. This works, and it is worth being precise about both the "works" and its price.

It works. Ayala & Béchard (NAACL 2024, arXiv:2404.08189) deployed RAG in a structured-output setting at ServiceNow — generating workflows from natural-language requirements — and report that RAG *significantly reduces* hallucination and improves out-of-domain generalization. The gains were clearest where the output had a checkable structure (a connection to structured-output validation in Ch. 6). In a high-stakes domain, MEGA-RAG (Xiong et al., 2025, *Frontiers in Public Health*) achieved a **>40% reduction** in hallucination rate versus baselines including a standalone LLM and standard RAG, in public-health question answering [verify], via multi-source retrieval (dense + BM25 + a biomedical knowledge graph), a cross-encoder reranker, and discrepancy-aware refinement. The reduction is real and replicated across domains.

Now the price, and it is the chapter's central mechanism. RAG does not *remove* the failure; it *relocates* it. Before RAG, the failure mode is confabulation: the model invents facts and citations from its parameters (the *Mata* failure). After RAG, the answer is only as correct as the documents the retriever supplied — so the failure mode *moves upstream into retrieval*. If the retriever pulls an outdated passage, an off-topic passage, or misses the relevant document entirely, the model will produce a **confident, well-cited, wrong** answer — grounded in the wrong source. The citation will exist (it's a real retrieved document) and may even support the claim (the model faithfully reflected the bad passage), and the answer will still be false, because the *retrieval* was wrong.

This is the book's thesis in one move: **validation relocates the failure rather than dissolving it.** The arrow of blame moves left, from the generator to the retriever. And it changes what you must validate. A pre-RAG validator checks the *generation*. A RAG validator must *also* check the *retrieval* — is the retrieved passage relevant? recent? actually the best available source? — because that is where the residual error now lives. The exact split between retrieval error and generation error after RAG is task- and corpus-dependent and genuinely contested; there is no standard way to attribute it automatically. But the engineering consequence is clear: when you adopt RAG, you inherit a retrieval-quality validation problem you did not have before.

And the residual is not small. MEGA-RAG's >40% reduction means the *other* ~60% of hallucinations survive. In a public-health or clinical setting, that residual is exactly the fraction that needs an expert before it reaches a patient or a policy. The data justifies "RAG *then* expert," not "RAG *instead of* expert."

> **Misconception to dislodge.** "We added RAG, so hallucination is solved." RAG reduces hallucination (replicated, ~40%+ in the cited cases) but does not eliminate it, and it introduces a new failure: a confident, well-cited answer grounded in a *wrong retrieved document*. The error moved from the generator to the retriever; if you only validate the generation, you are now blind to where the failure actually lives.

---

## 4.5 The layered method, and where the human must own the residual

Assembling the chapter into a pipeline, the layering mirrors Ch. 3's sieve — cheapest deterministic check first, judgment last — adapted to the thinner oracle that factual claims afford.

1. **Decompose into atomic claims.** Span-level (§4.3): break the answer into checkable units, each with the source it leans on.
2. **Citation-existence gate (deterministic).** For each claim's citation, resolve the DOI/URL/case cite. If it does not resolve, reject. This is mechanical, cheap, judgment-free, and it stops *Mata v. Avianca* outright. Run it first, on everything.
3. **Retrieval-quality check, if RAG (deterministic-ish).** Validate that the retrieved passage is relevant and current — not just that *a* document came back (§4.4). The failure moved upstream; check upstream.
4. **Claim-support gate (judgmental).** For each existing citation, retrieve the cited span and ask whether the source actually says what the claim attributes to it (entailment / NLI / careful reading). This catches the real-citation-wrong-claim class — the 50–90%-unsupported finding made operational [verify].
5. **Human expert for the residual, in high-stakes domains.** Correctness (§4.2, axis 3) and novel-domain accuracy have no mechanical oracle. The expert owns what the gates cannot certify.

The honest tension is at gate 4. *Existence is mechanically checkable; support is not.* And the obvious automation — use another LLM to judge whether the source supports the claim — **reintroduces the fluency problem.** You are now asking a fluent system to judge whether a fluent system's claim is supported, and the judge can be fooled by the same plausibility that fooled the original reader. Whether automated claim-support checking is reliable enough to *certify* high-stakes claims is unresolved, and it is precisely the LLM-as-judge problem this book treats later. So in high-stakes domains — legal, medical, financial — gate 4 is where a human expert enters, and gate 5 acknowledges that some correctness has no retrievable ground truth at all.

That unresolved gate is the bridge. Checking claim support with an LLM is a special case of a larger question: *can a model reliably validate reasoning — its own or another's?* The support check is a reasoning judgment ("does this premise entail this conclusion?"), and judging reasoning with a fluent model runs straight into the failure mode we have now seen twice — fluency defeating the judge. The next chapter takes that on directly: validating reasoning chains, where the oracle is thinnest of all, and where the question "did the model check its work?" finally has to be answered honestly.

---

## Exercises

1. **(Analyze)** Take an LLM answer to a factual question that includes three citations. For each citation, evaluate the three properties from §4.2 separately: (a) does it exist? (b) does it support the specific claim it's attached to? (c) is the claim correct? Find — or construct — at least one citation that *exists but does not support* its claim, and explain why the existence gate would have passed it.

2. **(Evaluate)** You are shown a RAG system's answer: confident, fluent, with a citation to a real document in the knowledge base. The answer is nonetheless factually wrong. (a) Name the failure mode from §4.4. (b) Explain why the citation-existence gate and even the claim-support gate could *both* pass while the answer is still wrong. (c) State the additional check that would have caught it, and which pipeline stage (§4.5) it belongs to.

3. **(Apply / produce-something)** Build and run a two-gate check on a real LLM answer. Pick a question, get an answer with at least four citations from a model. Gate 1: resolve every citation (DOI/URL/search) and record exists/doesn't. Gate 2: for each *existing* citation, read the source and label the claim *fully supported / partially supported / unsupported*. Produce: a small table of citation × (exists?, support-label), a one-sentence statement of how many failed each gate, and a note on any citation that *exists but doesn't support* — the class the existence gate alone would miss.

4. **(Create)** Design a factual-validation pipeline for a stated high-stakes domain (pick one: a clinical decision-support tool, a legal-research assistant, or a financial-disclosure summarizer). Specify the five stages from §4.5, state for each what it catches and what it cannot, identify exactly where the human expert enters and what residual risk they own, and justify — citing §4.4 and the MEGA-RAG >40% figure — why "RAG then expert" rather than "RAG instead of expert" is the defensible architecture for your domain.

---

## What would change my mind

A demonstration that automated claim-support checking — LLM-as-judge or NLI-based entailment — reaches reliability high enough to *certify* support in high-stakes domains without a human, on adversarial cases where the claim and the source are both fluent and superficially aligned but substantively mismatched. The chapter's claim is that support is judgmental and that judging it with a fluent model reintroduces the fluency problem; if a method demonstrably resisted that — caught the real-citation-wrong-claim cases at expert-level reliability across domains — then gate 4 could be automated, the human expert could move from "owns the support residual" to "spot-checks," and "high-stakes domains need expert review" would weaken to "high-stakes domains need expert review where the automated support-checker's confidence is low." I would also revisit the "RAG relocates the failure" framing if a retrieval architecture were shown to drive retrieval error below generation error to the point that validating retrieval added little — but the current evidence (a ~60% residual even at >40% reduction) argues the other way.

## Still puzzling

- **Automating support without re-importing fluency.** Existence is solved; support is not. Whether LLM/NLI support-checking can be made reliable enough to certify, rather than merely screen, is open and cross-references the LLM-as-judge problem.
- **Attributing post-RAG error.** What fraction of residual error after RAG is *retrieval* error versus *generation* error, and how to attribute it automatically — no standard decomposition exists.
- **Factual validation with no retrievable ground truth.** In genuinely novel domains there may be no source to ground against. This is the hard residual the chapter must admit is unsolved; it defers to the scalable-oversight problem later in the book.
- **Cross-lingual hallucination detection.** Mu-SHROOM shows detection performance varies sharply by language; low-resource languages are under-served, so a pipeline validated in English may not transfer.
- **The fuzzy "partially supported" boundary.** Reliable three-way (full/partial/none) span labeling is open even for humans, which limits how crisp the support gate can be made.

---

## References

- Garfield, E. (1955). Citation Indexes for Science: A New Dimension in Documentation through Association of Ideas. *Science*, 122(3159), 108–111. (A citation as a checkable, indexable object — the lineage of the existence check.)
- Bush, V. (1945). As We May Think. *The Atlantic.* (Knowledge as a traceable graph of linked claims; framing for "an untraceable claim is unverifiable by construction.")
- Lewis, P., et al. (2020). Retrieval-Augmented Generation for Knowledge-Intensive NLP Tasks. *NeurIPS 2020.* (Canonical RAG; the technique this chapter both recommends and criticizes.) `[verify venue/year specifics before printing]`
- Vázquez, R., et al. (2025). SemEval-2025 Task 3: Mu-SHROOM, the Multilingual Shared Task on Hallucinations and Related Observable Overgeneration Mistakes. [arXiv:2504.11975](https://arxiv.org/abs/2504.11975). (Span-labeling; 14 languages, 38 LLMs, 2,618 submissions / 43 teams.)
- Ayala, O. M., & Béchard, P. (2024). Reducing Hallucination in Structured Outputs via Retrieval-Augmented Generation. *NAACL 2024 (Industry Track)*, 228–238. [arXiv:2404.08189](https://arxiv.org/abs/2404.08189). (RAG significantly reduces hallucination in structured output; ServiceNow deployment.)
- Xiong, X., et al. (2025). MEGA-RAG: a retrieval-augmented generation framework with multi-evidence guided answer refinement for mitigating hallucinations of LLMs in public health. *Frontiers in Public Health*, 13:1635381 (PMC12540348; PubMed 41132171). (>40% hallucination reduction vs. baselines.) `[verify the >40% figure phrasing before printing]`
- *Mata v. Avianca, Inc.*, 678 F. Supp. 3d 443 (S.D.N.Y. 2023). (Fabricated ChatGPT-generated precedents; Judge P. Kevin Castel sanctioned the lawyers $5,000, June 22, 2023; "gibberish." Case name, facts, sanction verified.)
- "Correctness is not Faithfulness" (2024). [arXiv:2412.18004](https://arxiv.org/abs/2412.18004). (Formalizes the support-vs-correctness distinction; cited as the verified-era source for the concept.) `[verify title/authors before quoting]`
- Citation-corpus existence/support figures (existence ≤ ~0.475; 50–90% of responses not fully supported) — surfaced partly from future-dated preprints arXiv:2602.23452 ("CiteAudit") and arXiv:2603.07287. `[verify — do not print exact percentages as settled; the existence-vs-support distinction is established, the magnitudes are not.]`

---

**Tags:** factual-claim-validation, citation-existence, claim-support, exists-vs-supports-vs-correct, span-level-hallucination, mu-shroom, rag-relocates-failure, retrieval-error, mata-v-avianca, expert-review-high-stakes
