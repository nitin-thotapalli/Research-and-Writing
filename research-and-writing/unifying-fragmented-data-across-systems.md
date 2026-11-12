# Unifying fragmented data across systems: from disconnected events to one operational view

*P02 · Data Integration, Product Management & AI Infrastructure*

## 1. The case

**Broad problem:** Businesses operate across many applications. Each system records a different part of the same real-world process, often with different identifiers, timestamps, schemas, update schedules and definitions. Teams then spend time locating, joining, interpreting and reconciling those fragments before they can answer an apparently simple question.

**Worked example — logistics:** A fictional commerce operator wants to know **which orders were actually delivered on time yesterday**. Its commerce platform records orders, a warehouse management system records fulfilment, and two logistics partners record dispatch and delivery. The records exist, but the systems disagree about IDs and event timing.

**Product question:** How might we provide a reliable, traceable **unified order view** across these sources, without replacing each operational system or asking users to manually reconcile every exception?

The problem is deliberately cross-industry. The logistics example makes it concrete; the same mechanisms appear in healthcare (patient journeys), enterprise AI (tickets, code and deployments), customer operations (CRM, support and billing), and finance (transactions, contracts and ledgers).

## 2. Proposed solution

**Recommendation:** Build a narrowly scoped **unification layer around a shared business entity**, starting with an *order*. Ingest source records, map IDs to a canonical order identifier, standardize event types and time semantics, maintain provenance, reconcile conflicts and publish an explicit **resolved / incomplete / disputed** view through an API or queryable table.

Do **not** begin with a generic “single source of truth” platform, a new dashboard, or an LLM that freely guesses which records belong together. First prove one high-value cross-system question can be answered correctly and repeatably.

The solution has five parts:

1. **Connect:** Read from existing systems through approved exports, APIs or event feeds.
2. **Identify:** Join records using stable keys and explicit crosswalks; flag ambiguous matches for review.
3. **Normalize:** Convert source-specific statuses, event timestamps and units into a documented canonical contract.
4. **Resolve:** Apply versioned rules, quality checks and evidence-backed exception handling.
5. **Serve:** Expose the joined entity, its event history, freshness, confidence/status and source lineage to users, analytics tools or AI agents.

**Decision principle:** A unified record is useful only when the business meaning and provenance of its fields survive the transformation.

## 3. Worked result and business value

Consider six synthetic orders due for delivery by the end of a reporting day. The commerce system contains all six; the warehouse and carriers record different subsets.

| Order | Commerce ID | Carrier evidence | Resolved state | On-time verdict |
| --- | --- | --- | --- | --- |
| O-101 | C101 | Delivered 16:20, due 18:00 | Delivered | On time |
| O-102 | C102 | Delivered 19:10, due 18:00 | Delivered | Late |
| O-103 | C103 | Dispatched, no delivery scan | In transit | Not delivered by cutoff |
| O-104 | C104 | Delivered 17:05, due 18:00 | Delivered | On time |
| O-105 | C105 | Delivered 17:40, due 18:00; partner ID mapped by crosswalk | Delivered | On time |
| O-106 | C106 | Conflicting delivery timestamps from two partner feeds | Disputed | Exclude from confirmed calculation until resolved |

**Illustrative calculation:** Three orders are confirmed on time, one is confirmed late, one is not delivered by cutoff, and one is disputed. Among the **five orders with a resolved verdict**, the confirmed on-time share is **3 ÷ 5 = 60%**. Across **all six due orders**, the conservative confirmed-on-time share is **3 ÷ 6 = 50%**. These answer different questions and must be labelled separately; the disputed order must not be quietly counted as on time or silently discarded from an all-due denominator.

**What unification changes:** Without ID mapping, O-105 might appear missing; without reconciliation, O-106 might receive a false status; without a shared definition, one team could report 60% and another 50% as though they were the same metric.

**Potential value:** Faster cross-system answers, fewer manual lookups, fewer duplicate/misjoined entities, clearer exceptions, and a dependable context layer for downstream workflows and AI. These are **hypotheses to measure**, not claims of implemented improvement or observed financial returns.

---

## 4. How we arrived at the recommendation

### 4.1 Start with the user decision, not the technology

**Primary user:** An operations analyst responsible for a daily delivery-exceptions review.

**Secondary users:** A customer-support representative needing the latest defensible order status; a data engineer maintaining integrations; an AI assistant answering where an order is and citing the right events.

The job is not “centralize all data.” It is: *When an order is late, missing or disputed, determine what has happened and what action is appropriate without searching four systems.*

Success requires an answer that is timely, correctly joined, interpretable and backed by records. A beautiful dashboard that joins the wrong order is worse than a manual workflow that surfaces uncertainty.

### 4.2 Where fragmentation enters the journey

| Stage | Example source | What it knows | What it does not know |
| --- | --- | --- | --- |
| Order placed | Commerce/OMS | Customer order, promised date, line items | Actual carrier delivery |
| Picked and packed | Warehouse/WMS | Internal picking, packing, dispatch readiness | Final-mile receipt |
| Shipped | Carrier A/B | Tracking ID, scans, delivery attempt | Complete commercial order context |
| Customer enquiry | CRM/helpdesk | Customer conversation, escalation reason | Authoritative shipment event chain |
| Management review | Reporting layer | Aggregated view, if joined | Can inherit wrong mappings or stale states |

**Observed classes of fragmentation to test:** no shared key, duplicate IDs, inconsistent event names, timestamp/timezone mismatch, event-arrival delays, partial system outages, multiple records for one action, and conflicting source authority.

These are hypotheses for the example, not findings from a real operator.

### 4.3 Separate five kinds of “one data view”

They are often confused, and each needs a different control.

| Layer | Question | Minimum control |
| --- | --- | --- |
| Connectivity | Can the relevant records be read? | Connector authentication, schedule and error visibility |
| Identity | Do records refer to the same real-world entity? | Stable keys, mapping version and ambiguous-match queue |
| Semantics | Do fields and statuses mean the same thing? | Canonical schema, unit/time conversion and definitions |
| State | Which event/current value should be trusted? | Source hierarchy, effective time, conflict rules and history |
| Access | Can another workflow safely use the result? | Query/API contract, provenance, permissions and freshness metadata |

A connector solves the first layer; it does **not** automatically solve identity, semantics or conflicting truth.

### 4.4 Investigate before building

For the chosen daily review, collect a limited, permissioned sample from each source and answer:

1. What proportion of due orders can be linked deterministically across sources?
2. Which identifiers are unique, reused, missing or transformed by partners?
3. Do source clocks represent event time, processing time or upload time?
4. Which system owns each field, and when can a later event supersede an earlier one?
5. How often are statuses disputed, and how does an analyst resolve them today?
6. What is the acceptable data age for a daily report versus a live support request?
7. Which existing warehouse, integration-platform or operational-data-store capabilities already solve part of this problem?

A manual, reviewed data crosswalk can be a valid baseline. Introduce new software only if repeated reconciliation exceeds the value of configuration and existing tooling.

### 4.5 Root-cause tree and falsification

| Symptom | Candidate cause | Test | Recommended action if confirmed |
| --- | --- | --- | --- |
| Shipment appears unrelated to order | Missing/mismatched business key | Trace OMS ID → WMS ID → carrier tracking ID | Persist explicit key crosswalk and review unknown IDs |
| Order shows delivered before dispatch | Event-time parsing or wrong timezone | Compare raw timestamp, zone and effective time | Standardize event-time contract; retain original timestamp |
| Two delivery dates disagree | Duplicate messages or source conflict | Inspect event IDs, proof of delivery, revisions | Idempotent ingest plus source precedence and dispute queue |
| Dashboard misses yesterday's orders | Late data / stale synchronization | Compare event and ingest timestamps with SLA | Expose freshness, watermark and late-arrival correction |
| Delivery-rate denominator changes by dashboard | Different eligibility or cutoff rules | Reproduce inclusion rules side by side | Publish one versioned metric definition with variants named |

Do not fix all five mechanisms just because they are plausible. Measure their contribution to incorrect decisions and prioritize the dominant one.

### 4.6 Data design: unification without erasing history

**Canonical entities:** Order, Shipment, DeliveryEvent, SourceRecord and IdentityMapping.

**Illustrative order view:**

| Field | Definition |
| --- | --- |
| canonical_order_id | Stable internally assigned order key |
| source_system, source_record_id | Original record identity and system |
| tracking_id | Carrier tracking reference, not assumed globally unique |
| event_type | Standardized event from approved taxonomy |
| event_time_utc | When the underlying event occurred |
| ingested_at_utc | When the platform received the event |
| promised_delivery_at_utc | Normalized promise/cutoff |
| current_status | Derived using versioned, documented precedence rules |
| resolution_state | Resolved, incomplete or disputed |
| mapping_version, rule_version | Reproducibility of entity join and state calculation |
| evidence_refs | Source event IDs, URLs or signed references |
| last_verified_at | Age of latest completed validation |

Retain immutable raw records where permitted. Keep transformations reversible/auditable. Distinguish record corrections from fresh operational events.

**Identity strategy:**
- Prefer exact, externally supplied order-to-tracking relationships.
- Use deterministic composite keys only when uniqueness rules are proven.
- Never silently turn fuzzy text similarity into a confirmed business join.
- Route ambiguous, many-to-many or conflicting mappings to an analyst review queue with evidence and a decision log.

### 4.7 Example SQL: an auditable point-in-time result

This is an illustrative query against a pre-validated canonical event table. It is **not** the complete identity-resolution or ingestion pipeline.

~~~sql
WITH ranked AS (
  SELECT
    canonical_order_id,
    event_type,
    event_time_utc,
    source_event_id,
    ROW_NUMBER() OVER (
      PARTITION BY canonical_order_id
      ORDER BY event_time_utc DESC, ingested_at_utc DESC, source_event_id DESC
    ) AS rn
  FROM canonical_delivery_events
  WHERE event_time_utc <= :report_cutoff_utc
    AND resolution_state = 'resolved'
)
SELECT
  o.canonical_order_id,
  o.promised_delivery_at_utc,
  r.event_type AS latest_event,
  r.event_time_utc AS latest_event_at,
  r.source_event_id
FROM canonical_orders o
LEFT JOIN ranked r
  ON r.canonical_order_id = o.canonical_order_id AND r.rn = 1
WHERE o.promised_delivery_at_utc <= :report_cutoff_utc;
~~~

For production, the state machine must also handle cancellation, returns, attempts, late-arriving events, source corrections and explicitly conflicting records. **Latest timestamp alone does not guarantee the authoritative current status.**

### 4.8 Product experience: one record, multiple views

**For the operations analyst:** A due-orders queue with current status, last source sync, disputed links, reason for exception and direct access to the event chain. Make “needs review” a first-class state.

**For support:** Search by customer order or carrier tracking ID, show a concise current answer and disclose when the evidence is stale or contradictory.

**For an AI agent:** Supply a structured entity-resolution tool returning canonical identifiers, timestamps, status, source references and flags. The agent may explain a confirmed answer; it must not invent missing joins, suppress unresolved conflicts or reinterpret eligibility rules.

This positions unification as **context infrastructure**, not another AI answer generator. Evaluation of the agent's reasoning belongs to the separate *AI Agent Reliability* article.

### 4.9 Compare solutions and trade-offs

| Approach | Advantage | Limitation | When to choose |
| --- | --- | --- | --- |
| Manual spreadsheet crosswalk | Fast, cheap, easy to inspect | Fragile at scale, high repeated effort | Small, infrequent reconciliation |
| Existing ETL/warehouse configuration | Reuses current infrastructure | May not expose business-level conflicts clearly | Trusted mappings and mainly batch analytics |
| Canonical entity + exception workflow | Explicit joins, shared semantics and lineage | More governance, integration and maintenance cost | Frequent cross-system operational decisions |
| LLM-only record matching | Flexible interpretation of messy text | Probabilistic errors, privacy and audit concerns | Only as a reviewed suggestion tool, never sole authority |

Prefer the smallest approach that resolves the real failure mode. Do not present a custom middleware platform as inevitable.

### 4.10 What to measure

**North Star candidate:** **Weekly cross-system decisions completed using a verified unified entity view.** Count a decision only when the required identifiers, freshness and sources pass an agreed acceptance rule.

| Metric | Calculation / test | Why it matters |
| --- | --- | --- |
| Entity linkage precision | Correct accepted joins ÷ independently audited accepted joins | Avoid wrong-person/order merges |
| Entity linkage coverage | Eligible entities with resolved mappings ÷ eligible entities | How much of the workflow is actually unified |
| Verified answer rate | Cross-system questions answered correctly with required evidence ÷ eligible questions | Outcome quality |
| Time to resolution | Median human minutes from exception opened to reviewed closure | Efficiency of workflow |
| Data freshness | Age since latest accepted source update, by system and task | Fitness for use |
| Conflict detection recall | Known conflicting cases flagged ÷ seeded conflict cases | Avoid false confidence |
| Reproducibility | Test results reproduced from stored inputs and rule versions ÷ reviewed cases | Auditability |

**Guardrails:** False-merge rate, silent data loss, cross-customer leakage, unexpected access, stale “current” labels, manual review burden and operating cost. Precision should take priority over aggressive automatic joining when the decision is consequential.

### 4.11 A narrow pilot with decision rules

**Scope:** One daily delivery review, three source systems, one canonical entity, one reporting cutoff and a reviewed sample of orders covering ordinary, late, missing, duplicate and conflicting cases.

**Baseline:** Analysts answer the same cross-system questions using existing tools. Record time, accuracy, missing-evidence rate, and the number of system switches. Freeze a reviewer-approved reference answer set before running candidate workflows.

**Candidate:** Existing connectors plus documented identity mappings, canonical events, a resolution queue and a source-linked operational view.

**Evaluation:** Randomize task order across reviewers or use balanced task sets; test both workflows on comparable cases, record false joins separately from unresolved cases, and include connector failures and delayed events.

**Go forward when:** Verified answer quality reaches an agreed threshold **without** a material increase in false merges or data leakage, and reduction in recurring analyst effort justifies operating/maintenance cost.

**Stop or simplify when:** Most errors originate upstream and need process fixes; a configured existing system solves the issue; or unresolved mapping ambiguity makes automation too risky.

## 5. Final recommendation

**Unify the business entity, not merely the raw tables.** Begin with a specific decision, model the shared entity and event history, make source identity and business meaning explicit, and expose unresolved uncertainty. A reliable unification layer can later support analytics, workflow automation and agentic systems across many industries.

**Evidence boundary:** This is an independent product/technical proposal with synthetic worked records and illustrative validation methods. The numerical result is a reproducible example, not a deployed customer outcome or a measured improvement.