# Containers... yay 

What is Docker. 

Its a dev platform to deploy apps. 

The idea is you have a "box" that has everything the app needs to run on any OS. So it doesn't matter if depenency change, or whatever. 

Essentially whenever I get tired of everyone pushing broken code to things I use, I've thrown it in a docker container to perserve its functionality. 

Microservices architecture, lift and ship apps, etc/ 

How Does Docker work on OS

Server

Container 1 Java App
Container 2 Node JS app
Container 3 DB 

Docker images are stored in Docker Repositories.
Public Repo --> hub.docker.com 

Private AWS solution --> ECR (Elastic Container Registry)

Dockerfile _=-> makes the docker Image -> which can be pushed to Docker Repo, then you can run it locally. 

## Docker Management On AWS
Amazon Elastic COntainer Service (ECS)
Amazon Elastic Kubernetes Service (EKS)
AWS Fargate
    - Serverless Container Platform 
        - I'm guessing its works on the same principal like its a lambda for containers?
    - Works with ECS and EKS

Amazon ECR
Storing container Images

**Dev Idea:**
It might be worth writing a tool that can pull down docker images//containers and do a quick once over? I bet some images have secrets or exploitable info contained in them? 

## EC2 Launch Type 
Essentially theres a boundry AWS sets up thats a ECS cluster, within that cluster there will be EC2 instances. 

YOu will then trigger an ECStask on the ECS Clusters, each EC2 instance will run the ECS Agent to register itself inside the cluster, and then AWS takes care of stopping the container from your task. 

## The Fargete Launch Options
Serverless
You just create a task definition
AWS runs the ECS task for you based on CPU/RAM needs. 

Scaling you mean increase the number of tasks, no need to manage a bunch of EC2 instances. 

## ECS / IAM ROles for ECS

EC2 Instance Profile:
Used by the ECS Agent
Makes API Calls to ECS 
Send Container Logs to CloudWatch
PUll Docker Image from ECR
Reference Sensitive data in Secretes manager or SSM parameter store

ECS Task Role:
Allows each task to have a specific role
Use different roles for different ECS services 
Taks role is defined in the task definition

## Load Balancer Intergrations
Just like running any web app on AWS. 

YOu have your cluster boundry, that has EC2 Instances in it. You stick the load balancer infront of it, and it will only accept 80/443 connection to the EC2 instances. 

You can use an application load balancer as expected, you should only really use a Network load balancer for high trhoughtput or high performance use cases, maybe pair with AWS Private Link. 

Just don't use Classic, no Fargate suppoer. 

## Data Volumes

Mount EFS systems onto the ECS tasks not to the EC2 instances directly. 
Works for both EC2 and Fargate implementations. 
Fargate + EFS == Serverless

## ECS Autoscaling 
You can manually increase the number ECS tasks

To automate this you could use AWS Application AUto Scaling

3 Metrics it uses
- ECS Service Average CPU Util
- ECS Service Average Mem Util
- ALB request count per target

Target Tracking - Target Value for CLoudWatch Metric 
Step Scaling - Cloud Alarm Trigger
Schedule Scaling - Scale Based on time (Like getting ready for xmas rush season)

ECS Auto Scaling != EC2 Auto Scaling
This is why you want to use Fargate its all serverless

If your using the EC2 launch type

Auto Scaling Group Scaling
Scale based on CPU utilization
Adds EC2 Instance over time

ECS CLuster Capacity Provider (smarter way to handle this)
- It detects when you lack capacity to launch new tasks so it'll auto provision more EC2 isntances


## Event Bridge
So an event triggers (like S3 Upload), Eventbridge is triggered to run an ECS task, the task may pull that file from S3 and then save the results into an RDS

## EVent BRidge Schedule
CRON JOBS IN AWS 
(every hour, check x,  run task, save output)

## SQS Quesue 


Intercept Stopped Tasks

## Amazon ECR