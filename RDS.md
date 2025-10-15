# RDS (Relational Database Service)
- managed DB service for databases use SQL
- allows you to create databases in the cloud that are managed by AWS
  - posgresm MySQL MariaDB ORacle Microsoft SQL Server IBM DB2 Aurora (AWS Proprietary Database)
**Why use RDS over deploying the database on an EC2 instance?**
  - RDS is a managed service
    - automating provisioning, OS patching
    - continuous backups and can restore to a specfic timestamp
    - monitoring dashboards
    - read replicas for improved read performance
    - multi AZ setup
    - maintenence windows for upgrades
    - scaling capability (vertical and horixontal)
    - storage backed by EBS
    - cannot SSH into instances
## Storage Auto Scaling
- helps you increate sotrage on your DB instance dynamically
- when RDS detects you are runing out of free database storage, it scales automatically. You can Avoid scaling manually
- you have to set a max storage threshold
- automatically modidy storage if
  - free storage is less than 10% of allocated storage
  - low-storage lasts at least 5 minutes
  - 6 hours have passed since last modification
- useful for unpredicatable workloads
- supports all RDS database engines

## RDS Read Replicas
- helps to scale your reads
- you can have up to 15 read replicas
- within AZ, cross AZ or Cross Region
- Replication is Async so you may read data that is not updated yet
- replicas can be updated to their own database
- apps must update the connection string to leverage read replicas
  ### Network Cost
  - there is a network cost when data goes from one AZ to another in another region 
  - for RDS read replicas within the same region, you dont pay a fee

## RDS Multi AZ
- SYNC Replication
- one DNS name - if something happens to the master there is an auto app failover to the standby(replicated RDS instance)
- increase avalability
- failover in case of loss of AZ, loss of network
- no manual intervention in apps
- not used for scaling
- no one can read or write to it, just a standby
- you can set up a read replica as multi AZ for disaster recovery
********
**How can you transition a single AZ DB to a multi DB AZ?**
- zero downtime operation (you dont need to stop the DB)
- just click modify in the DB and click on database. Very easy.
- The follwoing happens
  - snapshot taken
  - the DB is restored from the snapshot inthe new AZ
  - sync is stablished between the two databases
 
## RDS Custom
- managed Oracle and Microsft SQL Server Database with OS and database customization (only these 2 databases)
- access to underlying database and OS so you can:
  - configure setting
  - install patches
  - enable native featues
  - access the underlying EC2 instance using SSh or SSM Session Manager
- you should deactivate the automation mode to perform the customixation, take a DB snapshot just in case you break something with the customizations
- full acces to the underlying OS and database, thats the differene between RDS and RDS Custom


# Aurora
- proprietary tech from SWA (not open sourced)
- postgres and MySQL are both supported as Aurora DB (means your drivvers will work as if Aurora was a postgres or MySQL DB
- Aurora is AWS cloud optimized and claims 5x performationce improvement over MySQL on RDS, over 3x the performance of postgres on RDS
- Aurora storage auto grows in increments of 10gb up to 128 TB
- Aurora can have up to 15 read replicas. The replication process is faster than MySQL
- failover in Aurora is instantaneous
- Aurora costs more than RDS (20%) but its more efficient

## High Availability and Read Scaling
- stores 6 copies of your data accross 3 AZs
  - only needs 4 copies out of 6 for writes
  - onlye needs 3 out of 6 needed for reads
  - self healing with peer to peer replication
  - storage is striped accross 100s of volumes
- one aurora instance takes writes (master). Master is the only one that writes to your storage
- automated failover for master (less than 30 seconds)
- master plus up to 15 read replicas serve reads
- supports cross region replications

## Aurora DB Cluster
- Aurora provides a "write endpoint" that always points to the master even in failover. the clients only point to the endpoint
- Reader Endpoint: there can be up to 15 read replicas which can get confusing for the system to keep track of all of them so they all point to a reader endpoint. All the clients point to the reader endpoint and load balancing happens to balance which clients go to which read replica. The load balancing happens at the connection level not the statement level
- The read replicas are auto scaling (horizontal up to 15)
  
**What is backtrack?**
- restore data at any point of time without using backups




























