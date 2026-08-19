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
