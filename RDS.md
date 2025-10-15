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

## RDS Backups
- Trick : in a stopped RDS database, you will still pay for storage. If you plan on stopping it for a long time, you should snapshot & restore instead. The snapshot is costing you less than the stopped RDS databse. To restart the database, just use the snapshot.
  ### Automatic Backups
  - daily full backup of the database (during the window)
  - transaction logs are backed up by RDS every 5 mins
  - ability to restore to any point in time (from oldest backup to 5 mins ago)
  - 1 to 35 days of retention, set to 0 to disable automated backups
  - backups expire
  ### Manual DB Snapshots
  - manually triggered by the user
  - retention of backup for as long as you want

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
  - some read replicas can be larger than others
    - you can set up custom endpoints that point to specific read replicas
    - you do this because they are more powerful and are better for running analytical queries
    - if you decide to set up a custom endpoint, the automatic reader endpoint will be deleted so youl ljust have to use custom endpoints going forward
  
**What is backtrack?**
- restore data at any point of time without using backups

## Aurora Serverless
- automated databse instantiation and autoscaling based on actual usage
- good for infrequent intermittent or unpredictable workloads
- no capacity planning needed
- pay per second, can be more cost effective
- you dont have to provision capacity in advanced

## Global Aurora
- Aurora Cross Region Read Replicas
  - useful for disater recovery
  - simple to put in place
- aurora global database (recommended)
  - 1 primary region (read/write)
  - up to 10 secondary regions (read only), replicate lag is less than 1 second
  - up to 16 rea replicas per secondary region
  - helps for decreasing latency
  - in case of a disater you can promote another region (for disater recvoer) has an RTO (Recover Time Objective) of < 1 minute
  - typical cross-region replication takes less than 1 second for your Aurora Global Database
 
## Aurora Machine Learning
- enables you to add ML based predictions
- simple, optimzed, and secure integration btw Aurora and AWS ML services
- Supported Services:
  - Amazon SageMaker (user with an Ml model)
  - Amazon comprehend (for sentiment analysis)
- you dont need to have ML experience
- use cases: fraud detection, ads targeting, product recommendations, sentiment analysis

## Babelfish for Aurora PostgreSQL
- allows Aurora PostgreSQL to understand commands targeted for MS SQL Server
- babelfish allows the app to communicate to Aurora PostgreSQL using the T-SQL language
- so SQl Server apps can work on PostgreSQL
- Requires no to little code changes
- the same apps can be used after a migration of your database


## Aurora Backups
  ### Automated Backups
  - 1 to 35 days ( cannot be disabled)
  - point-in-time recovery in that timeframe
  ### Manual DB Sanpshots
  - Manually triggered by the user
  - retention of backup for as long as you want

## RDS & Aurora Restore Options
- Restoring a backup or snapshot creates a new database
- restoring MySQL RDS database S3
  - create a backup





























