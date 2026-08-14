## Budget Params
Admin access != give you billing access
This is just to make sure you down blow through the free tier. 

## EC2 General Info
EC2 Elastic Compute Cloud, Infra as a Service
VMs in the cloud, 
Storing data on virtual drives (Elastic Block Storage - EBS)
Breaking load across a bunch of machine (Elastic Load Balancing - ELB)
Scaling using auto-scaling group (ASG)

"Rent compute on demand."

### EC2 Size and Config
OS (Windows Linux Mac ) - I was not aware Mac was an option?
    - Maybe I can use this for testing MacOS Apps?

Compute Power and Cores (CPU)
Ram Configuration
Storage
    - EBS // EFS storage options (Network Based)
    - EC2 Instance Store (Hardware Based)
Network Card
Firewall Rules - Security Group
Bootstrap Script (Config on first launch EC2 User Data)

When creating a SSH keypair (recommended for EC2)
.pem for newer windows

If you power down a instance the public IPV4 IP address may change, but the private internal address wont. 


## EC2 User Data (Bootstrapping )
Launches commands when a machine boots
    - this could be useful for my "Theres a C2 In My AWS Soup" idea
Script is only launch on the very first  boot never again
    - Use lisence key, download CobaltStrike, set up SSL cert, etc. 
    - Runs with root user (Has sudo rights)

## EC2 Instance Types

General Purpose, Compute Op, Memory Op, etc

Naming convention:
m5.2xlarge
m == instance class
5 == generation (this will improve over time)
2xlarge == size of the instance class

General Purpose
Balanced between Compute, Memory, Network. 

Compute Optimized
Batch processing data
Media Transcoding
High Performance Web
Gaming Servers
High Performance Coputing
Modeling // Machine Learning
C# naming convention

Memory Optimized
Wordloads the porcess large data sets in memory
Databases (relational/non-relational databases)
Elasti Cache (Distrivuted Web Scale Caches)
Business Intelligence (in memory databases)
Real time processing of big unstructured data
R-series, X1, Z1 naming conventions.

Storage Optimized
HIgh Frequency Online Transaction Processing (OLTP)
Relational and NoSQL databases
Cache for in memory database (Redis)
Datawarehousing (duh)

## EC2 Security Groups
Super Important to AWS Net Sec
They control how traffic is aloowed ~~or denied~~ to EC2 Instances

Security Groups only Contain allow rules (I guess that means everything is implicitly denied then)

Can be ref by IP or by sec group. 

I.e. Security Group Web Servers 
    - allow HTTP access to the host from X IPs addresses
    - type, protocol, port range, source, description
    - inbound vs outbound groups

Security groups can be attached to multiple instances 
They are locked down by region and VPC combos
This is essentially a firewall OUTSIDE the host, the traffic will never make it to the host. 
Best practice is to maintain one security group for SSH

If app times out --> security group issues
BUT, if you app gives connection refused, its an app error or it never launched. (First means the traffic never made it to the app like an external firewall, the 2nd means it did but the app itself failed for some reason)


