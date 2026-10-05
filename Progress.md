Ken — Databricks DE Progress

Plan
8 weeks, ~2.5 h/day. Associate exam end of Week 6. Start applying Week 7. 
Target: Data Engineer roles, Toronto + US remote. Cloud-neutral until Week 5 
(market scan showed AWS/Azure roughly even; Databricks skills matter more than cloud).

Skills matrix (1–5)
Skill	Start	Now	Target (Wk 8)	Evidence / notes
SQL	1.5	2.5	4	Window functions, frames, join traps, reconciliation. Still relies on templates
Python	1.5	1.5	3	Not yet exercised beyond PySpark syntax
PySpark / Spark internals	0	1	3.5	groupBy/agg, joins, Window + lag; internals not started
Delta Lake	0	0	3.5	Day 3
Linux / Git / Docker	3.5	3.5	4	Git folder workflow working
Cloud (Azure + AWS)	1	1	2.5	Week 5
Unity Catalog	0	0.5	3	Knows catalog.schema.table; samples vs system catalog
Performance tuning	0	0	2.5	Has seen a join strategy in explain()
Cost / FinOps	0	0	2.5	
DevOps (DABs, CI/CD)	1	1	3.5	
Observability	0	0	2	
Streaming / CDC	0	0	2.5	
Iceberg / UniForm	0	0	1.5	
AI data engineering	0	0	1.5	

Log
Day 1 (Sep 25)
- Done: Free Edition setup, Git folder, bronze/silver/gold schemas, 
- 4 SQL drills on samples.tpch, market scan (15 postings)
- Learned: window functions, ROW_NUMBER/RANK/DENSE_RANK, QUALIFY, 
- ROWS vs RANGE frames, non-deterministic ties, three-level namespace
- Reconciled top-3-per-customer: 1,499,214 rows (UI grid had shown 30,000 — truncated)
- Lesson: never read counts off the results grid; use COUNT(*)

Day 2 (Oct 5)
- Done: LAG/LEAD, fan-out bug reproduced (~5x inflation) and fixed two ways, 
- customers-with-no-orders reconciled three ways, LEFT JOIN + WHERE trap, 
- first PySpark (Q1 + Q2 match SQL), lag in PySpark, read explain() (PhotonShuffledHashJoin)
- Learned: grain ("after this join, what is one row?"), F vs Window in PySpark, 
- withColumn, lazy evaluation (transformations vs actions)
- Quiz: 70% (missed: peer rows under RANGE; three-part namespace + system catalog)
- Scenario (dashboard 4x revenue): first answer 4/10 — no data diagnosis, no containment, 
- gatekeeping instead of automated checks. Learned structure: 
- Contain -> Diagnose -> Prove -> Fix -> Prevent -> Communicate

Weak spots (carry forward)
- Retention of facts already taught (namespace, system catalog)
- Answering every part of a question; partial submissions (2b result not reported)
- Incident answers: needs structure and automated prevention, not process gatekeeping

Open items
- [ ] Report task 2b result (top customer gap)
- [ ] Markdown headers on Day 1 notebook cells (deferred)
- [ ] Find the nation join in the 4b explain() output and note its join type

Pace check
- Day 2 completed Oct 5 (10 calendar days after Day 1). Plan assumes near-daily sessions.
- Action: fixed daily 2.5 h calendar block. Re-plan exam date if pace stays below ~5 sessions/week.

Next session
Day 3: Delta Lake fundamentals (CTAS into workspace.bronze, DESCRIBE HISTORY, 
time travel, UPDATE/DELETE, what a Delta table looks like on storage) + Python refresher
