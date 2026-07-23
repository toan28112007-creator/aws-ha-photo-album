# Highly Available Photo Album Platform on AWS

One-line summary: A self-healing, auto-scaling web application 
with serverless image processing, deployed on AWS.

## Overview
- Problem: A photo-sharing web app that needs to handle variable 
  load and recover automatically from instance failure.
- Outcome: Multi-AZ architecture with auto-scaling, layered 
  security, and serverless image thumbnail generation.

## Architecture
![Architecture Diagram](architecture-diagram.png)
- Traffic flow: User → ALB → Auto Scaling Group (private subnets) 
  → S3 / RDS / Lambda

## Key Design Decisions
(Most important section — write as decision + reasoning + trade-off)
- Why NAT Gateway over a self-managed NAT instance
- Why a referrer-based S3 bucket policy over signed URLs / CloudFront
- Why Application Load Balancer over Classic Load Balancer
- Why request-count-based target tracking over CPU-based scaling

## Security Model
- Three layers: Security Groups (least-privilege) / Network ACLs 
  (ICMP isolation) / IAM (scoped role)
- Short table: SG name → allowed source → port → rationale

## Challenges & Debugging
(Highest-value section — 3-4 representative cases, not an exhaustive log)
- Case 1: IAM instance profile not persisted in custom AMI → 
  root cause → fix
- Case 2: Lambda Sandbox.Timedout → duration/memory analysis → fix
- Case 3: S3 Access Denied despite a correctly-scoped policy → 
  referrer header mismatch → fix
Format each case as: **Symptom → Diagnosis → Fix** (3-4 lines each)

## What I'd Improve for Production
- Scoped IAM role instead of a broad lab role
- RDS Multi-AZ with automated backups
- HTTPS via ACM + Route 53
- CloudWatch Alarms and centralized logging
- Full Infrastructure as Code (CloudFormation/Terraform) instead 
  of manual console configuration

## Tech Stack
AWS: VPC, EC2, Auto Scaling, ALB, Lambda (Python), S3, RDS (MySQL), 
IAM, Network ACL
Application: PHP, AWS SDK

## Screenshots
[3-5 most representative images — not the full 35-image assignment set]
![Album page displaying photos and metadata](screenshots/album-page.png)

![Auto Scaling Group targets healthy](screenshots/asg-healthy.png)

![S3 bucket showing original and Lambda-generated thumbnail](screenshots/s3-lambda-thumbnail.png)