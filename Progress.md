# Ken — Databricks DE Progress

## Plan
Depth-first, ~2.5 h/session. No fixed calendar dates (changed Oct 11 at Ken's request):
progress is measured in completed plan days, not weeks.
Minimum session on busy days: 45 min (quiz + one task) so the chain doesn't break.
Milestones: Associate exam after plan Day ~30; start applying once capstone v1 is live.
Target: Data Engineer roles, Toronto + US remote. Cloud-neutral until plan Week 5
(market scan showed AWS/Azure roughly even; Databricks skills matter more than cloud).

## Skills matrix (1–5)
| Skill                    | Start | Now | Target | Evidence / notes |
|--------------------------|-------|-----|--------|------------------|
| SQL                      | 1.5   | 3   | 4   | Checkpoint B 22/25: per-customer grain before ranking, tiebreakers, ROWS vs RANGE, fan-out-free report, 3 reconciliations |
| Python                   | 1.5   | 2   | 3   | table_summary(): dict return, f-strings, try/except |
| PySpark / Spark internals| 0     | 2   | 3.5 | Checkpoint C1 10/10 (groupBy, Window lag, lazy evaluation). Plan reading: labels exchanges; AQE/skew "why" still thin |
| Delta Lake               | 0     | 2.5 | 3.5 | Checkpoint C2 8/10: history, prove-before-fix with VERSION AS OF, RESTORE + reconcile |
| Linux / Git / Docker     | 3.5   | 3.5 | 4   | Git folder workflow working |
| Cloud (Azure + AWS)      | 1     | 1   | 2.5 | Plan Week 5 |
| Unity Catalog            | 0     | 1   | 3   | Namespace, system catalog, volumes |
| Performance tuning       | 0     | 1.5 | 2.5 | Query profile, measured Fix A vs Fix B, exchange labeling. Gap: AQE broadcast switch, why low-cardinality keys aren't skew after partial agg |
| Cost / FinOps            | 0     | 0   | 2.5 | |
| DevOps (DABs, CI/CD)     | 1     | 1   | 3.5 | |
| Observability            | 0     | 0   | 2   | |
| Streaming / CDC          | 0     | 0   | 2.5 | Starts Day 5 (Auto Loader) |
| Iceberg / UniForm        | 0     | 0   | 1.5 | Knows they are open table formats like Delta |
| AI data engineering      | 0     | 0   | 1.5 | |

## Log
### Day 1 (Sep 25)
- Free Edition, Git folder, medallion schemas, window-function drills, market scan
- Reconciled top-3-per-customer: 1,499,214 rows (grid showed 30,000 — truncated)

### Day 2 (Oct 5)
- LAG/LEAD, fan-out bug (~5x) fixed two ways, anti-join reconciled, LEFT JOIN trap,
  first PySpark, explain(). Quiz 70%. Scenario 4/10

### Day 3 (Oct 11 + Oct 13)
- Delta: CTAS, DML, RESTORE, schema enforcement + mergeSchema, table_summary(),
  Parquet vs Delta on a volume. Quiz ~60%. Scenario 6.5/10
- _delta_log read: UPDATE on a 46,877-row file used a deletion vector (5 rows hidden)
  plus a new 3 KB file, instead of rewriting 1.8 MB. Min/max stats let Delta skip 2 of 3 files.

### Day 4 (Oct 14) + 4B (Oct 16)
- Lazy evaluation timed: transformations 0.001s, action 1.686s; divide-by-zero demo explained correctly
- Partitions: repartition(8, o_orderstatus) = 3 non-empty (skew from a low-cardinality key)
- explain(): labeled exchanges; nation join = BroadcastHashJoin
- Fix A 11.8 s / 30M shuffled vs Fix B 2.7 s / 7.5M — aggregate before the shuffle
- Quiz 92%. Scenario (collect() on 500M rows): 5/10

### Week 1 checkpoint (started Oct 21, finished Nov 10) — 83/100, PASSED
| Section | Score | Notes |
|---|---|---|
| A Concepts | 16/20 | Missed A4 (default frame = RANGE, peers share a value) and A9 (mergeSchema, not overwrite) |
| B SQL | 22/25 | B1 7.5 (no COUNT(*) shown), B2 7.5 (two windows missing PARTITION BY), B3 7 (order reconciliation not independent) |
| C PySpark + Delta | 18/20 | C1 10/10. C2: misidentified the bad version (OPTIMIZE vs UPDATE); compared decimal to '0' string |
| D Plan reading | 11.5/15 | D1, D2 full marks. D5 and D6 gave the "what" without the "why" |
| E Scenario | 15.5/20 | Strong hypothesis, dedupe proof, MERGE as proper fix, good teammate message. Missed CFO message, a blocking check, bronze comparison |

## Weak spots (carry forward)
- Explaining *why*: still the main gap (checkpoint D5, D6)
- Default window frame (RANGE, peer rows): missed in two quizzes, though done correctly in practice (B2)
- Reading DESCRIBE HISTORY precisely: name the operation that changed the data, not the latest version
- Incidents: communicate to the business owner, not only the engineer; checks must block, not just alert
- Gaps between sessions: checkpoint took 3 weeks end to end; retention risk

## Open items
- [ ] 74-row difference in orders_1998: use DESCRIBE HISTORY operationMetrics (time travel now blocked)
- [ ] Markdown headers on Day 1 notebook cells (deferred)

## Next session
Day 5: Auto Loader into bronze (simulated file feed on a volume, checkpoint idempotency,
schema drift), plus a short catch-up quiz on the checkpoint misses
