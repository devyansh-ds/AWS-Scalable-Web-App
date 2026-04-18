# Scalable AWS Web Application
A production-style cloud project where I built and deployed a highly available and scalable web application on AWS using EC2, Auto Scaling, and Application Load Balancer, with automated deployment from GitHub.
This project demonstrates real-world DevOps and cloud engineering practices including scalability, automation, monitoring, and load balancing.

## Project Overview
The goal of this project was to deploy a web application that automatically scales based on traffic and continuously pulls the latest code from GitHub.
Instead of manually managing servers, the entire infrastructure is automated using AWS services like Auto Scaling Group, Load Balancer, and EC2 User Data scripts.

## Key Features
- Hosted web application on Amazon EC2
- Automated code deployment from GitHub
- Web server configured using Apache (HTTPD)
- Application Load Balancer for traffic distribution
- Auto Scaling Group for dynamic scaling
- Health checks for self-healing infrastructure
- CloudWatch monitoring for system performance
- Supports high availability across multiple Availability Zones
- Fully automated infrastructure setup using Launch Template

## Tech Stack

- AWS EC2
- AWS Auto Scaling Group
- AWS Application Load Balancer (ALB)
- AWS Target Groups
- AWS CloudWatch
- AWS VPC
- GitHub
- Linux (Amazon Linux 2023)
- Apache Web Server (HTTPD)
- Bash Scripting (User Data Automation)

## Architecture Workflow
GitHub → Launch Template → Auto Scaling Group → EC2 Instances → Target Group → Application Load Balancer → Internet Gateway → Users

## Project Setup
### 1. GitHub Repository Setup
- Repository Name: `AWS-Scalable-Web-App`
- Added file: `index.html`
### 2. EC2 Launch Template Configuration
- AMI: Amazon Linux 2023
- Instance Type: t3.micro
#### Security Group Rules:
- HTTP (80) → Allow All
- SSH (22) → Optional
#### User Data Script (Automation)
```bash
#!/bin/bash
yum update -y
yum install -y httpd git

systemctl start httpd
systemctl enable httpd

cd /var/www/html

git clone https://github.com/devyansh-ds/AWS-Scalable-Web-App.git temp
cp temp/index.html /var/www/html/
rm -rf temp

systemctl restart httpd
```
### 3. Target Group Setup
- Target Type: Instances  
- Protocol: HTTP  
- Port: 80  
- Health Check Path: `/`  
### 4. Application Load Balancer (ALB)
- Type: Application Load Balancer  
- Scheme: Internet-facing  
- IP Type: IPv4  
- Subnets: Two public subnets (different Availability Zones)  
#### Listener:
- HTTP → Forward to Target Group  
### 5. Auto Scaling Group (ASG)
#### Configuration:
- Attach Launch Template  
- Attach Target Group  
#### Capacity:
- Desired: 2  
- Minimum: 1  
- Maximum: 4  
#### Scaling Policy:
- Type: Target Tracking  
- Metric: CPU Utilization  
- Threshold: 70%  

## How It Works
- Auto Scaling Group launches EC2 instances  
- User Data script runs automatically  
- Apache is installed and started  
- Code is pulled from GitHub  
- Application is served via EC2  
- Instances register with Load Balancer  
- Application Load Balancer distributes traffic evenly  

## Testing Auto Scaling
- Use tools like `stress` or `stress-ng` to generate load  
### Expected Behavior:
- CPU usage increases  
- CloudWatch triggers scaling policy  
- New EC2 instances are launched  
- Instances automatically join Load Balancer  
- Traffic is distributed across all instances  

## Monitoring (CloudWatch)
### Metrics Monitored:
- EC2 CPU Utilization  
- ALB Request Count  
- Target Group Health Status  
- Auto Scaling Activity Logs  

## Real-World Use Cases
- E-commerce platforms  
- Streaming applications  
- Banking systems  
- High traffic APIs  
- SaaS platforms  

## Skills Demonstrated
- AWS Cloud Infrastructure Design  
- Auto Scaling & Load Balancing  
- CI/CD Concepts with GitHub  
- Infrastructure Automation  
- High Availability Architecture  
- Cloud Monitoring  
- DevOps Fundamentals  

## Final Outcome
- Fully scalable web application deployed on AWS  
- Auto scaling based on traffic  
- Load balanced multi-instance architecture  
- Automated deployment from GitHub  
- Production-style cloud infrastructure  

## Future Improvements
- Add CI/CD with AWS CodePipeline  
- Use Docker containers on EC2  
- Add HTTPS using ACM + ALB  
- Add Route 53 custom domain  
- Implement blue/green deployments  
- Centralized logging using CloudWatch Logs  
