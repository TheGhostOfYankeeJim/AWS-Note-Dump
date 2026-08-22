# S3 Buckets

## S3 Buckets General Info 
Infinite Scaling 
Files == Objects
Directories == Buckets
Buckets == Regionally Defined

Naming:
Shared Global Namespace - globally unique name
Account Regional Namespace - allows reuse of same bucket names across regions just adds a suffix to the end. 

Naming LAWS:
no uppercase
no underscore
no IP
must start with a lowercase letter or number
must not start with "xn--"
or end with "-s3Alias"

## S3 Objects
Objects have a key which is the full path

S3://bucketname/folder1/foldera/myfile.jpeg

key == prefix + obj name

so in this case the prefix is "/folder1/foldera/" the object name is "myfile.jpeg"

Keys are just long paths. 

Obj Values: content of the body

Max obj size is 50TB
If uploading more then 5GB you need to do a multi-part upload. (I ran into this issue with NPK)

Metadata (the other issue I ran into NPK sizing Ec2s)

Tags - useful for lifecycle management or sec

Version ID (if versioning is on)

Global == needs to be totally unique to other bucket names

Account region name space == name it whatever but AWS adds a long suffix to it. Just looking at it, it looks like its just your account number + region your working in + maybe two random letters?

S3 Presigned URL == Signed object, your credentials are encoded into the object so thats why it thinks your okie dokie to see it. Don't be done and share this URL ever. 
^^^ new tool idea that scrapes the web looking for these presigned URLS 

## S3 Security:

User Based --> IAM Policies 

Resource Based
Bucket Policies - Bucket wide rules (MOST COMMON)
Object Access Control List = Finer Grain
Bucket Access Control List = Less common 

IAM Principal can access an S3 Object:
    - IF the IAM permissions allow it OR the resource policy Allows it
    - AND no explicty deny

Encryption: can encrypt objects using encryption keys

Note: Polcies can allow for Cross-Account access to S3 Buckets. 

S3 doesn't trust its users so even if you have polcies that make the bucket and or the objects public. There are bucket setting that can overide that in the GUI

You can even set this at the account level to ensure no buckets EVER will be public. 

## Website Hosting

It can host a static website

## S3 Versioning 
Same idea as Sharepoint versioning
Its done at the bucket level
Same key will be overwritten as version 1,2,3 

Good for restoring previous version, easy roll backs. 

Any version before enabling versioning will be called "Null"

If you uncheck versioning all previous version will still exist. 

## S3 Replication
I want to copy data asynchronously from one bucket to an other bucket

CRR (Cross Region Rep) 
Need to enable versioning in both buckets
Cross Account Compatiable
IAM Permissions to S3

SRR (Same Region Rep)
Need to enable versioning in both buckets
Cross Account Compatiable
IAM Permissions to S3

You might do this for compliance or to offer the same data at a lower latency like east coast copies data to west cost. 

After anablig replication, ONLY NEW OBJs get replicated

S3 Batch Replication, replicates exisiting documents. 
Can replicate delete markers 

Can't chain bucket replication. 

## S3 Storage Classes

Standard - General Purpuse 
99.99 Ava
Used for frequently used data, low latency, high throughput
Sustain 2 concurrent facility failures

Standard-Infrequent Access (IA)
Less touched data, but rapid access when needed
lower cost than S3 standard, will get charged when you pull data out though 
99.9% Ava
Disaster Recovery, Backups, 

One Zone-Infrequent Access
99.9999999 in single AZ, data lost if AZ is destroyed
99.5 Ava
Secondary Backups, data you can recreate

Glacier Instant Retrieval
Milisecond retrievals (Like once a quarter) for 90 days

Glacier Flexible Retrieval
Expedited - 1 to 5 mines, standard 3 to 5 hours, and bulk 5 to 12 hours (but this is free)
minimum storage 90 days. 

Glacier Deep Archive
LONG TERM STORAGE
12 standard, bulk 48 hours
180 minimal storage


Intelligent Tiering 
This is based on usage of objects. 
Costs a monthly and auto-tiering fee
No retrieval charges though with this service

Frequent Access
Infrequent Access Teir - 30 days no touch
Archive Access Instant Access - 90 days no touch
Archive Access Tier - 90 to 700 days no touch
Deep Archive Access Tier - 180 to 700+ days no touch 

You can move between classes manually or automatically. 

Durability (the 11 9s thing) 99.999999999% across multiple AZs

So that means if you store 10m objects, you can expect to lose 1, every 10k years. All storage classes have this. 

Availability 

S3 Standard 99.99% 53 minutes a year of downtime. 

You can create your own lifecyle rules in a S3 bucket menu. 

## Express Zone
HIGH PREFORMANCE but only for one AZ (directory bucket).
You choose which AZ you want it in. 
100,000 requests per second, single digit millisecond latency. 
10X better preformance. 
Can co-locate your data and EC2 instances together, use for AI ML, Fin modeling, media processing, HPC, etc. 

## Notes

Explicity DENY in an IAM policy will take precendent over a S3 Bucket policy. 

Passed: Missed one question on what policies trump other policies. 