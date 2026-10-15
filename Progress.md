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
| SQL                      | 1.5   | 2.5 | 4   | Window functions, frames, join traps, reconciliation |
| Python                   | 1.5   | 2   | 3   | table_summary(): dict return, f-strings, try/except |
| PySpark / Spark internals| 0     | 1.5 | 3.5 | Partitions, repartition, exchanges, broadcast vs shuffled join. Lazy evaluation explained wrongly in 4a |
| Delta Lake               | 0     | 2   | 3.5 | DML, RESTORE, schema enforcement, VACUUM; read _delta_log incl. deletion vectors |
| Linux / Git / Docker     | 3.5   | 3.5 | 4   | Git folder workflow working |
| Cloud (Azure + AWS)      | 1     | 1   | 2.5 | Plan Week 5 |
| Unity Catalog            | 0     | 1   | 3   | Namespace, system catalog, volumes |
| Performance tuning       | 0     | 1   | 2.5 | Query profile (time, rows, hottest operator); skew via repartition by key; can't yet label exchanges unaided |
| Cost / FinOps            | 0     | 0   | 2.5 | |
| DevOps (DABs, CI/CD)     | 1     | 1   | 3.5 | |
| Observability            | 0     | 0   | 2   | |
| Streaming / CDC          | 0     | 0   | 2.5 | |
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

### Day 4 (Oct 14)
- Lazy evaluation timed: transformations 0.001s, action 1.686s
- Partitions: as read 3 (2 x 3,387,392 + 725,216 = 7.5M); repartition(8) = 8 x 937,500;
  repartition(8, o_orderstatus) = 3 non-empty, largest 3,654,501 / smallest 191,189
  (matches Day 1 status counts exactly) — skew from a low-cardinality key
- explain(): narrow plan has 0 exchanges; top_nations has 5 (2 join, 1 broadcast, groupBy, orderBy);
  nation join = BroadcastHashJoin
- Query profile (fan-out Fix A query): 21.4 s, 38,975,036 rows read across ALL tables;
  hottest operators = lineitem scan (30M rows) and its shuffle
- Quiz 92% (best so far; RESTORE answer mixed up history entries with data files)
- Scenario (collect() on 500M rows): 5/10 — right diagnosis, no code fix, no teammate message
- Hit DELTA_UNSUPPORTED_TIME_TRAVEL_BEYOND_DELETED_FILE_RETENTION_DURATION in practice
- Submission: complete, used the checklist template

## Weak spots (carry forward)
- Explaining *why* (4a: attributed laziness to narrow transformations; 4c: "reads fewer columns" for no-shuffle)
- Attributing metrics to the right object (profile "rows read" is the whole query, not orders)
- Scenario answers: include the concrete fix (code) and the people step (communication)

## Open items
- [ ] 74-row difference in orders_1998: use DESCRIBE HISTORY operationMetrics (time travel now blocked)
- [ ] Day 4B: lazy-evaluation demo, collect() fix, EXPLAIN on Fix A vs Fix B
- [ ] Markdown headers on Day 1 notebook cells (deferred)

## Next session
Day 4B (~45 min), then the Week 1 checkpoint (closed-book, scored /100; need 70% to advance)
