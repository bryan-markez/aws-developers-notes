# Relation Database System

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