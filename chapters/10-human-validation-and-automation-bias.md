> **Voice status:** `voice-unanchored`. Both root `style/` and `books/validating-output-from-ai-systems/style/` empty as of this draft.

---

# Chapter 10 — Human Validation and Automation Bias

*The Layer of Last Resort Is the One Fluency Disables First — So Engineer It, Don't Trust It*

**Author:** Nik Bear Brown
**Editor:** Nik Bear Brown

---

## Suggested titles

1. **Human Validation and Automation Bias: Why "A Human Will Catch It" Is Not a Control**
2. **The Reviewer of Fluent Output Is the Compromised Position: Designing Review Against a Known Failure Mode**
3. **The Intervention That Works Is the One Reviewers Dislike: Cognitive Forcing Functions and the Approval-Fatigue Problem**

---

## TL;DR

The human is the validation layer of last resort — invoked exactly where deterministic checks, tests, schemas, LLM-judges, and PRMs have run out — and it is the layer the fluency of AI output most efficiently disables. **Automation bias** is the well-documented tendency to over-rely on automated output, reducing vigilance; it is old (aviation, clinical decision support, GPS), replicable, and amplified by fluent, authoritative surface form. Lyell & Coiera's 2017 systematic review (JAMIA) establishes that the bias rises with **cognitive load / verification complexity** — which means *bounding scope is a designable lever*, not just an exhortation. Buçinca et al. (2021, "To Trust or to Think") found the chapter's sharpest tension: **cognitive forcing functions reduce overreliance more than the best explainable-AI interventions do — and reviewers subjectively dislike them**, rating them more effortful and less likable. So the working intervention cannot be left to reviewer discretion; it has to be built into the workflow. Beck et al. (2025, "Bias in the Loop," arXiv:2509.08514, N=2,784, preprint) add two findings: individual *disposition* toward AI predicts error-catching better than demographics or financial incentives (skeptics catch more; you cannot pay overreliance away), and **requiring corrections for flagged errors backfired** — the friction reduced engagement and increased acceptance of wrong suggestions. The chapter's contribution: the human layer is not a reliable backstop by default; it must be engineered against a measured cognitive failure mode, using four levers — cognitive forcing functions, adversarial framing, bounded scope, and fatigue-aware approval design — and even then a residual risk remains that someone must own. **Note on a common citation:** a widely-circulated "27% more issues missed, d=0.62 (Wang et al. 2024)" figure has *no traceable primary source* and is treated here as unverified; the *direction* is carried on Lyell & Coiera and Beck et al. instead.

---

## Learning objectives

By the end of this chapter you should be able to:

1. **(Understand)** Explain automation bias mechanistically — omission versus commission errors, the cognitive-load mediator, and why fluent output amplifies it — and why this makes "a human will catch it" a non-control by default.
2. **(Analyze)** Diagnose a review workflow for the conditions under which automation bias is worst: AI output shown first, single approve button, large scope, high approval volume, benign history.
3. **(Evaluate)** Assess a proposed review or approval design against the evidence — specifically, predict whether a friction-adding "guardrail" will increase scrutiny or backfire into rubber-stamping (Beck et al. 2025).
4. **(Create)** Design a review protocol that counters automation bias using the four levers, mapping each to its cited mechanism and the cognitive-load variable it targets.
5. **(Evaluate)** State, for a given pipeline, what residual risk the engineered human layer still cannot remove, and who must own it.

## Prerequisites

Ch. 1 (Why Validating AI Output Is a Different Problem) — the fluency problem (well-formed ≠ correct) stated as the book's thesis. Ch. 2 (The Deterministic Floor) — because the first move in fatigue-aware design is to *not* spend scarce human attention on what a machine catches. Ch. 9 (Process Reward Models) — the validation layer above the human; the human is invoked precisely where the PRM's step-truth ran out. Ch. 11 (Building a Validation Pipeline) is downstream: this chapter produces the human-layer slice of that pipeline.

---

## 10.1 The 51st pull request

Consider a reviewer — call the situation a composite, assembled from documented patterns and labeled as such; the mechanism is real, the person is not. They are the human-in-the-loop on an AI coding agent. The agent opens pull requests; the reviewer approves or rejects. Over a morning they review fifty PRs. Forty-nine are clean — the agent is good, the diffs are tidy, the code reads well, the tests pass. The reviewer approves them, and each approval is correct.

The fiftieth PR also reads well. The diff is tidy. The code is fluent — well-named variables, a sensible structure, a comment explaining the change. The reviewer approves it. It contains a logic error that flips a condition in an authorization check, and it ships.

Nothing about the reviewer was negligent in the ordinary sense. The problem is structural, and Lisanne Bainbridge named it in 1983 in "Ironies of Automation." When you automate the routine work and leave the human only the exceptions, you have stripped the human of exactly the practice and engagement that catching an exception *requires*. The reviewer who signs off on forty-nine good outputs is, by the fiftieth, in the worst possible state to catch the fiftieth: under-engaged, anchored on a history of correctness, reading fluent code that *looks* like the forty-nine clean ones because it is fluent in the same way. The skill of close reading atrophies precisely as the volume of "nothing to catch here" rises. The human is asked to intervene in the rare, high-stakes case for which monitoring — rather than authoring — has left them least prepared.

This is the inversion most engineers get backwards. The intuition is that *reviewing* is easier and safer than *authoring* — the work is done, you just check it. The human-factors literature says the opposite for fluent output: the reviewer of a fluent wrong answer is the *more* compromised position, not the less. The author had to construct the thing and confront every gap. The reviewer is handed a finished, confident, well-structured artifact and asked to find what is wrong with it — and the better it reads, the less scrutiny it draws. That is **automation bias**, and the rest of this chapter is about why it is real, why you cannot fix it by telling people to be careful, and what you can build instead.

The book's thesis — validation works where ground truth is mechanically available and fails where it isn't, and **fluency defeats human reviewers** — is most directly about this chapter. The human is the layer of last resort, invoked where the deterministic floor, the tests, the judges, and the PRMs have all run out. And it is the layer fluency disables most efficiently, because the human has nothing mechanical left to check against — only how good the answer looks, which is exactly the signal a fluent wrong answer is optimized to fake.

---

## 10.2 The mechanism: what automation bias is, and what mediates it

Automation bias is not a character flaw. It is a documented, replicable tendency to over-rely on automated decision support, reducing vigilance in seeking and processing information. It predates LLMs by decades — autopilots, clinical decision-support systems, GPS navigation. It comes in two error types, and keeping them distinct is the start of any mitigation:

- **Omission error** — you *miss* something the automation did not flag. The authorization bug in §10.1 is an omission error: the agent did not flag it, and the reviewer, trusting the agent's silence, did not look. Omission errors are the quiet ones; nothing alerts you that you missed anything.
- **Commission error** — you *act* on a wrong automation recommendation, against evidence you had or could have gotten. Approving a migration the system recommended even though a check you could have run would have contradicted it.

Raja Parasuraman and colleagues built much of the modern empirical study of this, including the omission/commission distinction and the "levels and stages of automation" framework. The distinction matters for design because the two error types have different fixes: omission errors are countered by forcing independent search ("what did the automation *not* tell you?"); commission errors are countered by forcing a check against an independent source before acting.

Now the load-bearing finding for this chapter. Lyell & Coiera's 2017 systematic review in JAMIA ("Automation bias and verification complexity," DOI 10.1093/jamia/ocw105) established that automation bias is associated with **cognitive load and task complexity** — and crucially, it is *not* merely a multitasking phenomenon, as the older literature had assumed. The heavier the verification task, the more the reviewer defaults to trusting the machine.

This is the sentence that turns a lament into an engineering discipline:

> **If cognitive load mediates the bias, then reducing cognitive load reduces the bias — and cognitive load is something you can design.**

You cannot design away human fallibility. You *can* design the load. Bounding scope, decomposing claims, sending machine-catchable errors to the machine — these are not motivational posters; they are interventions on the variable Lyell & Coiera identified as the mediator. That reframing is the whole reason this chapter is "Create-level" rather than a warning.

> **Misconception to dislodge.** "Automation bias is a discipline problem — train reviewers to be more careful, hold them accountable, and it goes away." False, and the evidence is direct. Disposition predicts error-catching, but exhortation and incentives do not fix the bias (§10.3), and the load that drives it is a property of the *task design*, not the reviewer's willpower. You do not solve automation bias by finding better humans; you solve it by engineering the conditions under which an ordinary human can exercise judgment.

---

## 10.3 The two findings that break the obvious fixes

If telling people to be careful doesn't work, two reflexes follow: *explain the AI's reasoning so the reviewer can judge it*, and *add friction so the reviewer has to engage*. The evidence says both reflexes are, at best, half right — and one of them backfires.

### Cognitive forcing beats explainability — and reviewers hate it

Buçinca, Malaya & Gajos (2021, "To Trust or to Think: Cognitive Forcing Functions Can Reduce Overreliance on AI in AI-Assisted Decision-Making," PACMHCI, DOI 10.1145/3449287) ran the comparison directly. They tested **cognitive forcing functions** — interventions that interrupt the reflexive acceptance of the AI answer — against the best **explainable-AI (XAI)** interfaces. The three forcing functions: (1) make the human commit a decision *before* seeing the AI suggestion; (2) introduce an on-demand delay before the AI answer appears; (3) let the human choose whether to see the AI at all.

The headline result, which this chapter foregrounds: **cognitive forcing functions reduced overreliance more than the explainable-AI interventions did.** Explanations, the intuitive fix, were *worse* at reducing overreliance than simply forcing the human to engage before the AI could anchor them.

And the sting in the tail: **participants subjectively preferred the explanation interfaces and rated the cognitive-forcing interfaces as more effortful and less likable.** The intervention that *works* is the one reviewers *dislike.*

This is the chapter's sharpest tension, and it has a direct design consequence. If the effective intervention is the disliked one, you cannot leave it to reviewer discretion — because given the choice, reviewers will route around friction and choose the comfortable interface that doesn't work. The forcing function has to be built into the *workflow*, removed from the individual's moment-to-moment choice, the same way you don't let individual developers opt out of CI.

> **Misconception to dislodge.** "Good review UX means a smooth, pleasant, low-friction interface." For *catching errors*, often the reverse. The pleasant interface — AI answer shown first, one approve button, full explanation on tap — is the one that maximizes overreliance. Good *review* UX deliberately inserts the friction that recruits engagement, and it does so by design rather than by asking the reviewer to opt in, because they won't.

There is a refinement worth carrying. A 2025 follow-on (Buçinca et al., PACMHCI, DOI 10.1145/3710946, `[verify]` exact issue) found that *partial* explanations — withholding part of the reasoning so the human has to fill the gap — reduce overreliance more than *full* explanations, which tend to be accepted wholesale. Explanations are not useless; complete ones invite uncritical acceptance, and the value is in the engagement the gap forces, not the information the explanation supplies.

### Required corrections backfire; disposition dominates

Beck, Eckman, Kern & Kreuter (2025, "Bias in the Loop: How Humans Evaluate AI-Generated Suggestions," arXiv:2509.08514, preprint) ran a randomized experiment with **N = 2,784**, manipulating AI suggestion quality, task burden (whether corrections were *required*), and performance-based financial incentives. Two findings go straight into design.

First: **individual attitude toward AI was the strongest predictor of performance — stronger than demographics or incentives.** AI-skeptics caught more errors. You cannot pay or instruct overreliance away; disposition dominates. This makes skepticism a potential *staffing* lever (§10.4) and it explains why "we'll incentivize careful review" tends not to move the needle.

Second, and counterintuitive: **requiring corrections for flagged AI errors *reduced* engagement and *increased* acceptance of incorrect suggestions.** A friction that was meant to help — *you must justify any correction* — instead induced more rubber-stamping. Not every friction recruits engagement; the wrong friction trains the reviewer to take the path of least resistance, which is to accept.

> **Misconception to dislodge.** "Adding friction or mandatory steps makes reviewers more careful." Not reliably. Buçinca's *decide-before-seeing* friction reduced overreliance; Beck's *required-corrections* friction increased it. The lesson is not "add friction" but "add the friction that forces engagement *with the content* — not friction that adds cost to the act of disagreeing." Much guardrail design assumes friction = scrutiny. That assumption is wrong, and you must test which kind of friction you've built.

Hold these together with §10.2 and you get the chapter's stance: the bias is real and load-mediated; explanations under-perform forcing functions; incentives don't help; disposition does; and friction cuts both ways. None of this is fixed by better intentions. All of it is addressable by design.

---

## 10.4 The four levers: designing the human layer

Frame the mitigations not as virtues but as four designable levers, each mapped to a cited mechanism. This is the chapter's produced artifact and seeds the human-layer slice of Chapter 11's pipeline.

**Lever 1 — Cognitive forcing functions.** Build the interruption into the workflow. The validated forms (Buçinca et al. 2021): (a) **decide-before-seeing** — the reviewer commits an independent judgment or prediction *before* the AI output is revealed, which defeats anchoring directly; (b) **mandatory delay** before the AI answer appears; (c) **hide-by-default** — the human chooses to reveal the AI, rather than being handed it. Use partial rather than full explanations (the 2025 follow-on) so the gap forces engagement. *Mechanism:* recruit active processing before the fluent answer can anchor. *Design rule:* remove it from reviewer discretion — they will not voluntarily choose the friction that works.

**Lever 2 — Adversarial framing.** Replace the confirmatory prompt with an adversarial one. "Is this correct?" invites the omission error — it primes the reviewer to look for confirmation and stop. "What are the three strongest reasons this is wrong?" or "find the bug; there is one" recruits the engagement the forcing-function literature shows is the active ingredient. *Mechanism:* a confirmatory frame searches for agreement; an adversarial frame searches for failure, which is what you actually need. *Honesty note:* present adversarial framing as a well-motivated *design hypothesis* grounded in the same engagement mechanism Buçinca validated — not as a separately RCT-confirmed result. `[verify]` as direct evidence appears.

**Lever 3 — Bounded scope.** This is the lever that targets the mediator Lyell & Coiera identified directly. Small diffs, not large ones. Decomposed claims, not whole documents. One decision at a time, not a batch. *Mechanism:* automation bias rises with cognitive load and verification complexity; bounding scope lowers the load, which lowers the bias. A 400-line diff is not reviewed; it is skimmed and approved. A 20-line diff can be read. Combine with Chapter 2's deterministic floor: **send IDE-catchable and deterministically-catchable errors to the linter, the compiler, the test suite — not to the human.** Do not spend scarce human attention on what a machine catches reliably; reserve it for what only a human can.

**Lever 4 — Fatigue-aware approval design.** The §10.1 problem at scale: an agent emitting dozens of approval prompts an hour converts human-in-the-loop into performative rubber-stamping. The decades-old analogue is **alert fatigue** in security operations, where true-positive detection collapses as alert volume rises regardless of analyst skill (peer-reviewed treatment: ACM Computing Surveys, "Alert Fatigue in Security Operations Centres," DOI 10.1145/3723158; vendor figures on false-positive rates are practitioner signal only, `[verify]`). The design principle, importable directly: **human-in-the-loop works when decisions are rare and consequential; it fails when they are frequent and varied.** So make approvals *risk-proportional* — auto-approve or batch the benign and low-risk (gated by deterministic checks), and surface to the human *only* the rare, consequential, irreversible actions. Blanket approval on everything is worse than no approval gate, because it manufactures the fatigue that guarantees the rubber stamp.

A fifth, organizational lever follows from Beck et al.: **disposition-aware staffing.** Since AI-skepticism predicts error-catching better than incentives do, staffing skeptics on high-stakes review is a real lever — with the open caveat (§Still puzzling) that we don't know whether a skeptic stays skeptical after 200 good outputs.

Assemble them into two workflows. The **naive HITL / overreliance path:** AI output shown first, full and fluent → single "Approve?" button → reviewer accepts. Every condition for automation bias is present — anchoring on the fluent answer, no forced engagement, unbounded scope, high volume, benign history. The **forcing-function workflow:** (1) reviewer commits an independent judgment *before* the AI output is revealed; (2) output revealed after a deliberate delay, with a partial rather than full explanation; (3) adversarial prompt — "list the strongest reasons this is wrong"; (4) scope bounded to a small unit, machine-catchable errors already filtered out by the deterministic floor; (5) the approval gate is risk-proportional — benign actions batched or auto-approved, only rare-and-consequential actions surface to the human.

James Reason's Swiss-cheese model ties this to the rest of the book. Each validation layer — deterministic checks, tests, LLM-judge, PRM, human review — is a slice with holes. An accident happens when the holes align: a fluent-but-wrong output the deterministic floor doesn't cover (a logic error, not a type error), the tests don't exercise (incomplete spec), the judge rates highly (it's fluent), the PRM can't help (no checkable step-truth here), and the human approves (anchored, fatigued, fluent output). The four levers shrink the hole in the *last* slice — and the honest point of the model is that the hole never closes. Which is why the human layer comes with a residual risk someone must own (§10.5).

---

## 10.5 What the engineered human layer still cannot do

Even a workflow with all four levers does not make the human a reliable backstop. It makes the human a *better-conditioned* backstop than the naive one. The residual risk is real and must be named, not waved away with "human-in-the-loop."

Three honest limits. First, **forcing functions may not survive production.** Buçinca et al. is a controlled study; whether decide-first and mandatory-delay workflows hold up under real deadline pressure — or get quietly routed around the first time a release is late — is open. A forcing function that the org tolerates only until it's inconvenient is not a control. Second, **skill atrophy is on a timer (Bainbridge).** The reviewer's own competence decays as they monitor rather than author; the rate of this decay in AI-assisted engineering is unmeasured, and a perfectly designed workflow staffed by reviewers whose close-reading has atrophied is still exposed. Third, **the magnitude of the review penalty is under-measured.** The *direction* is well-supported — reviewing fluent AI output degrades error detection relative to independent work, on the cognitive-load mechanism (Lyell & Coiera) and the disposition findings (Beck et al.). A clean, peer-reviewed effect size for code and prose is not established. (A widely-circulated "27% more issues missed, Cohen's d = 0.62, Wang et al. 2024" figure could *not* be traced to any primary source — no venue, DOI, or arXiv ID — and the specifics that surface in search appear only inside LLM-generated summaries; treat it as unverified, likely confabulated, and do not cite it as fact. The direction stands on the verified sources; the number does not.)

So the chapter's contribution to the book's thesis, stated plainly: **the human layer is not a reliable backstop by default; it must be engineered against a known, measured cognitive failure mode — and even engineered, it carries a residual risk that a human must explicitly own.** This is the layer where the book stops trusting "a human will catch it" and starts specifying the conditions under which a human *plausibly can*. It is not the layer where the problem is solved.

---

## 10.6 Bridge to Chapter 11

You now have the human-layer design — and the four levers, the Swiss-cheese frame, and the residual-risk honesty are not a standalone protocol. They are one slice of a layered pipeline. The point of bounding scope and filtering machine-catchable errors to the linter is that the human is *last*, catching only what survived the cheaper, more reliable layers below it.

Chapter 11 assembles the whole stack — deterministic floor → automated tests → LLM-judge / PRM → engineered human review — into an architecture wired into CI with observability, and maps each common failure mode (fluency-as-proxy, tests-that-don't-test, RAG retrieval error, self-eval loops, security regression, approval fatigue, judge circularity) to the layer that catches it. The human layer designed here drops into that pipeline as the last slice of Swiss cheese — the one with the hole the others were built to keep things from reaching, and the one whose residual hole is the reason the next chapters exist.

---

## Exercises

1. **(Analyze)** Take this workflow verbatim: *"The agent proposes a database migration, shows the full SQL and an explanation of what it does, and the on-call engineer clicks Approve. We approve about 40 migrations a shift."* (a) Identify every §10.2/§10.4 condition for automation bias present. (b) Classify the most likely error type (omission vs commission) and justify. (c) Name which of the four levers is most violated and why.

2. **(Evaluate)** A team, worried about rubber-stamping, proposes: *"To approve an AI suggestion, the reviewer must type a one-sentence justification for every approval."* Using Beck et al. (2025), predict whether this increases scrutiny or backfires, and explain the mechanism. Then propose a *different* friction, grounded in Buçinca et al., that is more likely to recruit engagement, and say why the difference matters.

3. **(Create, produce-something)** Design a review protocol for a specific setting of your choice (e.g., AI-generated code PRs, AI-drafted clinical summaries, AI-proposed financial transactions). Produce: a step-by-step workflow instantiating all four levers; for *each* lever, the cited mechanism and the cognitive-load variable it targets; the risk-proportional approval policy (what auto-approves, what batches, what surfaces to a human); and a one-paragraph statement of the residual risk the protocol cannot remove and who owns it.

4. **(Evaluate)** A vendor claims their explainable-AI dashboard "reduces reviewer error by showing the model's full reasoning for every decision." Using Buçinca et al. (2021) and the partial-explanation follow-on, state what is right and what is likely wrong about this claim, and what single change to the interface you would predict reduces overreliance more.

5. **(Analyze)** Map a realistic ship-the-wrong-output trajectory onto James Reason's Swiss-cheese model for a pipeline of: deterministic checks → automated tests → LLM-judge → PRM → human review. Name a specific "hole" in each slice and describe the single output that slips through all five, ending at an approved, fluent, wrong result. Then say which one lever from §10.4 would most shrink the final hole.

---

## What would change my mind

A robust, replicated demonstration that, in realistic production conditions (real deadlines, real volume, not a controlled lab task), a naive human-in-the-loop review — AI output shown first, approve/reject, no engineered forcing functions — catches fluent-but-wrong AI output at a rate indistinguishable from an engineered forcing-function workflow. The chapter's claim is that automation bias is real, load-mediated, and unfixable by exhortation or incentives, so the human layer must be engineered. If it turned out that ordinary motivated reviewers, under production conditions, caught errors just as well without forcing functions — that the lab effects of Buçinca and the disposition effects of Beck didn't survive contact with real work, or were swamped by reviewer expertise — then the four-lever design would drop from *necessary* to *nice-to-have*, and "engineer the human layer" would soften to "engineer it where you can measure that naive review is failing." The direction would still hold; the prescriptive force would not.

## Still puzzling

- **The magnitude of the review penalty.** The direction is well-supported; a clean peer-reviewed effect size for reviewing AI code/prose vs independent work is not (and the much-quoted Wang et al. number is untraceable). A genuine, citable gap.
- **Do forcing functions survive production?** Lab results are clear; whether decide-first / mandatory-delay workflows hold under deadline pressure or get routed around is open, and a forcing function that's tolerated only until inconvenient is not a control.
- **Does the skeptic stay skeptical?** Beck et al. show disposition predicts error-catching, suggesting a staffing lever. But Bainbridge predicts the skeptic's vigilance — like the author's close-reading skill — may decay after 200 good outputs. The decay timeline is unmeasured.
- **Why did required corrections backfire?** Is mandatory friction reliably disengaging, or is it task-specific? This matters because so much guardrail design assumes friction equals scrutiny, and Beck et al. is one strong but single (preprint) result against that assumption.

---

## References

- Bainbridge, L. (1983). [Ironies of Automation](https://doi.org/10.1016/0005-1098(83)90046-8). *Automatica* 19(6): 775–779. DOI:10.1016/0005-1098(83)90046-8. (Automating the routine work strands the human with the un-practiced exceptions.)
- Lyell, D., & Coiera, E. (2017). [Automation bias and verification complexity: a systematic review](https://doi.org/10.1093/jamia/ocw105). *Journal of the American Medical Informatics Association* 24(2): 423–431. DOI:10.1093/jamia/ocw105. Open access. (Automation bias is mediated by cognitive load / verification complexity — the designable lever.)
- Buçinca, Z., Malaya, M. B., & Gajos, K. Z. (2021). [To Trust or to Think: Cognitive Forcing Functions Can Reduce Overreliance on AI in AI-Assisted Decision-Making](https://doi.org/10.1145/3449287). *Proceedings of the ACM on Human-Computer Interaction* 5(CSCW1), Art. 188. DOI:10.1145/3449287. arXiv:2102.09692. (Cognitive forcing > explainability for reducing overreliance; reviewers dislike the forcing interfaces.)
- Buçinca, Z., et al. (2025). [Cognitive Forcing for Better Decision-Making: Reducing Overreliance on AI Systems Through Partial Explanations](https://doi.org/10.1145/3710946). *Proceedings of the ACM on Human-Computer Interaction.* DOI:10.1145/3710946. `[verify exact issue]` (Partial explanations reduce overreliance more than full ones.)
- Beck, J., Eckman, S., Kern, C., & Kreuter, F. (2025). [Bias in the Loop: How Humans Evaluate AI-Generated Suggestions](https://arxiv.org/abs/2509.08514). arXiv:2509.08514. **Preprint.** (N=2,784; disposition predicts error-catching better than incentives; required corrections backfired into more acceptance of wrong suggestions.)
- Reason, J. (1990). *Human Error.* Cambridge University Press. (The Swiss-cheese model of organizational accidents — layered defenses, aligning holes, active failures vs latent conditions.)
- Parasuraman, R., & Manzey, D. H. (2010). [Complacency and Bias in Human Use of Automation: An Attentional Integration](https://doi.org/10.1177/0018720810376055). *Human Factors* 52(3): 381–410. DOI:10.1177/0018720810376055. `[verify]` (Omission/commission distinction; empirical study of automation bias.)
- "Alert Fatigue in Security Operations Centres" (2025). *ACM Computing Surveys.* DOI:10.1145/3723158. `[verify]` (True-positive detection collapses as alert volume rises — the analogue for approval fatigue.)
- CSET, Georgetown (2024). [AI Safety and Automation Bias: The Downside of Human Oversight](https://cset.georgetown.edu/publication/ai-safety-and-automation-bias/). Issue brief. `[verify exact URL]` (Human-in-the-loop invoked as a control without engineering the conditions for judgment.)
- **Unverified — do not cite as fact:** the widely-circulated "27% more issues missed reviewing AI vs independent (Wang et al. 2024, Cohen's d = 0.62)" figure has no traceable primary source (no venue, DOI, or arXiv ID) and is likely confabulated. The *direction* is carried on Lyell & Coiera (2017) and Beck et al. (2025).

---

**Tags:** automation-bias, human-in-the-loop, cognitive-forcing-functions, ironies-of-automation, swiss-cheese-model, omission-commission-errors, cognitive-load, bounded-scope, adversarial-framing, approval-fatigue, alert-fatigue, disposition-aware-staffing, fluency-defeats-reviewers, residual-risk, bias-in-the-loop
