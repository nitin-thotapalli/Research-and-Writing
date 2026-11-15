# Improving Trust, Reliability and Accuracy in AI Agentic Systems

*Product thinking · Agent architecture · Evaluation design*

## The case in 60 seconds

**The problem:** An AI agent can return a convincing financial answer, cite a dashboard and still be wrong. Worse, it may repeat a failed tool action, use stale records, or present an uncertain result as verified. These are not all “hallucinations.” They are different system failures requiring different controls.

**The question I wanted to solve:** How would I design an analytical agent that people can depend on—not just one that performs well in a demo?

**My recommendation:** Start with a narrow, consequential workflow. Establish a source-of-truth and metric contract; separate language interpretation from deterministic calculation; insert validation and permission gates before consequential outputs or actions; instrument the execution trace; and test both successful completion and safe failure. Add agent autonomy only where it creates measurable value.

**Worked example:** A synthetic finance agent reports October net revenue retention (NRR) as **123%**. The correct answer is **103%**. The ₹20 lakh difference in the numerator comes from mistakenly including new-customer MRR in an existing-customer retention metric. The cited billing data can be real and the arithmetic internally consistent—and the conclusion can still be wrong.

**The product outcome I would optimize:** More *correct, evidence-backed tasks completed with acceptable review effort*, while keeping critical errors and unauthorized actions close to zero. Not simply more answers generated.

**Evidence boundary:** This is an independent design case using invented data and proposed tests. I did not run a production experiment or measure real accuracy improvement.

---

## 1. Why this is a business problem, not just a model problem

Imagine a finance team using an agent to answer board-reporting questions. The agent pulls information from billing, a warehouse, contract records and an internal metric glossary. It returns an answer in seconds instead of the analyst manually reconciling spreadsheets.

The value proposition is obvious: faster analysis, more questions answered and less repetitive work. But the failure cost is asymmetric. One incorrect number in a board report can trigger additional reviews, weaken confidence in the entire tool and potentially lead to a bad decision.

This creates three independent product requirements:

| Dimension | Question to answer | Example failure |
| --- | --- | --- |
| **Accuracy** | Is this answer correct under the approved definition? | New customer revenue included in NRR |
| **Reliability** | Will the workflow behave correctly and predictably across failures and variations? | Tool timeout leads to duplicate execution; partial records silently accepted |
| **Trust** | Can an authorized user understand the evidence, uncertainty and action boundary? | Citation points to a dashboard but does not show the cohort, formula or approvals |

The business does not purchase “LLM intelligence” for its own sake. It purchases completed work it can safely use.

### The user and the job to be done

**Primary user:** FP&A analyst preparing monthly management reporting.

**Job:** “Tell me October NRR, whether expansion offset churn, and show enough evidence for me to approve the number.”

**Adjacent stakeholders:** Finance lead (accountable for reporting), data owner (definitions and freshness), security/compliance (access controls) and engineering (operability).

**Non-goal for the first iteration:** A universal autonomous finance analyst that can query anything and publish anything. That enlarges the failure surface before the basic workflow is trustworthy.

## 2. Reproduce the failure before prescribing a solution

The agent receives:

> What was October NRR? Did expansion revenue offset the losses from existing customers?

Synthetic inputs, in ₹ lakh MRR:

| Input | Amount | Treatment in NRR |
| --- | ---: | --- |
| Opening MRR from existing customers | 100 | Denominator and starting cohort |
| Churn from existing customers | −8 | Subtract |
| Contraction from existing customers | −4 | Subtract |
| Expansion from existing customers | +15 | Add |
| New-customer MRR | +20 | **Exclude** |

**Correct NRR = (100 − 8 − 4 + 15) / 100 = 103%.**

**Gross revenue retention = (100 − 8 − 4) / 100 = 88%.**

**Ending total MRR = 100 − 8 − 4 + 15 + 20 = ₹123 lakh.**

The incorrect answer, **123%**, results from dividing ending *total* MRR by the opening existing-customer MRR. It is not a simple arithmetic slip: it is a **semantic definition failure**. The source data could be flawless and the agent's citation authentic.

Expansion of ₹15 lakh exceeded churn plus contraction of ₹12 lakh by ₹3 lakh. So the correct business conclusion is that the starting customer cohort expanded by **3% net**, while new customers separately added ₹20 lakh.

### Root cause: where the failure enters the pipeline

| Pipeline stage | Failure hypothesis | How I would verify it |
| --- | --- | --- |
| Intent interpretation | “Revenue retention” mapped to overall revenue growth | Inspect selected metric ID and definition version |
| Retrieval | New and existing customers mixed in the returned population | Compare customer IDs and opening-cohort membership |
| Calculation | LLM improvised numerator or denominator | Recalculate with a deterministic function |
| Explanation | Answer failed to distinguish retention from acquisition | Compare narrative with component-level results |
| Evidence | Citation referenced a dashboard rather than specific source rows | Replay data query and inspect source IDs |
| Release | No material-metric approval gate | Inspect approval state and publication events |

For this constructed case, the incorrect cohort inclusion is the demonstrated cause. The other rows are **failure hypotheses to test**, not claims about a real deployed agent.

### The deeper problem

A better prompt saying “be accurate” will not enforce a stable metric definition, prevent stale retrieval, guarantee tool idempotency or authorize publication. The agent needs a controlled environment—its **harness**—that makes the correct behavior easier and incorrect behavior detectable.

## 3. The system I would design

I would avoid building a swarm of agents. For this workflow, one orchestrating agent plus well-defined tools and independent checks is easier to test, cheaper to operate and more interpretable.

**Proposed flow:**

1. **Interpret and authorize:** Resolve the requested metric, reporting period, user permissions and intended output (analysis versus publication). Ask for clarification when a required choice remains ambiguous.
2. **Retrieve controlled context:** Load the approved NRR definition, opening cohort, billing movements, timestamps and source-record IDs. Treat retrieved documents as data, not executable instructions.
3. **Compute outside the LLM:** Call a typed calculation tool with explicit inputs and formula version; do not let a free-form explanation become the calculation engine.
4. **Verify:** Check period alignment, cohort membership, unique IDs, currency, missing values and reconciliation against ending MRR. Compare result and narrative for contradictions.
5. **Decide whether to release:** Return a verified answer when checks pass; otherwise expose the exception and withhold a definitive result. A human remains responsible for final board-report publication.
6. **Record and learn:** Save trace metadata, rule outcomes, source versions, approval state, corrections and failure codes for evaluation.

### Contracts that make each step testable

| Contract | Minimum fields / controls | Why it matters |
| --- | --- | --- |
| Metric contract | metric_id, definition_version, formula, eligible cohort, exclusions | Prevents semantic drift |
| Retrieval contract | customer_id, period, currency, as_of timestamp, source_record_id | Prevents unverifiable mixing |
| Tool contract | Typed input/output schema, timeouts, explicit error codes | Prevents silent partial success |
| Validation contract | Required reconciliations, tolerances, severity, pass/fail status | Creates independent checks |
| Action contract | Requester identity, allowed scope, approval state, idempotency key | Prevents unauthorized or duplicate effects |
| Evidence contract | Formula, component amounts, source references, exceptions, reviewer | Lets users reproduce and challenge results |

A citation is not proof. The useful evidence unit is **the exact claim linked to the source records, definition and computation that produced it**.

## 4. Designing for failure, not just the happy path

The most revealing tests are often not “Can it answer the clean case?” but “What does it do when the environment is messy?”

| Scenario | Unsafe behavior | Expected reliable behavior |
| --- | --- | --- |
| Billing API times out | Invent numbers or retry indefinitely | Bounded retry with backoff; fail visibly if unresolved |
| One customer appears twice | Count both rows | Detect duplicate business key and stop reconciliation |
| Warehouse is two days stale | Present result as current | Show as-of time; block if freshness requirement fails |
| Two sources disagree | Pick the first convenient value | Follow approved precedence, or escalate unresolved conflict |
| Metric definition changed | Silently reuse old formula | Pin and disclose approved metric version |
| Document includes malicious instructions | Treat retrieved text as commands | Isolate untrusted content from system/tool permissions |
| Agent asked to publish to board pack | Publish after generating plausible prose | Require authorized reviewer approval |
| External write succeeds but response times out | Repeat and create a duplicate | Check operation status using idempotency key before retrying |

**Recovery policy:** Retry *transient* errors within a fixed budget. Never automatically retry permission denials, failed reconciliations or irreversible write actions without establishing previous execution status. Preserve partial work only when it can be safely resumed.

**Escalation policy:** There must be a usable answer state between “success” and “failure”: *insufficient evidence*, *conflicting records*, *needs reviewer decision*, or *not authorized*. Appropriate abstention is a product capability, not an embarrassment.

### Model confidence is not enough

The model saying it is “95% confident” is not a verified probability. I would base release decisions on observable conditions: passing checks, evidence completeness, freshness, evaluator performance for that scenario, and severity of the intended action. Any calibrated confidence score would need separate empirical validation.

## 5. Controls by risk: not every action deserves the same friction

I would use a risk-tiered approval design:

| Task | Risk | Default autonomy |
| --- | --- | --- |
| Summarize a non-sensitive metric glossary | Low | Automatic, with source links |
| Draft an NRR explanation using validated outputs | Moderate | Automatic draft, clearly marked for review |
| Finalize a material management-report number | High | Deterministic verification plus accountable human approval |
| Send financial instructions or change authoritative records | Critical | Strict authorization, independent controls and explicit approval; no free-form autonomous execution |

The tradeoff is important: maximum approvals reduce autonomy, but zero approvals increase risk. Human-in-the-loop should concentrate where the *expected cost of a wrong action* justifies the review burden.

Security controls are also not interchangeable with accuracy controls. A correct calculation performed on unauthorized customer data is still an unacceptable outcome.

## 6. Evaluation: prove improvement instead of describing it

I would use a fixed, labelled test set and trace-based evaluations before changing models or adding agents.

### Test fixture

Start with **24 synthetic cases** across clean NRR, cohort changes, reactivation, duplicates, mixed currencies, late postings, conflicting definitions, stale tools, missing fields and authorization failures. Expand with real anonymized or permissioned failure cases when available.

For each case, freeze:

- Requested task and allowed action
- Input fixtures and expected calculation
- Correct evidence and definition version
- Expected behavior: answer, clarify, abstain or escalate
- Failure severity and acceptable latency

### Evaluation layers

| Layer | Evaluator | Key question |
| --- | --- | --- |
| Extraction | Field-level exact match and source validation | Were values associated with the correct accounts and periods? |
| Definition | Approved metric contract check | Was the right population/formula used? |
| Arithmetic | Deterministic reference calculation | Does the number reconcile? |
| Execution | Tool-call / trace assertions | Did tools run in the right order without unsafe retries? |
| Grounding | Claim-to-evidence review | Can each material claim be reconstructed? |
| Safety | Permission and adversarial tests | Did the agent block prohibited actions and injected instructions? |
| Product | Human reviewer assessment | Was the result useful with less checking effort? |

LLM-as-judge can help assess explanations and semantic quality, but I would **not** delegate exact financial arithmetic or permission enforcement to an LLM judge. Review the judges against human-labelled examples too.

### Primary and guardrail metrics

**Primary success:** Verified task completion rate = correctly completed, evidence-supported tasks / eligible attempted tasks.

**Quality metrics:** Critical numerical error rate; metric-definition accuracy; source coverage; inappropriate-answer rate; inappropriate-abstention rate.

**Reliability metrics:** End-to-end success rate; recoverable-failure recovery rate; duplicate side-effect count; timeout rate; p95 task latency.

**Trust and adoption metrics:** Median reviewer verification minutes; correction rate; percentage of validated outputs accepted; repeat usage by analysts; number of escalations resolved.

**Economic metrics:** Cost per verified task, including model/tool spending and human review time—not cost per model response.

A system that refuses every request might produce no incorrect numbers but also no value. That is why accuracy must be evaluated **alongside completion and review cost**.

### What would convince me to ship?

Compare the existing workflow against the new metric-contract-plus-verification workflow on the *same held-out cases*. Require no critical failures on the defined acceptance suite, an improvement in verified completion, and acceptable review time and latency. Then run a limited, monitored pilot with named owners and rollback criteria.

A perfect score on 24 examples would demonstrate only that those examples passed. It would not prove production reliability.

## 7. Which improvement should the PM prioritize?

There are multiple plausible solutions. I would explicitly compare them before implementing.

| Option | Impact on demonstrated error | Engineering effort | Main limitation |
| --- | --- | --- | --- |
| A. Stronger system prompt | Low / uncertain | Low | Cannot guarantee cohort membership or arithmetic |
| B. More retrieval and richer citations | Moderate | Moderate | More data can also introduce conflicting definitions |
| C. Metric contract + deterministic compute + reconciliation | **High** | Moderate | Requires agreement on business definitions and source access |
| D. Multi-agent reviewer / debate | Uncertain | High | Adds cost, latency and correlated model errors |
| E. Human checks every field | High for reviewed cases | High recurring cost | Limits throughput and does not fix the underlying system |

**My first bet is Option C**, with a narrow human approval boundary. Not because every agent should be deterministic, but because the demonstrated failure is a known business rule violation that can be checked mechanically.

I would postpone multi-agent review until trace data shows a class of errors that independent specialist reasoning actually improves.

## 8. Value creation and rollout

The business case must be quantified from a baseline, not assumed.

**Value mechanism:** Reduce analyst rework and critical errors while preserving useful throughput.

A simple measurement model:

> Monthly review time saved = eligible correctly completed tasks × measured reduction in reviewer minutes per task.

For illustration only, if a team completes 400 validated tasks per month and saves 6 review minutes per task, that is **40 hours of review effort avoided monthly**. This is released capacity, *not automatically payroll savings*. The calculation excludes implementation and ongoing operating costs, which need to be measured separately.

### Rollout sequence

**Phase 1 — Baseline:** Instrument current agent tasks, reconstruct errors and define severity. Establish who owns the metric definition.

**Phase 2 — Controlled pilot:** Implement NRR contract, deterministic calculation, evidence view, exception states and reviewer approval. Test the fixed suite.

**Phase 3 — Live evaluation:** Run in shadow mode or draft-only mode; compare agent outputs with analyst-approved answers and measure task economics.

**Phase 4 — Expand selectively:** Add adjacent metrics only after proving that existing contracts and control patterns generalize. Introduce higher autonomy gradually and with explicit rollback conditions.

The deepest risk is not a single incorrect formula. It is scaling an agent before anyone can answer: *which source did it use, which definition did it apply, which checks passed, what action did it take, and who was accountable?*

## Final recommendation

For high-consequence analytical agents, I would prioritize **system-enforced correctness over prompt-based confidence**.

Build the smallest workflow where you can reproduce the error, introduce independent controls at the actual failure point, measure **verified task completion**, and make exceptions visible rather than hiding them behind fluent explanations.

A trustworthy agent is not one that never encounters uncertainty. It is one that **knows when it lacks sufficient evidence, does not cross its authority boundary, can recover safely, and gives users a reproducible basis for deciding whether to rely on its work**.

---

*Method note: All numbers, scenarios and proposed controls here are illustrative. This is a product/architecture proposal, not a claim of deployed results.*
