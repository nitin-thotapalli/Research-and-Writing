# Improve trust, reliability and accuracy of AI agentic systems

**Case Study** · AI · Product Analytics  
**Approach:** Independent product and analytics problem-solving exercise  
**Source:** [Original Notion document](https://app.notion.com/p/40fe7cf19bdd813b8d1fd4d4fa104e74)

> **Evidence note:** This study distinguishes documented third-party evidence, illustrative or synthetic datasets, proposed tests and actual measured outcomes. It must not be interpreted as a deployed solution or verified uplift unless explicitly supported in the study.

---

> **Problem: Improve trust, reliability and accuracy of AI agentic systems.**
>
> **Specific task:** A finance analyst asks an AI agent to calculate net revenue retention from customer subscription records. The agent must use the correct definition, combine the right data, perform the arithmetic, provide supporting evidence and know when to ask for review.
>
> **Research status:** This is a worked, independent PM + Analytics case study using a fully disclosed synthetic business dataset. The calculation and root cause in the constructed example are solved. No third party AI product has been tested and no real world accuracy improvement is claimed.


## 1. Executive decision

An agent should not be considered reliable simply because it produces a coherent answer or includes a citation. The first product improvement I would test is a **metric definition check with independently calculated numbers and traceable source records**.

I would not rebuild the whole agent. I would introduce this control at the point where its answer depends on a business calculation. Then I would measure whether correct answers increase without adding unacceptable delays or human review.

## 2. Define the business problem precisely

A financial planning analyst needs the October net revenue retention percentage for a subscription business. The company's financial data comes from billing records and customer changes. A wrong definition can create a plausible but misleading performance claim.

The issue to solve is: **How can an analyst tell that an AI agent used the right customer population, source records and formula, before relying on its answer?**

Success means a correct percentage, a clear formula, links to the underlying data and a visible warning if an input is missing. More generated words are not success.

## 3. What the agent is asked to do

**User request:** “What is October net revenue retention, and did expansion offset losses from existing customers?”

**Illustrative source data, in ₹ lakh of monthly recurring revenue:**

| Input | October value | Meaning |
| --- | --- | --- |
| Opening MRR from existing customers | 100 | MRR at the beginning of the month |
| Lost MRR from those customers | 8 | Customers who canceled |
| Contraction in those customers | 4 | Existing customers spending less |
| Expansion in those customers | 15 | Existing customers spending more |
| New customer MRR | 20 | Customers acquired during October |

**Correct NRR:** (100 − 8 − 4 + 15) ÷ 100 = **103%**.

**Correct gross revenue retention:** (100 − 8 − 4) ÷ 100 = **88%**.

**Ending total MRR:** 100 − 8 − 4 + 15 + 20 = **₹123 lakh**.

A deliberately faulty agent response might say “NRR is 123%” by dividing total ending MRR of ₹123 lakh by the opening ₹100 lakh. That is incorrect because new customer MRR belongs in ending total MRR but is excluded from NRR.

**Completed conclusion for the exercise:** Expansion of ₹15 lakh exceeded churn and contraction of ₹12 lakh by ₹3 lakh, so existing customer revenue grew 3%. New business added another ₹20 lakh, but it must not inflate NRR.

## 4. Current user and system journey

The analyst asks a question. The assistant identifies a metric, calls a billing or warehouse tool, extracts five inputs, applies a formula, writes an answer and cites a source. The analyst checks it before entering it into a management report.

| Stage | Expected behavior | What can go wrong |
| --- | --- | --- |
| Interpret | Recognize the request as NRR, not total MRR growth | Wrong metric definition |
| Retrieve | Fetch the same cohort and period from billing records | New and existing customers mixed |
| Calculate | Apply the NRR formula deterministically | Freeform arithmetic or incorrect denominator |
| Explain | Distinguish expansion, losses and new business | Misleading growth narrative |
| Verify | Provide the exact inputs, period and source | Citation points only to a dashboard title |
| Approve | Analyst confirms before publication | Unchecked error enters a report |

This is a proposed workflow for the case study, not an observation of a named company's product.

## 5. Root cause analysis

**Observed failure in the constructed fixture:** The example faulty answer says 123%; the correctly defined result is 103%.

The difference is not a rounding problem. It is caused by including ₹20 lakh of new customer MRR in a metric defined around the existing customer cohort.

| Candidate cause | Test | Decision |
| --- | --- | --- |
| Billing values are wrong | Independently reconcile all five MRR components | Not indicated by this synthetic example |
| A customer is counted twice | Check unique customer identifiers and effective dates | Needs a separate validation case |
| Wrong cohort definition | Compare cohort membership against metric contract | Directly explains the constructed error |
| Arithmetic failed | Recalculate with a simple formula | Formula itself is straightforward once inputs are right |
| Agent cited data but not its method | Ask reviewer to reconstruct numerator and denominator | Makes error difficult to notice |

**Confirmed root cause within this synthetic example:** The wrong customer population was used. Real agent failures would require log analysis and labelled examples before making the same diagnosis.

## 6. Compare solutions

**Option A: Add more prompt instructions.** Cheap, but model behavior may remain inconsistent.

**Option B: Show raw billing tables and let analysts check everything.** Verifiable, but leaves substantial manual work.

**Option C: Enforce a metric contract, calculate it independently and display evidence.** More engineering work, but directly prevents the error demonstrated here.

**Decision:** Test Option C on NRR first. Keep a manual review path. Do not infer that every AI question needs the same control.

## 7. Proposed product solution

The assistant should first retrieve a metric definition: opening existing customer MRR; remove churn and contraction; add expansion; exclude new customers. It then gathers the five inputs with source IDs and periods. A calculation function computes NRR. A validation step checks completeness and any conflicting population definitions.

The response can say:

> October NRR was 103%. Existing customers began with ₹100 lakh of MRR, lost ₹12 lakh through churn and contraction, and added ₹15 lakh of expansion. New customer MRR of ₹20 lakh is excluded from NRR. \[View inputs and calculation\]

The system should not produce a definitive percentage if opening MRR is missing, the source period is inconsistent or customer IDs cannot be classified reliably.

**Minimum requirements:** Display formula and units; preserve source batch; distinguish existing from new customers; report data freshness; make results reproducible; allow analyst correction; keep an audit log.

## 8. Data model and validation

| Field | Why needed |
| --- | --- |
| customer_id, contract_id | Unique subscription identity |
| billing_period, source_as_of | Consistent reporting date |
| opening_mrr, ending_mrr | Reconciliation |
| change_type | New, churn, contraction or expansion |
| metric_definition_version | Stable meaning across runs |
| source_record_id, tool_run_id | Evidence and debugging |
| calculation_result, reviewer_status | Independent check and approval |

Validation rules include no duplicate customer and period keys, nonnegative opening MRR, consistent currency, stable account identity, reconciled total movements, and explicit treatment of midmonth new accounts.

## 9. North Star metric, KPIs and guardrails

| Measure | Exact definition | Baseline |
| --- | --- | --- |
| Product North Star candidate | Verified analytical questions completed per active team each week | Unknown |
| Primary success | Fully correct and supported task answers ÷ eligible tested questions | Not measured |
| Metric definition accuracy | Questions using correct approved formula and cohort ÷ evaluated questions | Synthetic error identified only |
| Numerical error rate | Incorrect numerical claims ÷ reviewed numerical claims | Not measured |
| Verification time | Median time to approve a correct answer | Not measured |
| Human correction burden | Manual corrections needed per eligible task | Not measured |
| Guardrails | Wrong approvals, excessive latency, rejected correct answers, cost per verified task | Not measured |

A success claim needs both correctness and completion. If the system refuses every answer it would avoid numerical errors while failing the product's purpose.

## 10. Test design

Create 24 fixed source cases covering new customers, churn, expansion, reactivation, mixed currencies, duplicates, late transactions and missing data. Freeze the gold calculation and expected behavior before testing.

Compare a basic agent workflow with the same workflow using a metric contract and independent calculation. Have finance reviewers grade correctness, evidence, appropriate abstention, timing and effort.

**Decision rule:** Continue only if critical errors are reduced on held out cases and reviewers still complete realistic tasks efficiently. A small synthetic benchmark is a feasibility check, not proof of production improvement.

## 11. Business value and recommendation

The value mechanism is fewer wrong or manually reworked analytical answers. Potential time benefit equals eligible correctly completed tasks × measured reduction in checking minutes. Do not convert time saved into payroll savings without evidence.

**Final case decision:** The root cause and correct result are resolved in the synthetic example. Recommend a limited metric contract plus deterministic calculation pilot, not a full agent redesign.

**What remains unknown:** Actual customer error frequency, availability of approved metric definitions, performance of existing products, and user improvement in real workflows.

**Portfolio evidence produced:** Defined user problem, independently checked formula, root cause analysis, solution alternatives, specific requirements, measurable evaluation and a conditional product decision.

---

*Preserved from the original Notion case study. The content is independently authored; any synthetic examples and limitations remain disclosed above.*
