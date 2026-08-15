## EBS for EC2 

EBS == Elastic Block Store (volume network drive)
"Network USB Stick"

Locked to one AZ, if you need to move a EBS Volume you'll need to snapshot it and then move it along. 

Your EBS needs to exist in the same AZ as the EC2 you want to attach it too. 

Can have more then one volume connected to EC2 instance. 

Delete on Termination Attribute
Check box, when a EC2 instance is being terminated, if check will be removed on termination. 

## EBS Snapshots
Just like VMWare takes a snapshot of the drive. 
Not required to detached EBS vol but recommended

This is how you transfer one AZ to an other. 

### EBS Snapshots Features
EBS Snapshot Archive
75% cheaper, takes 25-72 hours for restoring a lost archive

Recycle Bin for EBS Snapshots
Set Rules to retain deleted snapshots, anywhere from 1 day to 1 year. 

Fast Snapshot restore
FOrce full init of snapshiot to have no latency on first used. COSTS A TON, FYI. 

## AMI Overview
Amazon Machine Image
AMI is a customization of an EC2 instance.
Preinstalled with tools so it'll boot faster. 
AMIis built for a specific region but you can copy across regions. 

Public AMI
Make your own AMI (Could make a Phishing AMI for us)
AWS Marketplace AMI (someone else made it for you, maybe sells it to you)

Start EC2, customize it. Stop instance, build AMI (This snapshots the AMI), launch instances from other AMIs

## EC2 Instance Store
EBS volumes are network drives but limited performance

EC2 Instance store is a hardware disk, for high performance. Better I/O performance (some of these are insane like 3Mill+ IOPS). If the EC2 is stopped the storage is gone gone. 

Use this as a bugger, cache, scratch data, temp data. 
Risk of data lose if hardware fails
Backup and replicate if nessecary. 

EBS Volumes == 6 types
gp2/gp3 == general purpose
io1/io2 == highest performance SSDs (But if the EC2 is stopped all data is lost)
ST1 == HDD low cost HDD (spinning disk), this is accessed frequently//high-throughput workloads
sc1 == HDD lowest cost HDD volume, design for less frequently access workloads (data graveyard?)

All EBS's can be defined via size, throughput, IOPS
HOT TIP: only gp2/gp3 and io1/io2 can be used as boot volumes

General Purpose
Boot vol, Virt Desk, Dev and test envs
1GB - 16 TB
**gp3** - baseline is 3,000 IOPS, 125 Mib/s
    - can go up to 16k IOPS and 1k MiBs
    - You can independently set the IOPS regaurdless of the size
**gp2** - can burst to 3k IOPS
Size and IOPS are linked, max IOPS is 16k. 
3 IOPS per gig, so 5334GB drive maxs the IOPS at 16k.  

Provisioned IOPS (PIOPS) SSD
Crit business apps that have sustained IOPS performance
OR apps that require more than 16k IOPS
Great for database workloads.

**io1** - 4GB to 16TB
    - max PIOPS 64k for Nitro EC2, 32k for others
    - Can increast IOPS independently for storage size

**io2** - 4GB to 64TB
    - SUB-MILLISEC Latency
    - MAX IOPS 256k with a IOPS:GB ratio of 1000:1 
    - Supports EBS Multi-attach

**ST1 and SC1** 
    - Can't be a boot vol
    125GB to 16 TB

    ST1 Throughput Optimized 
    Big Data, Data Warehouses, Log Processing
    Max Throughput 500 MiBs  - max IOPS 500

    COLD HDD SC1
    Fpr data tjat is infrequently accessed
    Lowest cost possible
    

## EBS Multiattach 
Attach the same EBS Vol to multiple EC2 instances in the same AZ
Each instance has full read and write permissions to the high performance volume
Best Use:
    - Achieve higher application availability in clusted Linux Apps (Teradata)
    - Applications must manage concurrerent write ops
16 Ec2 Instances Max
File system that is cluster-aware (No XFS, EXT4)

## EBS Encryption
When Encrypted EBS are made
    - Data at rest == encrypted
    - All date in motion between the vol and instance == encrypted
    - Snapshots == encrypted
    All vols created from the snapshot are encrypted

Encryption and decryption are handled for you
Min impact on latency
EBS encryptions uses AES-256
Copying unencrypted Snapshop allows you to encrypt it
Snapshots of encrypted vols are also encrypted

How do encrypted unecrypted vol
Create a snapshot
Encrypt snapshot
Create new EBS volume from Snapshot
Now attached the encrypted volume to original source

## EFS Elastic File System
Network FIle System - Like a network share.
Highly Available, Scales, but $$$ cost 3X compared to gp2, pay per use as well
EFS can connect to multiple EC2 instances in multiple-AZs
Can all connect at the same time

Best Use: Content management, web serving, data sharing, wordpress

Uses NFSv4.1
Need to have a Sec Group to control access to EFS
ONLY WORKS WITH LINUX based AMIs (**NO WINDOWS**)
Encryption at rest using KMS
POSIX file System (what Linux uses) 
It scales with you no need to plan ahead

EFS Scale:
1000 concurrent NFS clients 10GB throughput
Can grow to Petabyte network Share

Has a performance mmode (This is set at creation)
    - General Purpose (this is the default)
    - MAX IO, big data, media processing

Throughput Mode
    - Bursting 1 TB = 50 MB/s + burst up to 100 MB/s
    - Provisioned - set throughput reguardless of storage size
    - Elastic - Automatically scales throughput up and down based on the workloads
    - Up to 3GB/s for reads 1GB/s for writes
    - used for unpredictectable workloads

### EFS Storage Classes
Standard Tier - touched a lot
Infrequent Access - touched less, costs to pull files, lower price to store files
Archive Tier - few times a year 50% cheaper
Implement lifecyle polcies to move files in between tiers

### Ava and Durability
Standard - Prod, Multi-AZ, great disaster recovery
One Zone: One AZ, Dev work, back up enabled by default, compatiable with Infrequent Access (EFS One Zone-IA)

Can get up to 90% in cost savings. 
