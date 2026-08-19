## RDS Aurora ElasticCache
"Databases"

Relational Database Service (SQL)
    - Postgres
    - MySQL
    - MariaDB
    - Oracle
    - Microsoft SQL Server
    - IBM DB2
    - Aurora (AWS Proprietary Database)

Why use RDS instead of database on EC2
    - Automated provisioning, OS patching
    - Continous Backups (POint of time restore)
    - Monitoring Dashboards
    - Read Replicas
    - Multi AZ Setup (Disaster Recovery)
    - Maintence Windows
    - Scaling (Vert and Hort)
    - Storage backed by EBS

YOU CAN'T SSH into the service, its a service not a host. 

### RDS Storage Auto Scaling 
Increase storage of your RDS dynamically//automatically. 
Set a max storage threshold (max limit for DB Storage, this will help control your costs spinning out of control)

Auto Mode If:
    - Free Storage is less then 10% of allocated storage
    - Low-storage lasts == 5 minutes
    - 6 Hours Passed since last write

Use for apps that have unpredictable workloads
Supports all RDS database engines (See list above)

## RDS Read Replicas for Read Scalability 
(RDS Read Replicas vs Multi AZ)
Scales Reads. Main database gets overwelmed trying to handle all request. 

You make up to 15 read replicas
    - Within AZ
    - Cross AZ
    - Cross Region

Replication is ASYNC, makes reads all the same//consistent

Replicas *can* be made into their own DB

Applications must update the connection string to read replicas

So if a report app wants access to your database, you make a read replica so the primary database//Application doesn't get overwelmed. 

Read Replicas only have access to SELECT (its the read function), no access to INSERT, UPDATE, DELETE (this changes data)

COSTS:
AZ to AZ in same region == FREE

AZ to AZ in differnt region == Not free

### RDS Multi AZ
This is mostly for diaster recovery
SYNC Replication

You get one DNS name to point your app towards 
THE RDS Master makes a Sync Replication to the RDS DB Instance Standby DB in a different zone. 

You can have Read Replicas be setup as Multi AZ for DR purposes.

How do you go from Single AZ to Multi?
Zero Downtime OP (No need to stop)
MOdify Database and click the Multi-AZ

## RDS Custom 
Managed ORacle and Microsoft SQL Server Databaes with OS and database customization
Gives you all the RDS goodies - Auto Setup, operation, scaling etc
But you now have control over the database and OS
- Config settings
- Install patches yourself
- Enable Native Features
- Access underlying EC2 Instances using SSH

IF you customize anything, de-activate automation mode, take a snapshot of the DB before chaning anything.

## Amazon Aurora
Compatialbe with PostGres (3x) and MySQL (5X)
Cloud Optimized  
Start at 10GB up to 256 TB
15 Read Applications, replication faster than MySQL (sub 10ms lag)
Failoer in Auroa is instant. 
20% more cost then compared to RDS, but wayyyyyy more efficent

### HIGH Availability
6 copies of your data across 3 AZ

4/6 needed for writes
3/6 for reads
Self healing with peer to peer replication
Storage is stripped across 100s of volumes

Aurora has one Master INstance that takes writes
Auto Fail Over less than 30 seconds (FAST)

You can have a Master and upto 15 Aurora Read Replicas

Supports Cross Region Replication 

### Aurora DB Cluster

Shared Storage Volume 

Theres a writer endpoint that always points to the master (or current master)

Theres a reader Endpoint, that serves as a connection to the load balancing of the read replicas (for autoscaling)

Load balancing happens at the connection level not the statement level. 
Has something called backtrack to restore data to any point of time without a dedicated backup

# Aurora - ADVANCED

Replica Auto-Scaling
You have your master Aurora DB for writing to the Shared Storage Vol. And you many have a few Reader Enpoints, if those start saying we're to busy, a new end point group with more Read will scale

## Custom EndPoints
You can dedicate some of the read hosts to a custom endpoint, so lets say your larger read machines can be in a custom endpoint to point at. 

So you might have heavy analytical queries, that you could point towards the beefier machines. 

## Serverless
Good for low or random workloads, no capacity planning, pay per second (Can be cost effective)

Proxy Fleet auto scales and load balances the servers

## Global AUrora 
Cross Region Reads
Mostly for Disaster Recovery

Aurora Global Database
1 Primary Region for read and write
Upto 10 secondary (read only regions)
Up to 16 Read Replicas PER SECONDARY REGION
Takes less than 1 second to replicate data cross-region

## Machine Learning
Lets you use ML-base predictions
Intergration between Aurora and AWS ML services
Supported Services
    - Sage Maker (Use any model you want)
    - Amazon Comprehend (sentiment analysis)

The big usecases for this are fruad detection, targeted ads, sentiment analysis, product recommendations

## Babelfish for Aurora PostgreSQL
Allows Aurora PostgreSQL to understand commands for a MS SQL server. Cross play I guess is the short of it. 

This can help you avoid doing migrations for the applications to communicate. 

## RDS Backup
Automatic Backup
Daily Full Backups
Transaction Logs are backed up every 5 minutes
1-35 days of retention, 0 for disabled

You can also do manual DB snapshots
Trigger by User
You can keeps this as long as you want

ProTIP: If you have a DB that isn't going to be used for a long time, take a snapshot and then restore. Otherwise your paying for the DB even in a stopped state. 

## Aurora Backups
Automatic Backups
1 to 35 Days (NOT ABLE TO DISABLE)
POint in Time Recovery in that time slot

Can manually trigger a snapshot and keep them as long as you want. 

## Restore Options

Can restore a RDS backup or snapshot creates a new database

IF your restoring MYSQL RDS from S3
Create a backup of your on-prem database
Store in S3
Restore the backup file into a new RDS running MySQL

If restoring MySQL Aurora cluster from S3
create a backup of your on-prem database USING Percona XtraBackup
Store in S3
Restore the backup File onto a new Aurora Cluster with MYSQL

## Aurora Database Cloning 
Create a new Aurora DB cluster from an Existing one
Faster than snapshot and restore

Uses Copy On Write Protocol 
New DB Cluster uses same data volume as OG Db cluster
When changes are made to the new DB Cluster, then additional storage is allocatedl and data is copied to be seperated

You want to use this for creating a staging database from the production database.

## RDS Security
At Rest Encryption
    - Database and replicas using AWS KMS (Key Managment System)
    If the master was not encrypted the replicas can't be encrypted
    IF you want to do this, you do the same thing like you would with an EBS. Take a snapshot, and then restore as encrypted. 

In Flight encryption, TLS by default, uses AWS TLS root certs

IAM Auth, IAM Roles (ec2 instances have access to DB)
Security Groups: Netowrk controlls
No SSH, unless you use RDS CUstom

Audit logs can be enabled and sent to CloudWatch for longer retention

RDS Proxy 
So lets say you have 1000 LAMBDA functions, and lets say effectively 400 of them are asking the same thing. 

Instead of letting all 1000 connection smack the RDS at once the Proxy will pool sim requests to gether. SO those 400 connections or requests now become 1, so now you have 601 requests not 1000 hitting your RDS DB.

## Amazon ElastiCache

ElastiCache is managed Redis or Memcached

In memory databases with super high performance
Makes the app stateless
AWS takes care of all the OS mait,patch,opts,setup, etc

The idea is any requests that are asked over and over again get stored in this cache, so you will have to rewrite the application code to point at the ElastiCache first. 

## ElasticCache DB 
See above, 
If quiery exists in Cache, done, if not go to RDS DB
Then write to cache

But you need a way to invalidate data.

## Redis vs MemCached

Redis
    - Multi AZ with Auto-failover
    - Red Replicas to scale reads
    - Data Durability using AOF persistence
    - Backup and restore
    - Supports Sets and Sorted Sets

    (IE. One REDIS replicates to an other)

MEMCACHED
    - Multi-node for partitioning of data (sharding)
    - No High Ava
    - Not Persistent
    - Backup and restore (Serverless)
    - Multithreaded

    (IE. This shares chucks of the DB with friends - Sharding)

    Got 24/26 questions right. Yay!