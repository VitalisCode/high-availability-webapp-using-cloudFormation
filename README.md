# Highly Available Web Application on AWS

A CloudFormation reference implementation for deploying a **highly available web application across multiple Availability Zones**.

> **Focus:** AWS networking · CloudFormation · Auto Scaling · Load Balancing · IAM · private subnets

## Architecture

![AWS highly available web application architecture](https://user-images.githubusercontent.com/99427790/224475443-b62e377a-b33e-4f3b-8738-79654af6e2db.png)

## Design

The application is designed around a public load-balancing tier and private application servers:

- Public subnets host the load balancer.
- Private subnets host the application instances.
- Multiple Availability Zones provide resilience against a single-AZ failure.
- An Auto Scaling Group maintains the application fleet.
- IAM roles provide controlled access to AWS services such as S3.
- Security groups restrict traffic between the load balancer and application tier.

## Infrastructure as Code

AWS CloudFormation templates are used to create and update the environment without manually provisioning individual resources.

The deployment scripts create the network stack first and then deploy the application/server stack.

## Prerequisites

- AWS account
- AWS CLI configured with appropriate permissions
- CloudFormation templates and parameter files in this repository

## Deploy

Review the region, AMI and key-pair values in the scripts/parameter files before deployment.

```bash
# Network infrastructure
./create.sh myFirstStack network.yml network-parameters.json

# Application/server infrastructure
./update.sh mySecStack servers.yml server-parameters.json
```

Always inspect the CloudFormation change set/stack events before and after deployment.

## Security notes

The original design uses private subnets for application instances and a public load balancer. For a modern production implementation, additionally consider:

- HTTPS/TLS with a managed certificate
- Systems Manager Session Manager instead of direct SSH where possible
- No broad `0.0.0.0/0` management access
- IMDSv2 and hardened AMIs
- Least-privilege IAM policies
- Centralized logging and monitoring
- Automated patching and vulnerability scanning

## Engineering takeaway

The project demonstrates how Infrastructure as Code can create a repeatable, resilient AWS application foundation while separating public ingress from private workloads.

## Technologies

**Cloud:** AWS  
**IaC:** AWS CloudFormation  
**Compute:** EC2 · Auto Scaling  
**Networking:** VPC · Public/Private Subnets · Load Balancer  
**Security:** IAM · Security Groups

## Author

**Vitalis Ibekwe**  
Cloud · Platform · SRE · DevOps Engineer

GitHub: https://github.com/VitalisCode
