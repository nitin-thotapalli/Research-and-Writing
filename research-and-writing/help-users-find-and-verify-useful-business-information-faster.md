# Help users find and verify useful business information faster

*P03 · Product Management & Analytics Research*

## 1. The case

**Problem statement:** An investment or corporate-development analyst must answer a precise company-research question, but a broad document search may return the wrong entity, fiscal year, or metric. A plausible answer is not useful unless its supporting documents establish both the figures and their definitions.

The fictional task: determine how Helios Software's net revenue retention changed from FY25 to FY26 and whether it includes new customers.

## 2. Proposed solution

**Recommendation:** First use existing company, reporting-period, and source filters. Where research tasks still fail, test a concise **Research Evidence Packet** that keeps the question, required documents, key figures, metric definition, and citations together.

Do not assume a new search feature is necessary until observation shows that the current retrieval and verification workflow is insufficient.

## 3. Worked results and business impact

**Worked result:** In the invented six-document corpus, NRR rises from **107% to 112%**, or **5 percentage points**, and excludes new-customer revenue. Three documents are required for the complete answer. A broad illustrative ranking returns **1 of 3** needed sources in the top three; a designed constrained ranking returns **3 of 3**.

**Potential business impact:** Faster verified research and fewer unsupported memo claims. Evaluate complete task accuracy, required-source coverage, query reformulations, and time to a reviewer-accepted answer.

**Evidence boundary:** The search rankings are synthetic test fixtures, **not** a benchmark or proven improvement in AlphaSense or any other live product.

---

## 4. How we arrived at the recommendation

### Decision rationale

Before designing more search filters or AI summaries, test whether users can locate **the minimum source set required to answer a specific business question correctly**. In the constructed corpus below, a generic search ranking surfaces a competitor and the wrong metric before the necessary definition document. A query with company, year and metric constraints resolves the example.

**Product recommendation:** Use the existing company and document filters wherever they are available. Where observed users still fail, test a small task focused search experience that keeps the research question, required document types and supporting citations together.

### User, business context and problem statement

An analyst is comparing possible acquisition targets. For one business, the analyst needs a defensible retention trend for a short screening note. The analyst should not spend most of the time repeating searches and figuring out which sources discuss the right company, period and type of retention.

**User question:** “How much did Helios Software's net revenue retention change from FY25 to FY26, and does this metric include new customers?”

Helios Software is fictional. No claims are being made about a real business.

The desired outcome is a correct answer with the documents needed to support it. A search result that looks relevant but describes logo retention, a competitor or an old period is not sufficient.

### Evidence available about the product category

Financial search tools already provide powerful capabilities. For example, [AlphaSense's document search guide](https://help.alpha-sense.com/hc/en-us/articles/52910432213907-Searching-in-Document-Search) describes company and keyword search. Its [source selection guide](https://help.alpha-sense.com/hc/en-us/articles/42208598365203-Search-Library-Identifying-the-Right-Sources-for-My-Search) helps users choose among different document types.

Earlier research in this workspace recorded dated individual reviews discussing relevance and advanced filtering. Those reviews justify further investigation, but they do not prove how frequently a current customer experiences the problem.

**Important design constraint:** Do not claim existing financial intelligence tools lack company filters, source selection, citations or document review features. This case tests the user task and the quality of its result.

### The six document research corpus

All six documents below are invented test fixtures. Their IDs, titles, values and search order are supplied so the case can be reproduced, not presented as real third party research.

| Document | Content | Relevant to this question? | Reason |
| --- | --- | --- | --- |
| D1 | Helios FY26 signed metrics table: NRR 112% | Yes | Current year value |
| D2 | Helios FY25 signed metrics table: NRR 107% | Yes | Prior year value |
| D3 | Helios FY26 investor deck: logo retention 95% | No | Different metric |
| D4 | Helios FY24 press note: NRR 120% | No | Wrong year |
| D5 | Vega Software FY26: NRR 118% | No | Wrong company |
| D6 | Helios FY26 metric appendix: NRR uses existing customers and excludes newly acquired customer revenue | Yes | Required definition |

**Correct answer:** NRR increased from 107% in FY25 to 112% in FY26, an increase of **5 percentage points**. The definition excludes revenue from new customers. The correct answer requires D1, D2 and D6. Do not confuse a 5 percentage point increase with 5% relative growth.

### Reproduce a search problem

Consider an illustrative ranking for a broad search “Helios retention 2026”:

| Position | Broad search result | Classification |
| --- | --- | --- |
| 1 | D3, Helios logo retention | Related company, wrong metric |
| 2 | D5, Vega NRR | Wrong company |
| 3 | D1, Helios FY26 NRR | Relevant |
| 4 | D4, Helios FY24 NRR | Wrong year |
| 5 | D2, Helios FY25 NRR | Relevant but not early |
| Not in top five | D6, definition appendix | Essential source missed |

A more specific query intent, “Helios Software net revenue retention, FY25 versus FY26, definition”, with entity and metric matching produces this **designed candidate** order: D1, D2, D6, D3, D4.

| Measurement on synthetic ranks | Broad ranking | Constrained ranking |
| --- | --- | --- |
| Relevant documents in top three | 1 | 3 |
| Precision at three | 1 ÷ 3 = 33.3% | 3 ÷ 3 = 100% |
| Recall at three, with three required documents | 1 ÷ 3 = 33.3% | 3 ÷ 3 = 100% |
| Can the full question be answered from top three? | No | Yes |

**These are calculated properties of invented rankings.** They are not actual improvements achieved by an algorithm or commercial product. Their purpose is to define how the product decision should be evaluated.

### Root cause analysis

The broad result list contains all the right information in the six document corpus, but the top results do not assemble the required evidence early enough. That means the demonstrated failure is not missing content.

| Possible cause | Test | Conclusion for this fixture |
| --- | --- | --- |
| Missing source coverage | Count required D1, D2 and D6 in indexed corpus | Rejected: all three exist |
| Company ambiguity | Check whether D5 is surfaced despite wrong company | Confirmed as one relevance issue |
| Metric ambiguity | Check whether D3 outranks NRR documents | Confirmed as another issue |
| Fiscal period matching | Check where D4 appears | Wrong period competes for attention |
| Verification workflow | Check whether the user opens D6 before answering | Required definition initially not in top five |
| Training gap | Watch a real user apply existing filters | Unknown until user research |

**Established cause in the synthetic fixture:** The broad search lacks enough entity, metric and period constraints. A real product study must still determine whether ranking, filters, query wording, source access or user experience is responsible for failures.

### Proposed product and analytics solution

The smallest useful experience is a **Research Evidence Packet** created from a user question and confirmed source constraints.

The system records the target company, the metric, reporting periods, the minimum evidence needed and the document sources. The analyst can inspect D1, D2 and D6 before writing the answer. The packet keeps any conflicting definition or unavailable source visible.

**Prototype contents:**

| Question element | Evidence | Result |
| --- | --- | --- |
| FY26 NRR | D1 | 112% |
| FY25 NRR | D2 | 107% |
| Difference | 112% minus 107% | Plus 5 percentage points |
| Metric definition | D6 | Existing customers, new customers excluded |
| Missing or conflicting evidence | None in this controlled sample | Ready for analyst review |

The research packet is an output of the workflow, not a replacement for search, and not a claim that products like AlphaSense lack comparable capabilities.

### Requirements and practical tests

1. Let the analyst specify company, metric and periods using existing product features before introducing any new UI.
2. Keep results tied to a canonical company ID and document date.
3. Distinguish logo retention and net revenue retention.
4. Show source title, reporting period, document version and excerpt for every numerical claim.
5. Explain when a source is missing, instead of silently using an unrelated document.
6. Preserve the user's reviewed evidence in a reusable research packet.
7. Allow analysts to reject irrelevant documents and record why.

**Acceptance tests:** A competitor's document must not silently substitute for the target; a prior year's report cannot appear as current without a label; an NRR answer must use an NRR definition; and the result must be reproducible from retained evidence.

### Metric hierarchy and data model

**Grain:** A query for retrieval diagnosis; a complete user task for product success; a query document pair for relevance grading.

| Metric | Exact meaning | Status |
| --- | --- | --- |
| North Star candidate | Correctly sourced research tasks completed per active team per week | Not measured |
| Primary success KPI | Tasks answered completely and accurately with required evidence ÷ eligible tasks | Not measured |
| Time to verified answer | Median time from task start to an accepted answer | Not measured |
| Precision at three | Relevant top three documents ÷ three | 33.3% versus 100% only in synthetic ordering |
| Recall at three | Required sources found in top three ÷ all required sources | 33.3% versus 100% only in synthetic ordering |
| Query reformulations | Number of revised queries needed for completed task | Not measured |
| Guardrails | Missed critical evidence, false matches, source coverage, analyst workload and latency | Not measured |

Minimum data fields: task\_id, query\_id, canonical\_company\_id, intended\_metric, reporting\_period, document\_id, document\_version, rank, relevance\_label, source\_access, opened\_at, task\_complete, reviewer\_result and time\_elapsed.

### Validation experiment

Build at least 20 source labelled research tasks across comparable documents, including company aliases, year boundaries, lookalike metrics, sparse sources and contradictions. Have reviewers label necessary documents without seeing the current ranking.

Give analysts realistic tasks using current search and source selection, then counterbalanced equivalent tasks with a structured evidence packet or query guidance. Compare task correctness, time, coverage, repeated queries and evidence quality. Check whether users already know how to use the available product controls.

**Decision rule:** If existing filters and guidance solve the problem, improve teaching or defaults. If constrained ranking measurably improves complete verified research without hiding important documents, consider a narrow relevance improvement. If documents are unavailable, solve coverage, not interface labeling.

### Outcome and final recommendation

**Completed exercise:** A distinct user question, six labelled source records, the correct five percentage point calculation, a reproduced retrieval error, a source packet and a measurable product hypothesis.

**Provisional decision:** Improve the research task through better use of company, metric and date context, verified source selection and a compact evidence packet. Do not propose a new search feature at a named company before testing its current product.

**Business value:** More correctly supported answers with less search and verification time. The benefit must be measured as successful verified tasks and minutes saved; clicks alone are a poor measure.

**Not established:** Actual AlphaSense retrieval performance, current failure frequency, customer demand or real world uplift. None of the fictional company data is represented as an actual financial research result.
