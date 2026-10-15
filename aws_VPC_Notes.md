# VPC Section

## CIDR
Classless Inter-Domain Routing (Most commonly found in AWS)

BaseIP\Subnet Mask

10.whatevers == BIG networks

172.16.0.0/12 AWS Default
192.168.0.0/16 home networks

All new accounts have a default VPC to add EC2 instances too. 

5 Per Region - Soft limit can request to have more
Max CIDR per region is 5 
Min size /28 max /16

Only private Ranges allowed

AWS Reserves 5 IP addresses in any cidr block you make. 
.0 is Network Address
.1 VPC Router
.2 Mapping to AWS DNS
.3 AWS Future Use
.255 Broadcast, AWS doesn't have broadcast but its still reserved. 

## Internet Gateway

Allows Resources in a VPC to connect to the internet. 

Must be created seperately from the VPC

## Bastion Hosts
IS a Ec2 Host named Bastion Host in a public subnet, which has access to the private subnet EC2 host. 

YOu want to restrict the Bastion host with ACLs as much as possible. 

## Nat Instancecs

Uses Elastic IPsa