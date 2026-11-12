# Finding and Verifying Business Information Faster

*Financial Intelligence & Research Operations | Independent Analytical Case*

## 1. The ask

**How can an investment or corporate-development analyst find the minimum set of documents required to answer a business question accurately, without confusing company, metric or reporting period?**

In this case, an analyst must answer:

**“How much did Helios Software's net revenue retention change from FY25 to FY26, and does the definition include new customers?”**

Helios Software is a fictional company. The decision is not whether to add another search bar; it is whether the research workflow reliably produces an answer that can be checked against its supporting documents.

## 2. Results from the worked case

A **synthetic six-document corpus** was created with known relevance labels.

| Document | Description | Needed? |
| --- | --- | --- |
| D1 | Helios FY26 signed metrics table: NRR 112% | Yes |
| D2 | Helios FY25 signed metrics table: NRR 107% | Yes |
| D3 | Helios FY26 investor deck: logo retention 95% | No — different metric |
| D4 | Helios FY24 press note: NRR 120% | No — wrong period |
| D5 | Vega Software FY26: NRR 118% | No — wrong company |
| D6 | Helios FY26 metric appendix: NRR excludes new-customer revenue | Yes |

**Verified answer within the fixture:** Helios NRR rose from **107% to 112%**, an increase of **5 percentage points**. Newly acquired customer revenue is excluded. D1, D2 and D6 are all required to support that complete answer.

Two **invented result orders** illustrate the search challenge:

| Retrieval test on the designed rankings | Broad result list | Constrained result list |
| --- | ---: | ---: |
| Required sources in first three results | 1 of 3 | 3 of 3 |
| Precision at three | 33.3% | 100% |
| Recall at three | 33.3% | 100% |
| Full answer support available in first three | No | Yes |

These percentages describe the supplied document orders. They are **not measured search-quality improvements** and are not results from AlphaSense or another commercial system.

## 3. Business impact and value case

A complete research answer requires less than a relevant-looking document: it requires the **right company, right metric, right time period and the definition needed to interpret the number**.

The business opportunity is to reduce repeated query reformulations, analyst verification effort and unsupported claims in investment or strategic materials. Potential impact should be measured through **correctly supported tasks completed** and **time to verified answer**, not clicks or raw search speed.

Actual time saved, customer benefit and commercial retrieval performance are unknown.

## 4. Analysis and root cause

In the constructed broad ranking, logo retention appears before NRR, a competitor's document competes with the target company, and the metric-definition appendix does not appear in the top five.

The complete source set nevertheless exists in the corpus. Therefore the worked failure is **not missing document coverage**. It is insufficient alignment of retrieval with the requested company, metric, period and verification requirement.

For a real product, the causal explanation may instead involve retrieval relevance, source permissions, poor query construction, misleading document labels or failure to use existing filters. The design must test these alternatives rather than assume the search engine is defective.

Tools such as AlphaSense already describe [document search](https://help.alpha-sense.com/hc/en-us/articles/52910432213907-Searching-in-Document-Search) and [source selection](https://help.alpha-sense.com/hc/en-us/articles/42208598365203-Search-Library-Identifying-the-Right-Sources-for-My-Search). The proposed workflow should be evaluated against those existing capabilities rather than positioned as an allegedly absent feature.

## 5. Proposed solution

Test a **task-oriented Research Evidence Packet** that assembles the support needed for one verified answer.

**Workflow:** Define question → identify company and metric → set fiscal periods → search available sources → confirm the essential evidence → calculate → cite → review.

The packet would present:

- **Answer:** FY25 NRR 107%; FY26 NRR 112%; change +5 percentage points.
- **Evidence:** Direct references to D1 and D2 for the figures and D6 for the definition.
- **Context:** Company identity, fiscal years, document versions and metric terminology.
- **Exceptions:** Missing definitions, contradictory evidence, uncertain company matches or inaccessible documents.

Start with existing company/date/source filters and task guidance. Add a product intervention only if realistic user sessions reveal persistent problems that the current capabilities do not solve.

## 6. Validation plan and success measures

Create at least **20 labelled research tasks** across company aliases, lookalike metrics, fiscal-year boundaries, conflicting sources and incomplete coverage. Have domain reviewers identify necessary evidence independently of ranking.

Compare current user workflow with structured task guidance or a compact evidence packet on counterbalanced tasks.

| Measure | Definition |
| --- | --- |
| Primary success | Complete, correct answers supported by all required sources / eligible tasks |
| Time to verified answer | Median elapsed time until the analyst accepts the supported answer |
| Evidence quality | Required source coverage and incorrect citation rate |
| Retrieval diagnostics | Precision and recall at fixed ranks, by failure class |
| Guardrails | Critical missed documents, false matches and additional workflow burden |

A changed ranking is worthwhile only if it improves *complete research outcomes*, not simply a search metric in isolation.

## 7. Recommendation

**Treat verification as a research-task outcome, not a decorative citation feature.** Test existing search controls first; where gaps remain, use constrained retrieval and an inspectable evidence packet. A future product commitment depends on observed analyst difficulty and measured task performance.

**Research status:** The synthetic corpus, correct answer and ranking diagnostics are complete. Live product performance and customer impact have not been measured.

### Public references

- [Document Search — AlphaSense help](https://help.alpha-sense.com/hc/en-us/articles/52910432213907-Searching-in-Document-Search)
- [Choosing the right research sources — AlphaSense help](https://help.alpha-sense.com/hc/en-us/articles/42208598365203-Search-Library-Identifying-the-Right-Sources-for-My-Search)

[← Research & Writing index](README.md)
