# Route53

## What is DNS
You already know this. 

## Route53
Authoritive DNS
Domain Registrar 
Only SLA with 100%
Can check the health of your hosts

## Records
Domain, Type, Value, Route Policy, TTL (Cache time)

DNS Record Types you must know:
A/AAAA == IPv4/IPv6
CNAME == Hostname to other hostname
    target domain must have an A/AAAA record
    No CNAME record for the top node of a DNS namespace (Zone Apex)
    No CNAME for example.com but www.example.com okie dokie 
NS - Name Servers
    How traffic is routed

## Hosted Zones

Public Hosted Zones
    app.ianscoolzone.com

Private Hosted Zones
    app.mycompany.internal

.50 per month per hosted zone, $12 min

TTL 
How long a record is good for in the cache 

Low TTL will cost more though 

Pro Tip: Best way to save money, set a long TTL, and when you need to make a change, set it low, once you know everyone has elipsed the old TTL, make the change, the old short TTL will trigger and get the new change with the now long TTL again 

TTL is require for all records except ALIAS records

## CNAME vs ALIAS

In AWS each resource is assigned a AWS Hostname <longstring>.elb.amazonaws.com but I want to actually use myalb.ianscooldomain.com. 

With CNAME 
Hostname to Hostname
ONLY FOR NON ROOT DOMAIN coolguy.ianscooldomain.com

ALIAS 
Hostname to AWS Resource
myalb.ianscooldomain.com -> Amazon Resource Name
Works with BOTH root and non root domains 
FREE
Native Health Check Built In

Alias ONLY mapped to AWS Resources 
NO TTL that is set by Route53

Good use for it are:
ELB, CloudFront, API Gateway, BeanStalk, S3 Websites, VPCs, Global Accelerator,  ROute 53 (In same hosted zone)

No ALIAS Record for EC2 DNS Names though? 

## Routing Policies

Defines how to respond to DNS Queries 

Supports:
    Simple
    Weighted
    Failover
    Latency Based
    Geolocation
    Multi-Value Answer
    Geoproximity (Uses traffic flow features)

Simple: 
Route traffic to single resource

If more than one resource, the a resource in that group is selected

Weighted:
Control what % of requests go to specific resources
Simply you assign weights to each host, they don't need to add up to 100%
The value has to be between 0 - 255
HEalth Checks
DNS records needs to have the same name and type

You'd use this to soft test a new app, send 10% of the traffic to the new app to make sure nothing breaks. 

0 means stop sending traffic, if all zeros that manes all records will be returned equally. 

Latency Based:
Direct users to least latency 
Traffic is based between users and AWS regions
Has Health Checks

## Health  Checks
Checks public health resources (can do private as well)

Health Checks that monitor a public check
Send a request to our define directory /health
30 secs, 10 secs for higher cost
Supported ProtocolL HTTP(s) // TCP

If > 18% from the 15 health checkers, Route53 considers it healthy

Only triggers on 200s and 300s. 

Can also trigger on the first 5120 bytes of the response

You need to add a rule to the router/firewall to allow the global health checkers to connect


Calculated Health Check
Combines multiple into a single health check
Parent checks in on child (Max 256) health check.

Cloud watch health Check

This is how you do private health checks

You create a CW Metric and then have that trigger an alarm, then you create a health check that looks at that alarm specifically. 

Routing Pol - Failover
If primary fails healthcheck, it then tells client to go to secondary 

Geolocation 

Based on where the user actually is, not latency. 

US citizen, point them towards the US servers.
French user goes to french version of the app.
Japanese user goes to japanese version of the app. 

Geoprox
Based on Geo location of users and your resources. 

Uses a bias value. (1 to 99 to increase traffic to resource)
(-1 to -99 to decrease traffic )

When you need to shift traffic from one region to an other region

IP-based Routing
Route based on client IP address.

List CIDR

So this might be useful for my AWS C2, set all microsoft AV stuff to go to google and not my C2. 

Multivalue 
Route traffic to multiple resources
Can use health checks
Up to 8 healthy records
Not a sub for an ELB

# Domaain Reg vs DNS Service
Can use whatever you want, like godaddy. 

Essentially you change Godaddys NS to AWS route53 as a DNS provider

## Hybrid DNS
Does AWS DNS and on Prem DNS stuff

Uses a resolver endpoint. 

Make a vpn connection between cloud and on prem. 

Effectively you have an inbound and outbound Resolver endpoint. The outbound resolver endpoint points to on prem DNS servers. 

Got all the quiz answers right, go me. s