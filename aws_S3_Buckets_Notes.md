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

# Moving Between Storage Classes

Transition Actions - Move to IA after 50 days after creation

Expiration Actions - Delete Logs after 365 days 

Apply rules to prefixs 
Apply rules to object tags 

S3 Analytics can help you detemine what data should go into what storage class. Only works for Standard and Standard IA

Report is updated daily, 

takes 1 to 2 days to start seeing data analysis 

## S3 Requester Pays
Bucket owners pays for everything usually. 

A lot of heavy files, can enable requester pays. 
Self explanitory. 
Must be authenticated inside AWS. 

## S3 Events Notifications

Object creates, destoyed, Replicated, etc 
Can filter based on extension

And then send them to whatever target you want. 
Usually in seconds but can take a minute or longer. 

SNS Resource Access Policy - Allows S3 bucket to send messages to SNS. SQS, or Lambda. 

All Events end up in Amazon EventBridge  then can be sent to AWS Services as a desition. 

More advance filtering rules, 

## S3 Baseline Performance

100ms-200ms - 
3,500 PUT/COPY/POST/DELETE 
5,500 GET HEAD per prefix in a bucket

No limits to amount of prefixes in your buckets

a prefix is anything past bucket names and before the file name 

Multipart upload - recommended for 100Mb files
MUST USE for files that are 5GBs
Can help parallelize uploads

S3 Transfer Acceleration, sends to edge location (200+) which then sends the data to the S3 in the target region

This is compatiable with multi-part upload 

## S3 Byte Range Fetches

Parallelize Gets by byte ranges

Better resilience in case of failures

Can speend up downloads. 
Header range only request the first 50 bytes. 

## S3 Batch Operations

Bulk operations on pre-existing objects in the bucket. 

Encrypted all unencrypted objects, modify ACLS, add tags, etc. 

You can track progress, make reports, etc. 

## S3 Storage Lens
Helps you optimize Storage across your AWS Organization
Discover ANomalies, cost effciences, and apply data protections best practices
Make or use default dashboard
Export metrics daily to an S3 Bucket

## Default Dashboard
Visualised insights, multi-region multi-account
Can't delete but disable is possible of the default dashboard

Missed one question about S3 and its Athena inegration with a byte range reader. 

# S3 SECURITY 

## Encryption 
Bucket policies are always eval'd before default encryption settings. 

Server Side Encryption

### Server Side Encryption with Amazon S3 Managed Keys - Default option 

Severside encyrption, AES256, Key is owned by AWS
Set Header "x-amz-server-side-encryption":"AES256"
Enabled by default for new buckets//objects


### Server Side Encryption with KMS Keys stored in AWS KMS (SSE-KMS)
You want to manage aws keys, so you can actually access the key. 
"x-amz-server-side-encryption":"aws:kms"

Some limitations:
Biggest bottleneck will be the API calls. 
For example you download the files, KMS will use the Decrypt API. Every call counts towards your KMS quota (per second) this varies based on region. You can increase the quote amount bu the Sevice Quotas console. 

### Server-Side Encryption with Customer Proivded Keys 
Upload File + key
(Key is managed outside of AWS)

Then AWS uses the key with the file for encryption 

The user must provide the key to decrypt the file. 


### Client Side Encryption

You encrypt the data before sending to AWS, and decrypt it after pulling from S3. 

## Encryption in Transit
SSL/TLS (connection between you and target host is encrypted)

You can force this as a bucket policy. Using 

```
"effect":"Deny",
"Principal":"*",
"Action":"s3:GetObject",
"Resource": "arn:aws:s3:::bucketname/*",
"Condition":{
    "Bool": {
        "aws:SecureTransport": "false"
    }
}

```

## DSSE-KMS
"double encryption based on KMS"

## CORS (Cross Orgin Resource Sharing)

Orgin == protocol + host + port
example: https://www.ianscoolsdomain.com 

Browser Based Security

Get images from other server. 

So the web Browser does a preflight request to the cross origin (that has the images)

Get HTTP Verbs from CrossOrigin 

So if a client makes a cross-orgin request to our S3 Bucket
We need to returned the correct CORS headers

Easy mode: slap a * for all orgins 

## S3 MFA Delete
Force users to use a generated code before doing important S3 operations. 

- PErm delete an object version
- Suspend versioning 

To Enable: Versioning must be enabled on the bucket 

Only a bucket owner (root account) can enabled disable this feature. 

Have to enable this via CLI

Once enabled you can only delete files from the CLI 

## S3 Access Logs

Auth or Denied, for any account. 

Target Logging Bucket must be in the same AWS Region

NEVER EVER EVER Set your logging bucket to be the monitored bucket. Its creates a terrible loop that'll grow forever. 

## S3 Pre-Signed URLS

S3 Console - 12 Hours
AWS CLI - 168 hours

Users given a pre-signed URL inherit the permission of the user that generated for GET/PUT

Like Sharepoint sharing a link to a folder//file. 


## S3 Glacier Vault Lock
Write Once Read Many 

Make a vault lock policy, lock the policy. 
(you use this for compliance needs.)

S3 Object Lock:
- versioning must be enabled
Lock at the object level, not Bucket level.

Compliance Mode: 
Object Versions can't be overwritten, or deleted by any users, including the root user
Obects retention modes can't be changed, retention periods can't be shortened. 

Governance Mode:
Most users can't overwrite or delete objection version or alter lock settings
Some users have special permissions to change the retention or delete the object. 


Legal Hold: Object is protected for ever. 

## S3 Access Points 

Diff groups don't need access to a whole bucket so you can tie groups to an Access Point. (R/W access.)

Can do this for VPC orgin using an VPC Endpoint

## S3 Lambda Objects

Run a bit of code before returning the object to the user

Useful for redactions. Converting data, adding watermarks, etc. 