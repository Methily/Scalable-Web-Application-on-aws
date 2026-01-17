# Scalable-Web-Application-on-aws
 This repository is based on project for making building highly available, scalable web application in AMAZON WEB SERVICES.

 ## IN THIS PROJECT -
 The services we will use are - 
 
 1) Amazon  Route 53
 2) IAM Roles & Policies (to check necessary permissions)
 3) Elastic Load Balancer (ELB)
 4) Auto Scaling Group (in EC2)
 5) EC2 Instances
 6) Amazon RDS (MySQL/PostgreSQL)
 7) Amazon S3
 8) Amazon CloudWatch
 9) AWS Cloud9

## LEARN -
* Architectural Design.(it will be easier to understand the structure)
* Estimating class- Pricing Calculator.
  - to estimate prices and learn how to manage the services based on your bugdet.
 
This repository shows the implementation of a highly available, fault-tolerant, and auto-scaling web application infrastructure on Amazon Web Services (AWS). The project showcases enterprise-grade cloud architecture patterns and best practices for modern web applications.
Architecture Overview
This project implements a 3-tier architecture with the following components:

Presentation Tier: Load-balanced web servers across multiple Availability Zones

Application Tier: Auto-scaling EC2 instances running application logic

Data Tier: RDS database with read replicas for high availability

# Prerequisites
Technical Requirements-
You use Terraform to connect your own laptop with AWS environment. This is done by setting up AWS CLI. 
Terraform is like code that we usually do similar to python, you can say. It increases our workload efficiency by everything in one go rather then going to every single service and doing work manually.

AWS Account with appropriate permissions
Basic understanding of networking concepts (VPC, subnets, security groups)
Familiarity with Linux command line
Knowledge of web application deployment

#Required Knowledge (Recommended)
- Terraform
-AWS CLI
-Basic knowledge of AWS Services.
-Architectural designs

## Architectural Design Principles
 -you should be able to Understanding of 3-tier architecture
 -Load balancing concepts
 -Database replication and failover
 


## AWS Pricing Calculator

Make sure  to have Cost estimation for production workloads.
For Budget management and cost controls use Cloud watch and IAM for managment access.


##  Deployment Guide

- Phase 1: Infrastructure Setup 
Step 1: Network Foundation
bash#
- Create VPC and networking components
  (once you do make the VPC it should look something like this -)
  
  <img width="581" height="797" alt="Screenshot 2025-03-24 090803" src="https://github.com/user-attachments/assets/1f2c3a07-ddd2-4592-8b9e-fb801af94c7c" />

and when you created the 
- aws cloudformation create-stack \
  --stack-name scalable-webapp-network \
  --template-body file://cloudformation/network.yaml \
  --parameters ParameterKey=Environment,ParameterValue=production
Step 2: Security Configuration
bash# Deploy IAM roles and security groups
aws cloudformation create-stack \
  --stack-name scalable-webapp-security \
  --template-body file://cloudformation/security.yaml \
  --capabilities CAPABILITY_IAM
Step 3: Database Layer
bash# Deploy RDS instance with Multi-AZ
aws cloudformation create-stack \
  --stack-name scalable-webapp-database \
  --template-body file://cloudformation/database.yaml \
  --parameters ParameterKey=DBInstanceClass,ParameterValue=db.t3.micro
Phase 2: Application Deployment
Step 4: Load Balancer and Auto Scaling
bash# Deploy ALB and Auto Scaling Group
aws cloudformation create-stack \
  --stack-name scalable-webapp-compute \
  --template-body file://cloudformation/compute.yaml
Step 5: Monitoring and Logging
bash# Setup CloudWatch dashboards and alarms
aws cloudformation create-stack \
  --stack-name scalable-webapp-monitoring \
  --template-body file://cloudformation/monitoring.yaml
  
--------------------------------------------------------------------------------

#Architecture Diagram
                   
  <img width="683" height="755" alt="image" src="https://github.com/user-attachments/assets/41db76db-3b6c-4431-8bef-a17d7ad9c336" />

----------------------------------------------------------------------------------
CPU Utilization: Scale out when >70% for 2 consecutive periods
Memory Utilization: Scale out when >80% for 2 consecutive periods
Request Count: Scale based on requests per instance
Database Connections: Monitor RDS connection pool

🎯 Testing and Validation
Load Testing
bash Apache Bench load testing

# Expected Results:
 - Zero failed requests
 - Sub-second average response time
 - Automatic scaling triggers activated

Application Response Time: < 500ms average
Error Rate: < 0.1% of total requests
Availability: 99.9% uptime SLA
Cost per Request: Optimized through auto-scaling

USE - CloudWatch Dashboards
TO :-
 -Real-time application metrics
 -Infrastructure health status
 -Cost analysis and trending
 -Security event monitoring

# Security Best Practices
Implementation Features

-Encryption: All data encrypted in transit and at rest
-Network Segmentation: Private subnets for application tier
-Access Control: IAM roles with minimal permissions
-Secret Management: Database credentials in AWS Secrets Manager
-Security Groups: Restrictive inbound/outbound rules
-SSL/TLS: End-to-end encryption via ACM certificates

# Troubleshooting Guide
Make sure to constantly check the Cloud Watch for cost.

You can bash# to Check CloudWatch alarms. it is a personal AWS tmerminal for your environment.
aws cloudwatch describe-alarms --alarm-names "High-CPU-Alarm"

# Verify scaling policies
aws autoscaling describe-policies --auto-scaling-group-name webapp-asg
Database Connection Issues
Check RDS status
aws rds describe-db-instances --db-instance-identifier webapp-db

# Verify security group rules

aws ec2 describe-security-groups --group-ids sg-xxxxx

#Repository Structure should look like - 

scalable-web-application-aws/
├── cloudformation/
│   ├── network.yaml
│   ├── security.yaml
│   ├── database.yaml
│   ├── compute.yaml
│   └── monitoring.yaml
├── application/
│   ├── src/
│   ├── requirements.txt
│   └── Dockerfile
├── scripts/
│   ├── user-data.sh
│   ├── deploy.sh
│   └── health-check.sh
├── monitoring/
│   ├── dashboards/
│   └── alarms/
├── docs/
│   ├── architecture.md
│   ├── deployment.md
│   └── troubleshooting.md
└── README.md

#📄 License

 - MIT License 

  
