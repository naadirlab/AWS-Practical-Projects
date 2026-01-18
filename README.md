# AWS Networking & Infrastructure Projects

This repo has three small AWS projects I built while learning networking, high availability, and hosting.
The idea was to actually try things out and see how different AWS services work together in practice.

# Repository Structure

- 1 - Custom VPC Architecture
    - README.md
- 2 - Application Load Balancer & Multi-AZ EC2
    - README.md
- 3 - Static Website on AWS (S3 + CloudFront + Route53)
    - README.md
- Images
    - (screenshots for all projects)
- README.md 

## What I Built

### 1. Custom VPC Architecture

A custom AWS VPC built from scratch to understand core networking concepts.

Key components:
- Custom VPC with CIDR 10.0.0.0/16
- Public and private subnets
- Internet Gateway for public access
- NAT Gateway for outbound internet access from private instances
- Separate route tables for public and private traffic
- EC2 instances in public and private subnets
- Security groups with restricted access


### 2. Application Load Balancer & Multi-AZ EC2

An extension of the VPC project to focus on availability and traffic management.

Key components:
- EC2 instances deployed across multiple Availability Zones
- Application Load Balancer distributing HTTP traffic
- Target groups with health checks
- Security groups configured so EC2 instances only accept traffic from the ALB
- Simple web server setup using EC2 user-data
- Route 53 alias record pointing to the ALB 


### 3. Static Website on AWS (S3 + CloudFront)

A simple static website hosted and delivered using AWS managed services.

Key components:
- S3 bucket configured for static website hosting
- Public access managed via bucket policy
- index.html and error.html
- CloudFront distribution in front of S3
- HTTPS enforced with HTTP → HTTPS redirection
- Caching optimized for better performance

## Key Learnings

- How AWS networking components (VPC, subnets, route tables) work together
- The difference between public and private infrastructure in practice
- Why NAT Gateways are used instead of exposing private instances
- How Application Load Balancers improve availability and security
- How CloudFront improves performance and reduces load on origins
- How DNS and alias records integrate with AWS services

## Why These Projects Matter

These setups are close to real-world architectures:
- Not everything is public
- Access is restricted by design
- High availability is planned, not added later
- Managed services are used where they make sense

This repository reflects my learning path in AWS and cloud fundamentals and serves as a base for moving into Terraform, CI/CD, and container-based deployments.

