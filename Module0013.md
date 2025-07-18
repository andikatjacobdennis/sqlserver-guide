### ** Module 13: Performance Tuning & Monitoring**
**Objective:** *Optimize SQL Server performance and establish proactive monitoring.*  
**Topics:**  
- **Indexing:** Index selection, filling factor, defragmentation, statistics, missing indexes, filtered/indexed views  
- **Query Optimization:** Execution plans (actual/estimated), hints (MAXDOP, OPTIMIZE FOR, FAST), parameter sniffing (RECOMPILE, KEEPFIXED PLAN, LOCAL variable trick), tempdb usage/tuning, Resource Governor  
- **Statistics:** AUTO_UPDATE_STATISTICS, UPDATE STATISTICS WITH SAMPLE, histogram, cardinality estimation, incremental stats  
- **Monitoring Tools:** DMVs (sys.dm_exec_requests, sys.dm_os_wait_stats), XE (Extended Events), Profiler (legacy), Query Store (capture, analyze, force plans), PerfMon  
- **Community Scripts:** sp_WhoIsActive, sp_BlitzFirst, sp_BlitzCache, Ola Hallengren maintenance  
- **Automation:** Policy-Based Management (PBM), Central Management Server (CMS), SQL Server Agent (jobs, alerts, operators)  
- **Predictive Maintenance:** Machine learning via sp_execute_external_script (Python/R), Azure ML integration  
**Practice Lab:** Index rebuild/reorganize, query store setup, DMV analysis, Resource Governor configuration
