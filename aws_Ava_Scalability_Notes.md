## High Ava and Scalability 

Scalability = Application/Sys an handle greater loads by adapting

Two types of scalability
    - Vrtical Scalability
    Horizontal Scalability == elasticity

Scalability is linked, but different to high availability

### Vertical Scalabilty
its just upgrading the EC2 to to a larger EC2
t2.micro -> t2.large
Use with databases (RDS, ElastiCache can scale vertically)
Hardware limit are the hard limits.


### Horizontal Scalability
Horizontal Scalability means increasing the number of instances//implications 

### High Availability
High Availabilty goes hand in hand with horizontal scaling
High Ava == running in at least two AZ
Goal: Survive Data Center Loss

This can be passive (RDS Multi AZ)
The Hig ava can be active (for horizontal scaling)

Auto scaling Group Multi AZ
Load Balancer Multi AZ

## Elastic Load Balancing 
One point of connectivity, the client doesn't know which isntance they're connecting to. 

Why: 
    - Spread Load 
    - Expose a single point of access
    - Do reg health checks
    - Provide SSL termination
    - Enforce cookie stickiness (Session state)
    - High AVA across zones
    - Sep public and private traffic 

Why use it: 
    - Managed Load Balance
    - AWS manages it, makes sure it's working, takes care of upgrades maintaining, already intergrated with AWS services, cheaper then doing your own load balancing. 

### health checks
/health common
Checks on port and a route
If the response is not 200, no traffic to instance

Classic - CLB  (Older Gen)
    - AWS doesn't want you to use it

Application Load Balancer 
HTTP/(s), Web Socket 

NEtwork Load Balancer
TCP/TLS/UDP

Gateway Load Balancer 
Operates at layer 3. 

Can set up internal (private) and external (public) ELBs

Users can access HTTP/HTTPS from anywhere.

Then the EC2 Instance only allow traffic from load balancers

## Application load balancer (v2)
Application load balancers layer 7 (HTTP)
Load balance to multiple apps on the same machine (Containers)
Supports HTTP/2 and WebSocket
Redirect http to https

Support route routing
/users to /posts as an example to differnt target groups 
Routing based on hostname in URL
Routing based on query string//headers

ALB great for micro services & contianers (Docker or Amazon ECS)
Has a port mapping feature to redirect to dynamic port in a ECS
Otherwise you'd need a ton of classic load balancers per application to accomplish the same. 

### App Load Balancer Target Groups 
Can be: 
    - EC2 Instances (Managed by auto scaling group)
    - ECS tasks 
    - Lambda Functions 
    - IP Addresses

ALB can route to multiple target groups
health checks are at the TARGET GROUP level.

You get a fixed hostname 
Application servers don't see the client IP directly
The true IP of the client will in the X=Forwarded-for header
Can use X-Forwarded-Port and X-Forwarded-Proto

Network Load Balancing is when you need super high preformance with lowwwww latency. 

Gateway is used for security. 
Firewalls, intrusion detection, etc. For analzing the network. 

## Network Load Balancer 
Layer 4 - Transportation Layer 
Forward TCP/UDP data to your instances
Can handled millions of requests per second
One static IP per AZ. 

Target Groups: can be EC2 instances, can register IPs must be IP addresses (Private and hardcoded), network load balancer in front of a application load balancer

Health Checks support TCP, http and https
