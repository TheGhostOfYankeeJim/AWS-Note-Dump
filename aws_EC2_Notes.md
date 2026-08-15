## Budget Params
Admin access != give you billing access
This is just to make sure you down blow through the free tier. 

## EC2 General Info
EC2 Elastic Compute Cloud, Infra as a Service
VMs in the cloud, 
Storing data on virtual drives (Elastic Block Storage - EBS)
Breaking load across a bunch of machine (Elastic Load Balancing - ELB)
Scaling using auto-scaling group (ASG)
EC2 Instances are also bound to one AZ.

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

Out of the gate, all inbound is blocked, all outbound is authorized. 

Can have IPV4 and IPV6 rules. 

## Security Groups Referencing 
You can reference security groups in other groups. 
i.e. you have 3 sec groups.
So you can say okay my EC2 instance has a security group that allows groups 1 and 2 to have unmitigated access. Group 1 and 2 would access it but group 3 won't. This way you don't have to hard code names or IP address, just refernce the groups. 

## General ports to know
22 - ssh
21 - FTP
22 - SFTP
80/443 (web stuff duh)
3389 (windows RDP)


## how to connect 
Linux just use SSH (you've done this 1000 times before)
Can use .pem files for ease just don't lose control over them lol
You cna use EC2 Instance connect (web based ssh connection)
A lot of the amazon basic images use the ec2-user as a default starting user.

You'll need to change the permissions of your pem key, generally its too open to start with. 

``` chmod 400 ```

``` ssh ec2-user@<publicIP> -i <nameOfPEMFile> ```

With windows 10, essentially the same way as linux. 
.ppk file is for putty only

``` ssh -i <pemfile> ec2-user@<public ip>```

## EC2 Instance Connect
Browser Based SSH instance (you've seen this in Azure//Digital Ocean for trouble shooting)
ideal if you don't want to manage ssh keys. This could be useful if your on a test that doesn't allow SSH/Putty, etc but allows HTTPs outbound. This will all look like HTTPs traffic from Amazon. 

## EC2 Instance Roles
You could add AWK Keys to the EC2 instance but thats super dangerous. 

So lets say you want to on a EC2 to list IAM users, you could attached an IAM role to the EC2 to give it this specific permissions. 

That way you never need to have creds just chilling on the machine for an attacker to get. This would be useful for adding troubleshooting looked up policies onto the machine. 

## EC2 Purchasing Options
On Demand Instance - Short workload, predict price, pay by second
Reserved 
    - 1 to 3 years 
    - Can do Convertible Reserved Instances (Long workloads with flexible instances)
Savings Plan
    - 1 to 3 years
    - Commit to a amount of usage

Spot instances
    - Short cheap workloads
    - Can lose instance
    - This is the same thing you use in NPK with GSpot instances

Dedicated Hosts 
    - Book a whole physical server

Dedicated Instance
    - No other Customers share your hardware (This is confusing) I think it means you share the server but have dedicated hardware for you. 

Capacity Reservations
    - Reserve capacity in specific AZ for a duration of time

### EC2 On Demand
Pay for what you use. 
Linux and Windows, per second after first minutes billing
Other OS bill per hour
Highest Cost, no upfront payment
No longterm commit
Best Usage:
    - Short term un-interrupted workloads

### EC2 Reserved Instance
72% dicount compared to On-Demand
Reserve specific instance attributes (Type, Region, Tenancy, OS)
Reserve Period == 1 year basic discount or 3 years larger discount. 
Payment Options - No Upfront, Partial, and Full. All upfront gives you maxiumum discounts.
Can buy or sell Reserved Instances in Market Place 
Best Usage:
    - Steady State Usage Apps (Databases)

### Convertible Reserved Instance
You can changes the type, instance, OS, scope, and tenancy
but only get up to a 66% discount

### EC2 Savings Plans

### Spot Instance
90% reduction compared to On-demand
You can LOSE these instances if your max prices is less then the current spot price (NPK bidding for a spot but then you get outbid and lose the spot)

Define Max Spot Price 
Essentially your bidding for compute. 
You win the bid if your max spot price is greater than the current spot price

If the current price exceeds your max price, you'll need to stop or terminate your instance within 2 minutes. 

You can do a one-time request or a persistent request

One-time request:
As soon as its spot request filled, your instance is created. And then the request goes away. 

### Spot Fleets
Set of spot instances
will try to meet target capacity with price constraints
You define possible launch pools
can have different pools that the fleet can choose
Spot fleet stops launching instance when reaching capacity or max cost

LowestPrice - Launch from pool with lowest price
Diversified distributed across all pools
capacityOptimized - optimized number of instances
priceCapacityOptimized - start iwth highest capacity and then select pool with lowest price

### EC2 Dedicated Host
You get your own server
Mostly for compliance reasons
Pay per second of resevered (1 or 3 years)
Most expensive options
Or a BYOL (bring yoru own liscence)


### EC2 Dedicated Instances 
Instances run on hardware thats dedicated to you
Can Share hardware with other instances on your account
No contol for instance placement.
Own instance on own hardware vs dedicated host which is lower level visibilty over the hardware itself 

## Networking
IPV4 and IPV6
IPV4 is most commonly used
(General college info about the differences)

If you need a public IP on your EC2 Instance to never change you'll use an Elastic IP. It'll remain as long as you don't delete the instance.

By default you can only have 5 elastic IP addresses, but you can request more through support (Like how I increased my spot instance requests for NPK)

Pricing you get changed per hourly use and ideal. Roughly $3.5 a month 

YOU NEED TO RELEASE THE PUBLIC IP ADDRESS TO STOP GETTING CHARGED FOR IT. 

### EC2 Placement Groups
When you create a placement group you have three options
Cluster - Instances are grouped together in a single AZ
Spread - spread across differnt hardward max 7 instances per group per AZ)
Partion - spread across of partitions, amounts different sets of racks. LIke a real server rack. So this helps reduce a rack failure. Big data apps HDFS, HBASE, etc.

With Spread: This is ensuring failure endurance. So if AZ group fails the others won't be effected. 

## ENI Elastic Network Interfaces
logical commpentent in VPC that reps a virtual network card
ENI primary private IPV4 and one more 2nd dary IPv4. 
Can have pne Elastic IP per private IPv4
One Public IPv4
One or more sec groups
Mac Address

Can attached them on the fly, but bound to specific AZs

So like if you want to move over one private IP to a different EC2 instance for fail over protections. 

## EC2 Hibernate
Stop - Data on disk is kept intact
Terminate -- bye bye everything
When you start - its a fresh boot

EC2 Hibernate 
Saves what was in memory. 
Which makes the boot much faster. 
Whatever was in memory is written to file in the root EBS volume
Root EBS volume has to be encrypted