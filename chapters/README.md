# Validating Output from AI Systems: chapter guide

[Return to the book overview](../README.md).

### Part I · Decide what to check

1. [Why Validating AI Output Is a Different Problem](../chapters/01-why-validating-ai-output-is-different.md): Define what an AI output has to satisfy beyond looking plausible.
2. [The Deterministic Floor: Validators That Don't Need Judgment](../chapters/02-the-deterministic-floor.md): Start with schema, type, range and other mechanically checkable rules.

### Part II · Validate the artifact or action

3. [Validating Generated Code](../chapters/03-validating-generated-code.md): Evaluate generated programs against requirements and tests.
4. [Validating Factual Claims](../chapters/04-validating-factual-claims.md): Trace factual assertions to independent, relevant evidence.
5. [Validating Reasoning Chains](../chapters/05-validating-reasoning-chains.md): Find where a reasoning chain makes an unsupported move.
6. [Validating Structured Output](../chapters/06-validating-structured-output.md): Check structure and meaning separately in machine-readable records.
7. [Validating Agentic Task Execution](../chapters/07-validating-agentic-task-execution.md): Compare the claimed agent action with observable execution evidence.

### Part III · Design oversight

8. [LLM-as-Judge: Uses and Failure Modes](../chapters/08-llm-as-judge-uses-and-failure-modes.md): Use a model judge within known limits and test where it fails.
9. [Process Reward Models: Step-Level Verification](../chapters/09-process-reward-models.md): Consider step-level feedback and its limits as evidence of correctness.
10. [Human Validation and Automation Bias](../chapters/10-human-validation-and-automation-bias.md): Design a human review that resists rubber-stamping a fluent answer.
11. [Building a Validation Pipeline](../chapters/11-building-a-validation-pipeline.md): Combine multiple checks into a reproducible validation process.
12. [Scalable Oversight: The Unsolved Problem](../chapters/12-scalable-oversight.md): Identify where oversight does not scale with fluent output.
13. [Capstone: Designing a Validation Strategy for a Real System](../chapters/13-capstone-designing-a-validation-strategy.md): Defend a validation strategy for a realistic system and name remaining risks.
