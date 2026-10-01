# Monitoring and Auditing

## CloudWatch

## Metrics
Has metrics for every service in AWS
Metrics belong to a namespace (service)

Dimension is an attrivute of a metric instance ID
up to 30 dimensions per metric
Uses Timestamps
Can make custom metric

## Metric Steam
Near real time deliebery, i.e. to Kinesis Data FireHouse and then send it else where. 

## Cloudwatch Logs

Log Groups, reps a app you own
Log Stream, instances within app/container

Define expiration policies

CloudWatch Logs
Amazon S3 
Kinesis Data Streams, Data Firehose, Lambda

Send logs via SDK, CloudWatch Logs Agent, CLoudWatch Unified Agent
Elastic Beankstalk - web apps
ECS, Lambda, VPC FLow Logs, API Gateway, CloudTrail, Route53

## CloudWatch Logs Insights
Query Logs (historical data only)
Find specific IP, look for errors 
The same stuff when I'm trouble shooting AWS NPK

Can save queries and add them to a dashboard

There is a way to query multiple log groups in different AWS Accounts


## S3 Export
CreateExportTask can take up to 12 hours

For real time or near real time you need to use CloudWatch Log Subscriptions 

Send data to Kinesis Data Streams//FireHouse and Lambda

Sub Filter which events delivered to your destination

Cross Account Sub
Effectively you used the Subscription filter to pass the logs to a Subscription Destination via a IAM Role to the other account (Cross Accout) allowing PutRecord API call. 

## Cloud Watch Agent 

By Default, no EC2 logs will to to CloudWatch

You need an Agent on EC2 to push logs

Need IAM permissions, can set this up for onprem solutions. 

CloudWatch Logs Agent - Old Version
Only send to CloudWatch Logs

Unified Agent - New Version
System level logs (Wayyyy more granular)
Send to CloudWatch Logs
Centeralized using SSM Parameter Store

## CloudWatch Alarms 

Alarms use to trigger Notificiations
Alarm States: 
ok
Insuff Data
ALarm

Period:
length of time in seconds to evaluate the metric
High Resolution, 10 30 60 secs

## Alarm Targets
Stop, Terminate, Reboot, and Recover an EC2 Instance
Trigger Auto Scaling Action
Send notification to SNS services and hook it to a lambda function

## Composite Alarms
Multiple Metrics for an Alarm trigger
AND // OR conditions

Helps reduce alarm noise 

## Instance Recovery

Status Check
Instance = checks EC2 VM
System = Checks Hardware
EBS = Checks the EBS volume(s)

Same private,oublic, elastic IP ect

## CloudWatch Network Synthetic Monitor
Essentially you can watch metrics between an on prem datacenter and your AWS environment. 

# EventBridge

Schedule Cron Jobs
Every hour trigger lambda function

Pattern, IAM Root user signs in. SNS topic with email notification
trigger lambda fumctions

Event Bridge Sits in the Middle. 

Source -> Filter to Event Bridge -> Creates JSON to other destinations. 

Partners will send their events via a Partner Event Bus.

## Chema Registry
EventBridge can analyze the events in your bus


## CloudWatch Container Insights
ECS EKS Fargate and Kubernetes platforms on EC@

Need a containerized version of the Agent to get the data

## Lambda Insights
Self explanitory

Runs next to your lambda as a layer and then creates a dashboard for you 

# Contributor Insights
Find top talkers in your network and understand how its impacting your network. 

Find the heaviest network users, or URLs that generate the most errors. 

Can build rules from scratch or use built in rules

# App Insights
Sage Maker for making dashboards
Alerts sent to EventBridge or SSM 

## CloudTrail 
Essentially helps you monitor your AWS account not the specific things in your AWS account. 

Example: Someone deleted something in your AWS account, you can use CloudTrail to find out who did what and when it happened. 

Can collect events and API calls from the console, SDK, CLI and AWS Services. 

Management Events
- Anything that modifies your resources

Read events don't modify resources and write events do

Data Events
Not logged by default
S3 Object activity (things been messed with)
Lambda Invoked API

CloudTrail Insight Events

## CloudTrail Insight Events
Essentially this looks for outliers in your data i.e. unusal activity

Retentions
 
 90 days by default then deleted
 If you want them longer send them to S3 and then you have to use Athena to analyze them. 

 ## EventBridge - Intercept API Calls

 DeleteTable API call, goes to a DB, logAPI call to CloudTrail that event goes to the EventBridge which triggers an Alert for SNS to do something about it like send an email saying so and so is deleting a table. 

 ## AWS Config

 Auditing and recording compliance AWS Resources
 Unrestricted SSH access to my sec groups
 Public BUckets
 How the ALB config changed over time

Out of box config rules 75+
Or make your own

They do not actually stop anything or stop stuff from being configured.

.003 per configuration per region, .001 per config rule eval per region

I.e. IAM access keys are older 90 days, AWS config monitors for this, SSM Automation Documents cand recoke the IAM Creds though from a trigger from AWS Config

## Notifications
Resoruce is non compliance trigger to event bridge
