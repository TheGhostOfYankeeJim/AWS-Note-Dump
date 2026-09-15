# AWS CloudFront 
CDN in AWS
Improves performance content is cached at the edge
100's of points of presence
DDOS protection (Shield // Wev App Firewall)

Has several Orgins

S3 Bucket
Secured using Origin Access Control 

VPC origin
Apps hosted in subnet
private apps, load balancers, etc

Custom Orgin
(I think this is the common way for CloudFront Forwarding for C2 stuff)


CloudFront vs Cross Region Replication 

CloudFront
Global Edge Network
Files are cahced for a TTL (day usually)
Ideal for static content that can be accessed anywhere

S3 Cross Region Replication
Must be set up for each region you want
Files are updated in real time
Read Only
Dynamic content, targets regions

## ALB/EV2 As an Origin
You use VPC orgins as a means to accomplish this.
Allows you to deliever content from your apps, but not fully expose them to the internet. 

Deliever traffic to private
App Load Balancer
Network Load Balancer
EC2 Instances 

Most secure way to expose your apps

## CloudFront Geo Restriction
Allowlist/Blocklist
Country is determined using 3rd party Geo-IP databases
Biggest Usecase is Copyright Laws to control access to content. 

## Cache Invalidations
Force an entire or partial cache refresh, this ignores TTL. 

This is called a CloudFront Invalidation. 
* or path /images/*

## Global Accelorator

App hosted in one region, but users everywhere. 

Go as fast as possible in AWS network. 

Unicast IP
One server holds one IP address

Anycast IP
All servers hold same IP address, but get routed to the nearest one. 

Works with Elastic IP, EC2. ALB. NLB. 
Has health checks
Internal AWS Network, 

## AWS Global Accelerator vs CloudFront
Both are Global Network and edge locations
Both integrate with AWS Shield

CloudFront
Improves perf of cacheable content (images//videos)
Dynamic Content (API acceleration, dynamic site delievery)
Content is served at the edge

Global Accel
Packets proxied at the edge through AWS regions
Good for non-HTTP apps
Good for HTTP apps that require a static IP
Goof for HTTP apps that require deterministic, fast, failover. 