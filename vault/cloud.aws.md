---
id: 71ddbf7d-3ee4-44d0-8673-6ec3654919ce
title: Aws
desc: ''
updated: 1624885592553
created: 1620974068458
---

## EC2 Instance Types

- General purpose
- Compute optimized
- Memory optimized
- Accelerated computing
- Storage optimized
## EC2 Pricing

- On-demand
    - Ideal for short-term irregular workloads that cannot be interrupted
    - Pay for only the compute time
- Reserved instance
- Spot instancce
    - Ideal for workloads with flexible start and end times
    - Do not require commitment to a consistent amount of compute usage
- Dedicated host
    - The most expensive
- Amazon EC2 saving plans
    - Ideal for workloads that involve a consistent amount of compute usage over a 1-3 year term

## Amazon EC2 Auto Scaling
- If you wanted the scaling process to happen automatically
- Enable us to automatically add or remove EC2 instances in response to demand
- Dynamic scaling
    - Responds to changing demannd
- Predictive scaling
    - Automatically schedules the right number of EC2 instannces based on predictive demand

## Type of load balancers:

- Application Load Balancer (ALB)
    - suits for load balancing HTTP & HTTPS traffic
    - operates at application layer
- Network Load Balancer (NLB)
    - suits for load balancing TCP, UDP & TLS traffic
    - operates at both application layer & transport layer (L4)
- Classic Load Balancer (CLB)
    - "legacy" load balancer
    - can handle HTTP, HTTPS, TCP & TLS traffic but with far fewer features than ALB & NLB
    - operates at both application layer & transport layer

# Useful resources

- [AWS in plain English](https://expeditedsecurity.com/aws-in-plain-english/)