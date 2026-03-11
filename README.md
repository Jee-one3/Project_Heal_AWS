# Highly Available & Resilient Infrastructure with Auto Scaling on AWS  
### Multi-AZ | Auto Scaling | Health-Based Recovery | Secure Private Backend

---
## Problem Statement

In a basic infrastructure setup, a server crash can cause downtime, traffic spikes can slow down performance and manual intervention is often required to restore the service. This creates operational risk, poor user experience and most importantly potential business loss. 

The challenge is to design a cloud infrastructure that can automatically detect failures, replace unhealthy servers, scale based on demand and keep backend systems secure — all without human intervention. 

This project addresses that challenge by building a resilient, self-healing architecture on AWS.

---

## Project Overview

Designed and deployed a **highly available, self-healing web infrastructure on AWS** that automatically detects and recovers from failures.

This project simulates real-world **Site Reliability Engineering (SRE)** principles by implementing:

- Multi-AZ high availability
- Health-based instance replacement
- Target tracking auto scaling
- Secure private subnet architecture
- Automated instance provisioning using Launch Templates

The system is built to **design for failure**, not just deployment.

---

## Architecture Diagram

![Architecture Diagram](Auto-heal.png)


---

## Architecture Components

### Networking Layer
- Custom VPC (10.0.0.0/16)
- 2 Public Subnets (Multi-AZ)
- 2 Private Subnets (Multi-AZ)
- Internet Gateway
- NAT Gateway (controlled outbound access)
- Separate Route Tables (public/private segmentation)

### Load Balancing Layer
- Application Load Balancer (ALB)
- Internet-facing
- HTTP (Port 80)
- Health checks enabled (`/` endpoint)

### Compute Layer
- Launch Template (immutable configuration)
- Auto Scaling Group (Min: 2, Max: 4)
- EC2 instances in private subnets only
- Amazon Linux 2
- Apache Web Server installed via User Data

### Security Design
- No public IP on EC2 instances
- Security Group referencing:
  - ALB SG → EC2 SG only
- IAM role-based authentication

### Scaling & Self-Healing
- Target Tracking Scaling Policy
- CPU utilization target: 30%
- ELB health checks enabled
- EC2 health checks enabled
- Automatic unhealthy instance termination and replacement
- SNS topic for mail notification

---

## Traffic Flow
Internet
➜
Internet Gateway
➜
Application Load Balancer (Public Subnets)
➜
Auto Scaling Group (Private Subnets across 2 AZs)
➜
NAT Gateway (Outbound only)

---

## Self-Healing & Failure Testing

The system was intentionally stress-tested to validate resilience.

### 1. Application Crash Simulation
Stopped Apache service on one instance.

**Result:**
- ALB health check failed
- Target marked unhealthy
- ASG terminated instance
- New instance launched automatically
- System restored without downtime
- Email notification via SNS

---

### 2. Manual Instance Termination
Terminated one EC2 instance manually.

**Result:**
- ASG detected capacity drift
- Replacement instance launched immediately
- Desired capacity maintained (2 instances)
- Email notification via SNS

---

### 3. Load Spike Simulation
Generated high CPU load using stress tool.

**Result:**
- CPU utilization exceeded 50%
- ASG triggered scale-out event
- Additional instance launched
- System stabilized under load
- Email notification via SNS

---

## Skills & Technologies Used

- Amazon EC2  
- Application Load Balancer (ALB)  
- Auto Scaling Groups  
- Launch Templates  
- Amazon VPC  
- NAT Gateway  
- IAM Roles  
- AWS Systems Manager (SSM)  
- CloudWatch Metrics  
- Linux (Amazon Linux 2)  

---

## Strengths & Limitations

### Strengths

- **High Availability (Multi-AZ Deployment):** Application remains accessible even if one Availability Zone fails.  
- **Self-Healing Infrastructure:** Unhealthy or terminated instances are automatically replaced by the Auto Scaling Group.  
- **Elastic Scalability:** CPU-based target tracking dynamically adjusts capacity based on demand.  
- **Secure Backend Architecture:** EC2 instances are deployed in private subnets with no public IP exposure.  
- **Health-Based Automation:** Application Load Balancer health checks ensure only healthy instances receive traffic.  
- **Production-Oriented Design:** Follows core Site Reliability Engineering (SRE) principles such as designing for failure and automated recovery.

---

### Current Limitations (Future Improvements)

- **Manual Infrastructure Provisioning:** Can be improved by implementing Infrastructure as Code (Terraform or CloudFormation).  
- **NAT Gateway Cost Optimization:** Could be replaced with a NAT Instance for cost efficiency in non-production environments.  
- **Limited Observability:** Can be enhanced with CloudWatch dashboards and log aggregation.
- **No CI/CD Pipeline:** Deployment updates are manual; future improvement includes automated CI/CD integration for zero-downtime deployments.   
- **Single-Region Deployment:** The system is Multi-AZ but not Multi-Region; cross-region failover could improve disaster recovery resilience.

---

## Why This Project Matters

This project demonstrates hands-on experience in:

- Building resilient cloud infrastructure
- Applying SRE principles
- Designing scalable production-ready systems
- Implementing automated recovery mechanisms
- Securing cloud-native applications

It reflects real-world DevOps and Site Reliability Engineering practices.

---

## Outputs

![VPC Structure](Outputs/1_VPC_Map.png)
![Target Group Healthy](Outputs/2_Target_Health.png)
![ASG Activity Showing Instance Replacement](Outputs/3_ASG_activity.png)
![ALB Design](Outputs/4_ALB_Map.png)

## Author

Jeevan Rohith Antony Manohar  
MS Computer Science | San Diego State University  
