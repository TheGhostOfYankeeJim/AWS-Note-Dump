# Why Serverless 
You just deploy code, not manage servers

LAMBDA DISCUSSION

## Lambda 

How NPK works, I send data to a function for processing. This is ideal for Short executions, runs on demand. 

Easy Pricing 
Pay per request and compute time
Free Tier == 1,000,000 Lambda Requests and 400,000 GB of compute time

10GB of Ram per function
More ram == more improved CPU and Network 

## LAMBDA Limits

They're by per region

Execution: Mem 128 MB - 10GB 
Max Execution Time - 15 Minutes
Environment Vars 4KB
Disk Capacity in the function container 512 to 10GB
Concurrancy Executions 1000

Deployment:
Lambda Function Deployment Size (50 MB)
Side of Uncompressed Deployment 250 MB
Use TMP directory to hold other files at startup
Size of ENV vars 4KB

## Lambda Concurrency 

1000 Concurrent Executions

Reserved concurrency (Function limit)  this applies to all apps in your account.
It'll throttle and return a 429 error

If you need more, request in a support ticket. 

## Cold Start 
New Instance is loaded and code outside the handler run (init)

First request can have a higher latency then the rest this is due to everything is starting up. 

Counter is Provisioned Concurrency:
Concurrency is allocated before the function is invoked. 

## Lambda SnapStart
Improces your lambda functions performance by 10x for Java Python and .NET

The Initial phase can take a bit, so the function is pre-initalized. 

## CUstomization at the Edge

When you need to execute logi at the edge (like a cdn) before reach the app or user itself. 

So you wrote code and attach it to cloudfront

Theres two flavors for this CloudFront Functions & Lambda@Edge

CloudFront Functions
- Cache Key Normalization
- transform request att like headers, cookies, query strings

Header Manipulation

URL Rewrites or redirects

Request Aithentication and Auth
create and validate user generated tokens 

Lambda@Edge
Longer Execution TIme
Adjust CPU and Memory
Code has 3rd party libaries
Network Access to external services for processing
File Susmte access or access to the body of the HTTP requests

## Lambda VPC

By Default the function is launched outside your VPC
There for it wont have access to things inside your VPC like RDS. 

Counter, just launch it inside your VPC. 

THe issue though is you can scale to many lambda function that your RDS instance can handle so you'd want to put an RDS proxy between your instance and your internall Lambda functions

IMproves scalability by poolong and sharing DB connections. 

Again this MUST be i your VPC, RDS Proxies can not be outside your VPC

## RDS to Lambda
Is possible, like a new user signs up, the RDS server would then invoke a lambda function to send a welcome email which invokes amazons ses service. 

The Pitfall you need to watch out for is Lambda RDS Events. RDS events do NOT give you access to you data inside the RDS.

# Amazon DYnamoDB

Fully managed, across multiple AZs
NoSQL Database, not a relational database
Works with IAM for security
No maitence or patching
Standard Class and Infrequent Access table classes

## Basics

Its just tables, each table has a primary key
Infinit number of rows, each row has attributes, can be null as well
Max size is 400KB

Schema needs to rapidly change then other RDS solutions


PRovisioned Mode (Default option)
Specify how many reads/writes per second
plan capacity beforehand
Can auto-scale

On-Demand Mode:
auto scales
No planning
Pay for what you use
great for unpredictable workloads, spikes, etc

# Advance Features
DynamoDB Accelerator

Its a cache for your DB. Helps with congestion. 

Stream Processing
Ordered Stream of item-level mods in a table

Global Table
Its a table in multiple regions and they can replicate to and from each region
Enabled Streams for this

TTL 
Delete items after they expire
Think session data that needs to be removed

Backups for DR

Continous Backups using point in time recovery
35 days
point in time recovery
recovery makes a new table

Integration with S3

Export your table to S3 

## API Gateway

Ing AWS Lambda
Supports WebSocket Protocol
Handle diff ENVs
Handle Security
API keys, request throttling
Swagger and OPen API imports
Transform and validate requests and responses
Make SDK and API specs
Cache API Responses


You'd normally do this to expose an Rest API that's backed by Lambda. 

Expose HTTP endpoints in a backend

Can uses this for AWS services as well

API Gateway ENdpoint Types

Edge-Optimized (Default)

Regional 
All users in same region 

Private
Only accessed inside your VPC

## API Gateway Security

IAM Roles (internal roles)
Cognito (external users)
Custom Authorizer (Lambda Function)

## Step Functions

Design a flow chart of when something works or doesn't work. 

Intergrates with a ton of stuff. 

Can use a human approval feature, 

## Cognito 

Gives users outside the AWS account to interact with our web or mobile app.

Cognito User Pools
Sign in functionality for app users
Integrate with API and ALB 


Cognito Identity Pools (federated)
Can hook into preexisting identity providers
