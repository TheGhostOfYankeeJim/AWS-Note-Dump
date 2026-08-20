# Classic Solutions Game

## This is some common scenerios on how to tackle these issues. 

Good section just watch the videos again. 

GoldenAMI - Install all the tools you need ahead of time
BootStrapping with USer Data (Slower only for dynamic conf)

Hybrid == Beanstalk 

## BeanStalk
Dev Center View deploying APP on AWS
Managed service, handles the capacity provisoning, load balancing, health monitor, instance config. 

You still have full control though.

Beanstalk is 
Application
Application Version
Environment
    - AWS resources running the app version
    - Tiers Web Server Tier and Worker Environment Tier
    - Can create envs like dev, staging, prod

Server Tier vs Worker Tier
Web environment is your typical ELB points to scaling group that has your EC2 Servers with the APP

The Worker Environment uses the SQS Queue that will send SQS messages to the now EC2 wrokers

Deployment MOdes
Single Instance
1 EC2, one RDS master, has an elastic IP

High Ava Mode 
ALB ppointing to ECS Auto Scaling Group 
With RDS MAster and RDS Standby on backend of the EC2s

Passed, I just need to review the EC2 Scaling, MultiAZ stuff again. I still miss questions on that. 