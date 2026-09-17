# Snowball AWS Offering

Highly Secure portable device 

Helps migrate PETAbytes of data

Two flavors to go with: 

Snowball Edge STORAGE Optimized - 210 TB

Snowball Edge COMPUTE optimized - 28 TB

## Data Migrations
So uploading huge amounts of data via the internet could take years. 

So AWS sends you a device, where ideally it'll be faster, and then you ship the device back to AWS. Then you start the import/export process to a s3 bucket. 

## Edge Computing
This allowd data to processed while at an edge location. (outside data centers like a boat, truck, etc)

Snowball Edge device acts as the compute for this. Run Ec2 Instances or Lambda Functions at the edge. 

Use Case: Preprocess Data, Machine Learning (ML), transcoding media. 

## Snowball into Glacier
No direct way to do this, so you gotta go through S3 first, then use S3 Lifecycle policy to transfer it into Glacier. 

## Amazon FSx 
third party file systems on AWS
Luster, Windows File Server, OpenZFS, NetApp

## Windows File Server
Fully managed windows file share
SMB Protocol
Microsoft AD, ACLs, User Quotas 
Linux can mount it too (Samba)
You can use DFS namespaces as well. 

Scales up to 10s of GB/s
Storage Options
SSD (Fast)
HDD (Slower, cheaper, etc)

Can be accessed on-prem with a VPN or Direct Connect
Can be configured for Multi-AZ
Data is backup daily to S3

## Lustre
Linux + Cluster
Parallel distributed file system, large scale computing
Machine Learning, or High Performance Computing
MASSIVE Scale
SSD
HDD

Seamless integration with S3. 

"Can read S3 as a file"

Can write the output of your computes back to S3 

VPN or Direct Connect from On Prem 

## FsX File System Deployment Options

Option one:

Scratch File System 
Temp storage
Data is not replicated
High Burst (6x faster, 200MBps per TiB)

Use this for short-term processing, or you need to optimize the costs.

Persistent File System
Long Term Storage
Data is replicated within the same AZ
Replace failed files within Minutes
Long-term processing, sensitive data

## Netapp OnTap
Works with NFS, SMB, iSCSI protocol 
More workloads running ONTAP or NAS to AWS
Works with pretty much everything
Autoscaling storage
Can take snapshots, replication, low-cost, compression and dedupe data. 
Point-in-time instant cloning

## OpenZFS
Managed
Only works with NFS protocol
ZFS to AWS
Support Snapshot, compression and lowcost
Instant Cloning, etc. 

## Storage Gateways 
"Hybrid Cloud"
Part in cloud, part is on-prem

S3 is proprietary storage. 

How do you show S3 data to on-prem solutions?
- AWS Storage Gateway is the answer.

Bridge between onprem data to cloud data

Disaster Recover, backup and restore, tied storage. 

Types of Storage Gateway. 
S3 Gateway 
Vol Gateway
Tape Gateway

## Amazon S3 File Gateway
So essentially your app server which is either NFS or SMB, talks to the S3 File gateway which then does operations over HTTPS

Nice thing is you could use a lifecycle policy to move stuff to S3 Glacier. 

Most recent used data is stored at the cache

## Volume Gateway

Block Storage with iSCSI protocol backed by S3
EBS Snapshot that help restore on-prem volumes
Cached Volumes, low latency to most recent data
Stored Volumes, entire dataset is on premise, but scheduled backups to S3. 

Same path as the other one, but uses the iSCSI protocol. 

## Tape Gateway
This uses physical magntic tapes
Virtual Tape Library (VTL)
Back up dtat using tape-based processes (iSCSI interface)

Again same path as before, you have our backup server using ISCSI, which has access to the tapes, this also communicates to the Tape Gateway, and then the Tape Gateway uses HTTPS. Which then stores the virtual tapes in S#

## AWS Transfer Family 
Moving data in and out of S3 using FTP. 

FTP, FTPS (over SSL), SFTP (Secure FTP)
Managed Infra, Scalable, Reliable, highly Available, etc. 

Cost == per endpoint, per hour + data transfers in GB

Can use outside auth systems or use the inservice auth. 

## AWS DataSync 
Move large amount of data to and from locations
- On prem, or other cloud locations, TO AWS
Uses a agent in the other cloud/on-prem to do this. 

AWS to AWS no agent required

Replication is Sync on a schedule

Can preserver the metadata and permissions of the files

