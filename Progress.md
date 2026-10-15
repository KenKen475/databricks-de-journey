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
| SQL                      | 1.5   | 2.5 | 4   | Window functions, frames, join traps, reconciliation. Join-filter placement not yet retained |
| Python                   | 1.5   | 2   | 3   | table_summary(): dict return, f-strings, try/except that keeps the loop running |
| PySpark / Spark internals| 0     | 1   | 3.5 | groupBy/agg, joins, Window + lag. Architecture is Day 4 |
| Delta Lake               | 0     | 2   | 3.5 | CTAS, DETAIL/HISTORY, DML, RESTORE, schema enforcement + mergeSchema, VACUUM, Parquet vs Delta on a volume |
| Linux / Git / Docker     | 3.5   | 3.5 | 4   | Git folder workflow working |
| Cloud (Azure + AWS)      | 1     | 1   | 2.5 | Plan Week 5 |
| Unity Catalog            | 0     | 1   | 3   | Namespace, system catalog, volumes; saw UC block UPDATE on a path-based Parquet table |
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

### Day 3 (Oct 11 + Oct 13)
- Done: orders_1998 CTAS (668,101 rows; Delta, 3 files, ~14.9 MB);
  UPDATE 97,175 / DELETE 133,941 / INSERT 3,094 rows;
  accidental DELETE recovered with RESTORE to v3; schema enforcement error
  (DELTA_METADATA_MISMATCH) then mergeSchema (old rows = NULL);
  table_summary() with try/except; Parquet vs Delta on a volume
  (UPDATE on Parquet rejected: UC_COMMAND_NOT_SUPPORTED)
- Learned: Delta = Parquet + transaction log; RESTORE adds a version (audit trail kept);
  VACUUM ends time travel to removed files; columnar formats; medallion layers
- Quiz: ~60%. Scenario (silver.customers -40%): 6.5/10, up from 4/10
- **Unresolved:** post-restore count was 537,328, expected 537,254 (74 rows off).
  Reported as "got the answer" without comparing to the expected value.

## Weak spots (carry forward)
- Comparing actual vs expected: 3d declared done with a 74-row mismatch
- Retention: LEFT JOIN filter placement (taught Day 2, missed Day 3 quiz); actions vs transformations
- Incidents: prove with data before changing anything; find root cause before rerunning
- Submissions: much better on Oct 13 (all items sent). Still paste outputs, since Git doesn't store them

## Open items
- [ ] Explain the 74-row difference in orders_1998 (version-by-version counts)
- [ ] 3B Q3: read add/remove actions in _delta_log for the UPDATE
- [ ] Paste table_summary() output
- [ ] Markdown headers on Day 1 notebook cells (deferred)

## Next session
Day 4: Spark architecture (driver, executors, partitions, jobs/stages/tasks,
narrow vs wide transformations, shuffles) + reading explain() and the query profile
