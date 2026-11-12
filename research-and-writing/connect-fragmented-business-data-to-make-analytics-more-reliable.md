# Connect fragmented business data to make analytics more reliable

*P02 · Product Management & Analytics Research*

## Problem overview

**Problem statement:** Teams work with financial and operational data spread across accounting, CRM and other systems. Delayed updates, mismatched definitions and unclear source quality can make reports hard to trust. Identify where source-to-metric problems create the most work, then improve data reliability and use. Drivetrain report freshness is one focused example.

**Primary user / persona:** Finance teams preparing monthly reports using data from accounting and sales tools.

**Core hypothesis:** Making incomplete updates and source-to-metric dependencies clearer can reduce avoidable checks and help analysts decide which numbers are ready, if current product workflows leave that gap.

**Expected outcomes:** More dependable business reports, less manual checking, faster investigation of data issues and greater confidence in decisions. Benefits are desired, not measured.

**Success metrics:** Correct source identification; data completeness; time to diagnose discrepancies; manual reconciliation effort; reports with unnoticed stale inputs.

**Evidence status:** Public signals only

**Research status:** Analysis

**Target companies / research contexts:** Drivetrain (focused example) · Aleph · Cube Software · Abacum

---

## Detailed case study

> **Research decision: Make figures comparable before putting them on a shared dashboard.**
>
> User: an operating partner reviewing results from several acquired businesses. Business need: compare revenue quality and operating performance across a portfolio. Relevant industry example: Drivetrain and its Diversis Capital customer story.
>
> **Status:** A documented real customer problem supports this case. The two company numbers below are explicitly illustrative and computed here. The product design and KPI plan are original; no customer deployment or improved outcome is claimed.

## 1. Executive conclusion

The first solution is not another dashboard. It is a **single metric definition, source map and reconciliation check** for each important portfolio KPI, starting with recurring revenue. That lets a portfolio manager compare businesses without quietly mixing different reporting rules.

The immediate decision in this case: Do not publish a consolidated recurring revenue KPI unless each contributed value uses the same definition, period and company boundary. Publish exceptions instead of filling gaps with guessed values.

## 2. Evidence that this is a real business problem

A published [Diversis Capital customer story](https://www.drivetrain.ai/customer-stories/diversis-capital-achieves-real-time-reporting-clarity) describes an investment operating partner trying to aggregate data across 15 portfolio companies. Companies used different formats and definitions for financial and SaaS metrics, which undermined comparability. A historical adjustment also missed segment information, affecting the reliability of retention and growth by segment. This is a customer account published by a vendor, so it is meaningful evidence but not an independent, representative failure survey.

A separate [Whatfix customer story](https://www.drivetrain.ai/customer-stories/whatfix-automated-financial-reporting-drivetrain) describes the operational difficulty of combining Salesforce and NetSuite information with many spreadsheet versions. The same story reports substantial improvements after adoption of existing software. Therefore we cannot claim Drivetrain lacks data centralization, mapping or reporting.

The opportunity for an independent product case study is to design and test **how a user knows that a shared figure is actually comparable and ready**, not to invent a missing database connector.

## 3. A specific decision and user journey

The investment operating partner wants to answer: **“What is the portfolio's annual recurring revenue this month, calculated on the same basis for every company?”**

The current manual workflow, generalized from the published problem, is:

1. Request monthly reports and definitions from portfolio finance teams.
2. Find relevant revenue and subscription fields in each company.
3. Compare definitions and reporting dates.
4. Exclude one time services and resolve differences.
5. Combine figures, check outliers, ask for clarifications and publish.
6. Reopen questions when a company restates a report.

The work is not finished when all files arrive. It is finished when the combined number is semantically consistent and traceable.

## 4. A worked example that exposes the error

The following two companies are **synthetic**. They are not actual Diversis portfolio holdings or actual Drivetrain records. They show why joining data is not enough.

Both companies send a line labelled “monthly revenue”. Company A counts ₹90 lakh of recurring subscription revenue plus ₹10 lakh of one time implementation fees. Company B counts ₹60 lakh of recurring subscription revenue plus ₹5 lakh of one time fees.

| Metric | Company A | Company B | Portfolio |
| --- | --- | --- | --- |
| Reported monthly revenue | ₹100 lakh | ₹65 lakh | ₹165 lakh |
| Recurring subscription component | ₹90 lakh | ₹60 lakh | ₹150 lakh |
| One time fees | ₹10 lakh | ₹5 lakh | ₹15 lakh |
| Incorrect annualization of all monthly revenue | ₹12 crore | ₹7.8 crore | ₹19.8 crore |
| Comparable recurring ARR, 12 × recurring monthly component | ₹10.8 crore | ₹7.2 crore | ₹18 crore |

**Result:** Simply multiplying the combined reported monthly revenue by 12 gives ₹19.8 crore. Standardizing the definition to recurring subscription revenue produces ₹18 crore. The difference is **₹1.8 crore, or 10% of the correct ₹18 crore**.

This is a definition mismatch, not an indication that either company booked the wrong general ledger revenue. ARR is a commercial run rate metric; it must not be described as audited annual revenue. For a real portfolio, additional adjustments could be needed for contract duration, concessions, FX, cancellations and acquired businesses.

## 5. Root cause analysis

**Observed symptom in the example:** Combined ARR is overstated.

| Candidate cause | Test | Finding in the synthetic example |
| --- | --- | --- |
| Source system failed to sync | Check refresh timestamp and row counts | Not required to explain this example |
| One company has different metric rules | Break reported revenue into recurring and one time categories | Confirmed in the provided synthetic numbers |
| The analyst used an invalid formula | Review annualization basis | Confirmed: total monthly sales were treated as ARR |
| Duplicate customers were counted | Check subscription and customer identifiers | Not evaluated; must be tested |
| Reporting periods differ | Compare period ends, effective dates and FX | Not evaluated; must be tested |

**Root cause for this worked scenario:** The portfolio combines incompatible definitions and then applies the ARR formula to the wrong input. The correction is to change the definition and source mapping before aggregate reporting.

The published customer story supports the relevance of inconsistent definitions, but it does not prove the same mistake occurred in the reported portfolio.

## 6. Product solution: one metric contract and exceptions workflow

Each portfolio company maps its source data to a centrally agreed metric. The platform records which sources and rules produced the number, checks the input, and returns either a verified figure or an exception to resolve.

**ARR metric contract:**

| Field | Required definition |
| --- | --- |
| Name | Comparable recurring ARR |
| Formula | 12 × eligible monthly recurring subscription revenue, for this simplified exercise |
| Excluded | One time implementation, consulting, equipment or nonrecurring fees |
| Reporting grain | One legal entity, monthly reporting period, subscription component and currency |
| Currency | Currency at company level; convert through a disclosed, consistent FX policy before aggregation |
| Required metadata | Source, cutoff, mapping version, accounting owner, approval date |
| Status | Draft, needs clarification, validated, approved |
| History | Keep old source, rule and published value when restated |

**Minimum product behavior:** Show the corrected portfolio value, and for each contributing company a link to its figures, exclusions and reconciliation. If source definitions change or a required month is missing, show the affected portfolio metric as needing review. Do not silently use the last known number as current.

### Example output

| Company | Reported annualized amount | Comparable ARR | Issue | Action |
| --- | --- | --- | --- | --- |
| A | ₹12 crore | ₹10.8 crore | One time implementation included | Exclude ₹10 lakh monthly component |
| B | ₹7.8 crore | ₹7.2 crore | One time services included | Exclude ₹5 lakh monthly component |
| Combined | ₹19.8 crore | ₹18 crore | Values not initially comparable | Publish corrected definition with evidence |

## 7. Data and SQL logic

Minimum fields: company\_id, source\_record\_id, product\_type, contract\_id, customer\_id, currency, period\_end, revenue\_amount, is\_recurring, source\_loaded\_at, mapping\_version, approval\_status.

Example analysis logic, independent of a specific vendor implementation:

```sql
SELECT
  company_id,
  period_end,
  SUM(CASE WHEN is_recurring = 1
           THEN revenue_amount ELSE 0 END) * 12 AS arr,
  SUM(CASE WHEN is_recurring = 0
           THEN revenue_amount ELSE 0 END) AS excluded_nonrecurring
FROM normalized_monthly_revenue
WHERE period_end = :reporting_month
  AND approval_status = 'approved'
GROUP BY company_id, period_end;
```

This simplified SQL assumes source rows already represent monthly amounts in a common currency, that the recurring flag is correct, and that duplicates have been removed. Actual production logic requires contract effective dates, currency conversions and versioned restatements.

Quality checks: same cutoff month, unique contract and component keys, valid currency, no missing reporting entities, totals reconcile to source components, and each published metric can reproduce its input rows.

## 8. Requirements and trade-offs

1. A data owner must approve the business definition and mapping for each company.
2. Every number must record company, period, version, data source and transformation.
3. Unresolved exceptions must be visible at the company and portfolio level.
4. A revised mapping must recalculate affected history with explicit approval, not overwrite it silently.
5. Users must be able to see reported data and standardized data side by side.
6. The workflow must not pretend a missing value is zero.

**Alternative:** A spreadsheet checklist could solve this for a very small portfolio at lower cost. A product workflow becomes attractive when many companies, revisions, metrics and reviewers make manual consistency unreliable. If existing platform features already provide this, prefer configuring the contract and controls, not building new features.

## 9. North Star, KPIs and guardrails

| Measure | Exact definition | Status |
| --- | --- | --- |
| Product North Star candidate | Portfolio monthly decisions supported by approved, comparable metrics per active operating team | Not measured |
| Primary case success | Portfolio metric submissions passing source and definition checks ÷ eligible submissions | Not measured |
| Comparison correctness | Correct portfolio aggregates ÷ checked aggregate cases | Synthetic example correct, no broad rate |
| Reconciliation time | Median minutes per reporting cycle spent resolving metric mapping | Not measured |
| Exception resolution | Exceptions closed with evidence ÷ identified exceptions within agreed time | Not measured |
| Adoption | Portfolio companies submitting approved canonical definitions ÷ active portfolio companies | Not measured |
| Guardrails | False blocks, incorrect FX, missed restatements, late reports, manual review burden | Not measured |

**Business impact:** Avoided reconciliation effort plus fewer decisions based on inconsistent figures. Do not assign an investment return or payroll saving to this exercise without actual evidence.

## 10. Experiment and decision memo

Create a test workbook with five companies and deliberately varied metric rules. Prepare known correct calculations independently. Give finance analysts the same data and task using (A) ordinary source spreadsheets and (B) a standard definition plus exception view. Measure first pass accuracy, time to correct portfolio ARR and reviewer explanations. Include missing currency, late source and duplicate contract cases. Audit current commercial product features before proposing code.

**Decision:** The worked ₹19.8 crore versus ₹18 crore comparison establishes that inconsistent definitions can materially distort a combined metric. Recommend a versioned metric contract and reconciliation gate as the minimal solution to that class of error. A company specific rollout is conditional on seeing actual user errors and comparing with existing capabilities.

**Would reverse the decision if:** Existing workflows already enforce correct definitions, and the errors are caused chiefly by missing underlying transactions rather than definitions.

**Completed:** Public business evidence, concrete data example, verified calculation, proposed contract, metrics, and test design. **Unproven:** Real error prevalence, customer saving, and live product improvement.
