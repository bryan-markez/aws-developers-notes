# Relation Database Service

## RDS
Managed DB Service (Postgres, MySql, MariaDB, Oracle, SQLServer)
- automated provisioning, OS patching
- continuous backups and restore to specific timestamp (Point in Time Restore)
- monitoring dashboards
- read-replicas
- Multi-AZ setup for DR
- Maintenance windows
- Scaling Capability
- Storaged backed by EBS
- CANNOT SSH INTO RDS INSTANCES

Storage Auto Scaling  
When RDS detects that you are running out of free space, it scales automatically. You can also set a `Maximum Storage Thershold` for storage space.
Criteria for for auto scaling:  
  1) Free storage is less than 10% 
  2) Low-storage lasts at least 5 minutes
  3) 6 hours have passed since last modification

Read Replicas  
RDS can have up to 5 Read Replicas  
Replicas can be set within AZ, Cross AZ, or Cross Region (note, there is network cost of replicas are set in different AZs)  
Replication is eventually consistent  
Replicas have the option to be promoted to their own database  
Replicas have their own connection string

Multi-AZ for Disaster Recovery  
The replication is SYNCHRONOUS to the instance in the different AZ  
There is only one DNS name, so there is automatic failover on standby  
Read replicas could also be set-up for Multi-AZ for automatic failover

From Single-AZ to Multi-AZ  
Zero downtime operation  
"Modify" -> "Enable Multi-AZ"  
Internally, a snapshot is taken then a new DB is restored from that snapshot in a different AZ

At-Rest Encryption  
Both DB Master and Read Replicas can be encrypted with AWS KMS but it must be defined at launch time  
If Master is not encrypted, then the Read Replicas cannot be encrypted  
To turn a non-encrypted DB to an encrypted one, You need to create a snapshot of the DB then restore it as an encrypted DB

In-flight Encryption  
TLS-read by default

IAM Authentication  
Able to use IAM roles to connect to database without the need of username/password

Security Groups can be applied to control network access

## Amazon Aurora
Proprietary tech from AWS. Aurora is "AWS cloud optimized"
- supports Postgres and dMySQL
- storage automatically grows in increments of 10GB up to 128 TB
- can have up to 15 replicas
- replication process is faster on Aurora (sub 10ms replica lag)
- Failover in Aurora is instantaneous and it is HA native

The master Aurora Instance takes responsiblity for writing data (can have a Writer Endpoint)

Read replicas can have auto scaling and has a Reader Endpoint that handles connection load balancing

High Availability  
6 copies of your data across 3 AZ  
4 copes out of 7 needed for writes  
3 copies out of 6 needed for reads  
self healing with peer-to-peer replication  