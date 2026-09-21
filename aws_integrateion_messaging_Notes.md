# AWS Intergration and Messaging 

Middleware to manage AWS services?

Sync Communication
App to App
Can run into issues when one service produces more then the other service. 

Asynchronous / Event Based
App -> Queue -> Shipping Service app
This is to decouple services so one doesn't DOS the other essentially. 


## Standard Queue Review

SQS: Amazon Service - oldest service

Decoupling Applications 

Things to remember:
    Unlimithed Throughput, unlimited messages in queue 
    Short lifespan, 4 days - 14 days
    Low Latency
    Limit: 1024Lb per message sent

Can have duped messages (at least once delivery)
Can oder messages for you (Does best effort ordering)

Producers == things that send messages to the SQS using SDK (SendMessage API)

useful for managing ordering info for workflows. 

Consumers == Apps that uses the messages 
Consumers POLL SQS for messages (Can recieve 10 at a time)

SQS wiyh Auto Scaling Group
Use Queue length with CloudWatch Metric

Can use this to decouple between application tiers. (Like front end vs backend functionality//video processing)

Front end doesn't need much, but back end may need a beefier machine to accomplish the job. 

Encryption: 
In-Flight using HTTPS
At Rest using KMS keys
Can do client side encryption but its on you to do the encrypt and decrypt 

Access Controls: IAM Policies determine access to the SQS API

## Visibility Timeout
This all happens after a message is polled
- Becomes invisible to other consumers
- 30 seconds to be processed (which means the message has to be processed by this time)

If a consumer needs more time, but you don't want it to be processed twice use the ChangeMessageVisibilty API

IF you set it too high, and the consumer crashes it could take hours for it to reappear in the queue

Set it to short you can get duplicates

##  Long Polling
Effectively, consumer polls the queue, no messages in queue, the consomer can then just hang out and wait for a message to arrive 

Decreases amount of API calls
Decreases Latency of the app

## FIFO
Programming//Stack Theory 101 stuff essentially. 

(First in first out)

Lunch tray example. (Taking from the bottom of the pile)

300 msg/s without batching
3000 msg/s with batching
Exactly once send capability (uses dedupe ID)

PROTIP: Even if you select FIFO as an option the name MUST end with .fifo

# Amazon SNS
One message to many receivers
Pub/Sub model (Publish / Subscription)

Buying Service -> SNS Topic -> To many other services

Producer only hits one SNS topic

12,500,000 recievers to listen to one SNS topic 
Each subscriber to the topic will get all the messages (Can filter messages fyi)
100,000 Topics limit. 

Subscribers can, send emails, sms and mobile notifications, HTTPs endpoints, SQS, Lambda, Kinesis Data Firehouse.

Same encryption as SQS. 

## SNS and SQS Fan Out Method
Long and short of it, is the SNStopic, will feed into multiple SQS queues that other services can pull from their own queue. 

Nice that this is FULLY decoupled, no data loss 

Message Policy
JSON Policy 
If not policy a sub will get every message

## Amazon Kinesis Data Streams
"Real Time"

Created and used on the spot data. 
Take data from your apps/devices -> Producers Apps/Agent -> KDS -> Consumers (Apps, Lambda, etc)

365 Retention
Replay Data by consumers
Once sent can't be deleted
10MiB, normal use case is small real-time data
Partition ID.

## Capacity Modes

Provisioned Mode
Choose # of Shards
Each Shard: 1MB/s in (1000 records per second)
Each Shard: 2MB/s out
Manually Increase or Decrease # of Shards
Pay Per Shard Per Hour

On-Demand Mode
No need to provision or manage your capacity
Default starts at (4MB/s in)
Scale Auto based on throughput peak of the last 30 days

## Amazon Firehouse
Send data to target desinations 

~~Producers -> Firehouse ->~~
Just look up the graphic this is a little bit more involved. 
Fully Managed, can do 3rd Party as well, and custom HTTP Endpoints

Automatic Scaling, Serverless, pay for what you use. Near real time services, supports CSV, JSON, PArquet, Acro, raw, binary data. 

Custom data transformation with Lambda. 

## Kinesis Data Streams Vs Amazon Data Firehose 

Kinesis
- Streami g Data
Producer and Consumer Code
Real Time
Provisioned/On Demand Mode
Retention 365 days
Replay capable

Amazond Data Firehouse
Loaid Streaming data to elsewhere 
Fully Managed
Near real-time
Autoscaling
No Data Storage
No Replay capability

## AMazon MQ
This is for on-prem solutions 

I.e. I don't want to re-engineer my app to use SQS and SNS.

Managed broker service for RabbitMQ and active MQ


