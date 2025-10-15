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
