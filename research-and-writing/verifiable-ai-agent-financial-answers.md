# Designing Verifiable AI-Agent Financial Answers

*Product Management & AI Reliability | Independent Analytical Case*

## 1. The ask

**How can a finance analyst trust an AI agent's answer to a financial question before using it in a management report?**

Consider an analyst asking: **“What was October net revenue retention (NRR), and did expansion revenue offset losses from existing customers?”** The answer must use an approved definition, the correct customer cohort, reconciled billing records and reproducible arithmetic. A plausible narrative or a citation to a dashboard is insufficient.

**Decision to make:** Identify the smallest product control that prevents materially incorrect financial answers without turning every request into an expensive manual review.

## 2. Results from the worked case

The following is a controlled **synthetic business example**, not a production evaluation of any AI vendor.

| October recurring-revenue movement | Amount |
| --- | ---: |
| Opening MRR from existing customers | ₹100 lakh |
| Churn from existing customers | −₹8 lakh |
| Contraction from existing customers | −₹4 lakh |
| Expansion from existing customers | +₹15 lakh |
| New-customer MRR | +₹20 lakh |

**Correct NRR = (100 − 8 − 4 + 15) / 100 = 103%.**

**Gross revenue retention = (100 − 8 − 4) / 100 = 88%.**

**Ending total MRR = 100 − 8 − 4 + 15 + 20 = ₹123 lakh.**

A deliberately faulty answer reports **123% NRR** by including new-customer revenue in a metric that measures the existing cohort. The error is **20 percentage points** relative to the correct 103% figure. Expansion of ₹15 lakh offsets ₹12 lakh of churn and contraction, leaving **₹3 lakh of net expansion within the opening cohort**.

**Analytical result:** The case isolates a metric-definition and cohort-selection error, not an arithmetic mistake or missing data. Correctness requires inspecting *which* revenue entered the numerator, not merely checking the final calculation.

## 3. Business impact and value case

The potential value is a lower rate of unsupported financial claims, less analyst rework and stronger confidence in decisions built on AI-generated outputs.

A practical value equation is:

**Capacity recovered = verified tasks completed × reduction in review minutes per task.**

There is **no observed production accuracy uplift or time saving** from this exercise. The 20-percentage-point discrepancy is a correction within one designed example—not a measured improvement in an AI system. A commercial benefit would require actual task volumes, review effort, error severity and operating cost.

## 4. Analysis and root cause

A reliable analytical response passes through six steps:

| Stage | Required control | Failure illustrated or risk to test |
| --- | --- | --- |
| Interpret | Resolve the approved NRR definition | Confusion with total MRR growth |
| Retrieve | Use matching customer and reporting-period records | Mixing newly acquired and existing customers |
| Calculate | Execute a deterministic formula | Unchecked generative arithmetic |
| Explain | Reconcile expansion, churn and contraction | Misleading performance narrative |
| Evidence | Retain inputs, source IDs and calculation version | A citation that does not support the number |
| Approve | Route uncertain or material results for review | Unsupported number reaching management |

In the worked case, adding the ₹20 lakh of new-customer MRR to the existing-cohort numerator directly explains the faulty answer. In a real deployment, that diagnosis must be established from tool traces, source rows and a labelled evaluation set rather than inferred from the answer alone.

## 5. Proposed solution

Introduce a **metric contract and independent calculation layer** at the point where an agent makes a business-metric claim.

1. **Definition:** Retrieve the approved metric formula, eligible customer cohort, reporting period and exclusions.
2. **Evidence:** Fetch source records with customer IDs, billing dates, currencies and source versions.
3. **Computation:** Run a deterministic calculation instead of relying on a language model to infer arithmetic.
4. **Validation:** Check duplicate records, missing opening balances, inconsistent periods and reconciliation.
5. **Presentation:** Display the figure, numerator, denominator, contributing movements, evidence and freshness.
6. **Escalation:** Withhold a definitive answer when a critical input is missing; allow a finance reviewer to approve or correct it.

This is intentionally narrower than redesigning an entire agent. Existing tools and finance-approved calculation functions should be reused before building new infrastructure.

## 6. Validation plan and success measures

Build a **24-case fixed benchmark** spanning churn, reactivation, expansion, missing data, duplicated customers, late transactions and currencies. Compare the same agent with and without the metric contract, using identical questions and source records. Freeze gold calculations in advance and have finance reviewers evaluate the outputs.

| Measure | How to evaluate it |
| --- | --- |
| Primary success | Fully correct, adequately supported answers / eligible test questions |
| Metric-definition accuracy | Answers using the correct cohort and approved formula / evaluated answers |
| Verification time | Median analyst minutes to approve an answer |
| Error severity | Material incorrect claims and incorrect approvals |
| Guardrails | Unnecessary abstentions, latency, cost and manual workload |

A useful pilot must reduce *critical* errors on held-out tasks while keeping legitimate questions answerable and review effort reasonable.

## 7. Recommendation

**Pilot the metric contract and deterministic calculation for NRR before expanding to other financial metrics.** Preserve human approval for material reporting, audit corrections, and scale only after actual users and held-out test cases support the result.

**Research status:** The sample arithmetic, discrepancy and proposed control are complete. Real-world error prevalence, customer outcomes and product performance remain unmeasured.

[← Research & Writing index](README.md)
