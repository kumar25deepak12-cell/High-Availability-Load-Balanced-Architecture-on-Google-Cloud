**High Availability Load Balanced Architecture on Google Cloud Platform**

**Project Overview**
This project demonstrates the implementation of a highly available and scalable web application architecture on Google Cloud Platform using a Global HTTP Load Balancer, Managed Instance Groups (MIG), autoscaling, health checks, and multi-zone deployment.

The architecture distributes incoming traffic across multiple VM instances to eliminate single points of failure, improve availability, and support automatic scaling during traffic spikes.

**Architecture Diagram**
**Internet
   ↓
Global Load Balancer
   ↓
Backend Service
   ↓
Managed Instance Group (MIG)
   ├── VM Instance 1
   ├── VM Instance 2
   └── VM Instance 3**
   
**Services Used**

| Service                      | Purpose                      |
| ---------------------------- | ---------------------------- |
| Compute Engine               | VM hosting                   |
| Instance Template            | Reusable VM blueprint        |
| Managed Instance Group (MIG) | Automated VM management      |
| Global HTTP Load Balancer    | Traffic distribution         |
| Health Checks                | Backend monitoring           |
| Autoscaling                  | Dynamic resource scaling     |
| Firewall Rules               | Traffic control and security |

**Project Objectives**

Design a highly available architecture
Eliminate single points of failure (SPOF)
Implement load balancing
Configure autoscaling
Enable self-healing infrastructure
Deploy workloads across multiple zones
Validate backend health and traffic routing

**Implementation Steps**
**Step 1** —Created a new GCP project for the HA architecture lab.
Project Name: ha-loadbalancer-lab

**Step 2 **— Created Instance Template
Navigated to:
Compute Engine → Instance Templates
Created:
web-template

Configuration:
Machine Type: e2-micro
OS: Debian 12
Allow HTTP Traffic enabled

Purpose:
Standardized VM configuration
Automated infrastructure deployment
Reusable compute blueprint

**Step 3 **— Added Startup Script
Configured startup automation script inside instance template.

Startup Script:
#! /bin/bash
apt update
apt install -y nginx
echo "Hello from $(hostname)" > /var/www/html/index.html
systemctl restart nginx

Purpose:
Automatically install nginx
Configure web page dynamically
Enable scalable automated deployments

**Step 4 **— Created Managed Instance Group (MIG)
Navigated to:
Compute Engine → Instance Groups
Created: web-mig

Configuration:
Multi-zone deployment
Autoscaling enabled
Instance Template: web-template
Zones Used:
asia-south1-a
asia-south1-b
asia-south1-c

Purpose:
High Availability
Fault Tolerance
Automated VM lifecycle management

**Step 5** — Configured Autoscaling
Autoscaling Settings:
Setting	Value
Minimum Instances	2
Maximum Instances	5
Metric	CPU Utilization
Target CPU	60%

Purpose:
Handle traffic spikes automatically
Improve elasticity
Optimize resource usage and cost

**Step 6 **— Created Health Check
Created:
web-health-check

Configuration:
Setting	Value
Protocol	HTTP
Port	80

Purpose:
Monitor backend instance health
Remove unhealthy instances automatically
Support self-healing infrastructure

**Step 7** — Created Global HTTP Load Balancer
Navigated to:
Network Services → Load Balancing
Created:web-load-balancer

Configuration:
Global External Application Load Balancer
Frontend Protocol: HTTP
Port: 80
Backend Service attached to web-mig

Purpose:
Distribute incoming traffic
Improve application availability
Enable fault-tolerant architecture

**Step 8 **— Configured Backend Service
Created:
web-backend-service
Backend: web-mig
Health Check: web-health-check

Load Balancing Policy:
Round Robin

Purpose:
Distribute traffic evenly
Route requests to healthy instances only

**Step 9** — Troubleshooting Backend Unhealthy Issue
Problem
Backend service status showed:
Unhealthy
Root Cause: HTTP traffic on Port 80 was blocked by firewall rules.
Health checks could not access nginx service running on VM instances.

**Step 10 **— Fixed Firewall Issue
Navigated to: VPC Network → Firewall

Created firewall rule:
Setting	Value
Name	allow-http
Direction	Ingress
Source IP Range	0.0.0.0/0
Protocols/Ports	tcp:80

Result:
Backend health became healthy
Load balancer successfully routed traffic

**Step 11** — Validated Load Balancer
Accessed Load Balancer frontend IP: http://LOAD-BALANCER-IP

Output:
Hello from web-mig-xxxxx

This validated:
Load Balancer functionality
Backend health
Traffic distribution
HA architecture deployment

**Key Learnings**

| Concept                  | Learning                        |
| ------------------------ | ------------------------------- |
| High Availability        | Eliminate downtime              |
| Load Balancer            | Traffic distribution            |
| Managed Instance Groups  | Automated infrastructure        |
| Autoscaling              | Elastic resource scaling        |
| Health Checks            | Self-healing architecture       |
| Multi-zone Deployment    | Fault tolerance                 |
| Firewall Troubleshooting | Backend connectivity validation |

**Architecture Benefits**
Eliminates Single Point of Failure (SPOF)
Supports dynamic traffic scaling
Provides automated recovery
Improves application availability
Enables fault-tolerant infrastructure

**Real Enterprise Use Cases**

| Component             | Enterprise Equivalent       |
| --------------------- | --------------------------- |
| Load Balancer         | Public traffic entry point  |
| MIG                   | Web/Application server tier |
| Health Checks         | Infrastructure monitoring   |
| Autoscaling           | Traffic spike management    |
| Multi-zone Deployment | Disaster resilience         |


1. Load Balancer?
Distributes traffic across multiple servers
Prevents overloading a single instance
Improves availability and scalability

2. High Availability?
High Availability is the ability of a system to remain operational with minimal downtime by eliminating single points of failure using redundancy, load balancing, autoscaling, and fault-tolerant infrastructure.

3. Managed Instance Groups?
Autoscaling
Self-healing
Centralized infrastructure management
Rolling updates

5.Health Checks important?
Health checks continuously monitor backend instances and ensure traffic is routed only to healthy workloads.

6. issue was faced during implementation?
Backend service became unhealthy because HTTP traffic on Port 80 was blocked by firewall rules.
Fix:allow-http → tcp:80
Production Improvements

**Production Improvements**

Future enterprise enhancements:
HTTPS with SSL certificates
Cloud Armor
Cloud CDN
Terraform automation
Cloud Monitoring & Logging
Multi-region deployment
Identity-Aware Proxy (IAP)
CI/CD pipeline integration

**Final Outcome**

Successfully designed and implemented a highly available, scalable, fault-tolerant, and load-balanced web application architecture on Google Cloud Platform using cloud-native infrastructure services and enterprise architecture principles.

<img width="1536" height="1024" alt="image" src="https://github.com/user-attachments/assets/fa5a4445-17f5-4822-a004-fe3a072d83cf" />

	
