---
id: 71ddbf7d-3ee4-44d0-8673-6ec3654919ce
title: Aws
desc: ''
updated: 1620974305746
created: 1620974068458
---

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