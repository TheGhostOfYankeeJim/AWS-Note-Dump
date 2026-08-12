AWS Regions and AZ (Avaibility zones)

https://aws.amazon.com/about-aws/global-infrastructure/regions_az/

Each region has several AZs. 

## Region
Mostly a cluster of data centers
Naming convention us-east-1 eu-west-3 etc
Most AWS services are region locked
    i.e. S3 in us-east-1 and another S3 service in eu-west-3 will act like independant services

## How to choose a region:
    It depends. 
    Compliance - local goverments may require data to be hosted in specific regions
    Latency issues, deploy application and services where the bulk of your users are
    Not all services are avaiable to all regions. 
    Pricing does vary from region to region. 

## AWS Availability Zones
    Each region has at least 3 AZ, at most 6. Average is 3. 
    The naming conventions are us-east-1a, us-east-1b, us-east-1c
    Each AV is geographically seperated from each other so ensure they are isolated from a major disaster.
    Each of these AZ are liknked together, with high bandwidths and ultralow latency networking.

## AWS Points Of Presence (Edge Locations)

400+ PoPs (not includinging 10+ Regional Caches), in 90 cities and 40 countries. 
Any content served to users is with lower latency. (Like a CDN I guess?)

## General Knowledge 
You can flip around to different regions in AWS by using the dropdown next to your username in the GUI. 

Again not all services are in all regions, its important to remember that. 

Some services are globally scope such as...
    IAM, Route53,CloudFront (CDN), WAF, etc

Some services are region scoped such as... 
    EC2
    Elastic BeanStalk
    Lambda