# Ken — Databricks DE Progress

## Plan
Depth-first, ~2.5 h/session. No fixed calendar dates (changed Oct 11 at Ken's request):
progress is measured in completed plan days, not weeks.
Milestones: Associate exam after plan Day ~30; start applying once capstone v1 is live.
Target: Data Engineer roles, Toronto + US remote. Cloud-neutral until plan Week 5
(market scan showed AWS/Azure roughly even; Databricks skills matter more than cloud).

## Skills matrix (1–5)
| Skill                    | Start | Now | Target | Evidence / notes |
|--------------------------|-------|-----|--------|------------------|
| SQL                      | 1.5   | 2.5 | 4   | Window functions, frames, join traps, reconciliation. Join-filter placement not yet retained |
| Python                   | 1.5   | 1.5 | 3   | try/except utility pending (Day 3 task 4c) |
| PySpark / Spark internals| 0     | 1   | 3.5 | groupBy/agg, joins, Window + lag. Transformations vs actions not retained |
| Delta Lake               | 0     | 1.5 | 3.5 | CTAS, DESCRIBE DETAIL/HISTORY, UPDATE/DELETE/INSERT, RESTORE, mergeSchema, VACUUM trade-off |
| Linux / Git / Docker     | 3.5   | 3.5 | 4   | Git folder workflow working |
| Cloud (Azure + AWS)      | 1     | 1   | 2.5 | Plan Week 5 |
| Unity Catalog            | 0     | 0.5 | 3   | catalog.schema.table; samples vs system catalog (now retained) |
| Performance tuning       | 0     | 0   | 2.5 | Has seen a join strategy in explain() |
| Cost / FinOps            | 0     | 0   | 2.5 | |
| DevOps (DABs, CI/CD)     | 1     | 1   | 3.5 | |
| Observability            | 0     | 0   | 2   | |
| Streaming / CDC          | 0     | 0   | 2.5 | |
| Iceberg / UniForm        | 0     | 0   | 1.5 | Knows they are open table formats like Delta |
| AI data engineering      | 0     | 0   | 1.5 | |

## Log
### Day 1 (Sep 25)
- Done: Free Edition setup, Git folder, bronze/silver/gold schemas,
  4 SQL drills on samples.tpch, market scan (15 postings)
- Learned: window functions, ROW_NUMBER/RANK/DENSE_RANK, QUALIFY,
  ROWS vs RANGE frames, non-deterministic ties, three-level namespace
- Reconciled top-3-per-customer: 1,499,214 rows (UI grid had shown 30,000 — truncated)
- Lesson: never read counts off the results grid; use COUNT(*)

### Day 2 (Oct 5)
- Done: LAG/LEAD, fan-out bug reproduced (~5x inflation) and fixed two ways,
  customers-with-no-orders reconciled three ways, LEFT JOIN + WHERE trap,
  first PySpark (Q1 + Q2 match SQL), lag in PySpark, read explain() (PhotonShuffledHashJoin)
- 2b: longest gap between orders = 2,343 days (custkey 23306) — plausible for 1992–1998 data
- Learned: grain, F vs Window, withColumn, lazy evaluation
- Quiz: 70%. Scenario: 4/10

### Day 3 (Oct 11) — partially complete
- Done: orders_1998 CTAS (668,101 rows; Delta, 3 files, ~14.9 MB);
  UPDATE 97,175 / DELETE 133,941 / INSERT 3,094 rows (all plausible);
  accidental DELETE recovered with RESTORE to v3; mergeSchema (old rows = NULL)
- Learned: Delta = Parquet + transaction log; RESTORE adds a version (audit trail kept);
  VACUUM ends time travel to removed files; retention = cost vs recovery window
- Quiz: ~60% (missed: LEFT JOIN filter placement, transformations vs actions; partial on F)
- Scenario (silver.customers -40%): 6.5/10, up from 4/10. Structure now used correctly.
  Gaps: restored before proving; reran without a root cause; check didn't block publication
- Concept gaps closed by explanation: Parquet/file formats, where Delta sits in the stack,
  medallion architecture

## Weak spots (carry forward)
- Partial submissions: 4th session in a row with items missing (Day 3: 3d count, 4a, 4c)
- Retention: LEFT JOIN filter placement (taught Day 2, missed Day 3 quiz); actions vs transformations
- Incidents: prove with data before changing anything; find root cause before rerunning

## Open items
- [ ] 3d: reconciled count after RESTORE (expected 537,254)
- [ ] 4a: error class + explanation
- [ ] 4c: table_summary with try/except, output
- [ ] Day 3B: Parquet vs Delta on a volume
- [ ] Markdown headers on Day 1 notebook cells (deferred)

## Next session
Day 3B wrap-up, then Day 4: Spark architecture (driver, executors, partitions,
jobs/stages/tasks, shuffles, actions vs transformations) + reading explain() and the query profile
