### ** Module 12: Backup, Restore & High Availability**
**Objective:** *Master enterprise backup/restore, disaster recovery, and high availability.*  
**Topics:**  
- **Backup Types:** FULL, DIFFERENTIAL, LOG, COPY_ONLY, FILEGROUP, TAIL_LOG, CHEKSUM ENCRYPTION, Geo-Replication, Azure Blob Storage  
- **Restore Types:** FULL, DIFFERENTIAL, LOG, STANDBY, PIECEMEAL, PAGE, WITH RECOVERY/NORECOVERY/STANDBY, restoring system databases  
- **Recovery Models:** FULL, BULK_LOGGED, SIMPLE, switching between  
- **High Availability:** Always On Availability Groups (AG), Failover Cluster Instance (FCI), Log Shipping, Database Mirroring (legacy), Distributed AG, Geo-Replication  
- **Monitoring:** msdb.dbo.backupset, msdb.dbo.restorehistory, sys.dm_db_index_usage_stats, sys.dm_db_log_stats, LAST BACKUP  
- **Disaster Recovery:** Recovery Time Objective (RTO), Recovery Point Objective (RPO), AG listeners, manual/auto failover, DR drill, backup validation  
**Practice Lab:** Tail-log backup, point-in-time restore, AG configuration, failover test, restore to STANDBY
