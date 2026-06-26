# TrendMart International — Global E-Commerce Cloud Migration Strategy

![AWS](https://img.shields.io/badge/Cloud-AWS-orange)
![Architecture](https://img.shields.io/badge/Focus-Cloud%20Architecture-blue)
![Strategy](https://img.shields.io/badge/Focus-Migration%20Strategy-green)
![Disaster Recovery](https://img.shields.io/badge/Design-Disaster%20Recovery-red)
![Academic Project](https://img.shields.io/badge/Project-Academic-lightgrey)

## Overview

TrendMart International is an academic cloud architecture and migration strategy project completed for **CS6620: Fundamentals of Cloud Computing**. The project presents a full AWS modernization strategy for a global e-commerce company migrating from a legacy monolithic infrastructure to a scalable, resilient, multi-region cloud platform.

The solution focuses on improving availability, reducing latency, supporting global traffic growth, meeting EU data residency requirements, and enabling faster software delivery through cloud-native architecture.

## Business Problem

TrendMart operates on an outdated monolithic system supported by more than **200 physical servers** and a large **30TB traditional database**. The legacy architecture causes major business and technical challenges:

* Frequent failures during peak shopping events
* A 6-hour Black Friday outage affecting customer trust
* 40% cart abandonment during performance degradation
* Slow international page loads due to centralized infrastructure
* GDPR compliance risk from improper EU customer data handling
* Slow feature delivery caused by tightly coupled monolithic architecture
* High operational cost from over-provisioned physical infrastructure

## Proposed Solution

The proposed architecture migrates TrendMart to a distributed AWS platform using managed services, microservices, global routing, regional data placement, and automated disaster recovery.

### Target Outcomes

* Support up to **1,000,000 concurrent peak users**
* Achieve **99.99% availability**
* Deliver sub-200ms global user experience through CloudFront and regional routing
* Meet EU data residency requirements using EU-West regional isolation
* Reduce outage risk through Multi-AZ and cross-region disaster recovery
* Enable faster feature releases using microservices and CI/CD
* Keep projected AWS infrastructure cost below the $300K/month target

## Architecture Diagrams

### 1. Overall System Architecture

![Overall System Architecture](01%20OVERALL%20SYSTEM%20ARCHITECTURE%20DIAGRAM.png)

This diagram shows the global AWS architecture across North America, Europe, and Asia. It includes Route 53 latency-based routing, CloudFront, regional VPCs, ECS Fargate services, Aurora, DynamoDB, S3, messaging components, and centralized security and operations services.

### 2. Network Design and Security Zones

![Network Design and Security Zones](02%20NETWORK%20DESIGN%20AND%20SECURITY%20ZONES%20DIAGRAM.png)

This diagram details the VPC design across public, private application, and private data subnets. It includes Internet Gateway, NAT Gateways, Application Load Balancer, ECS application tier, Aurora, DynamoDB, S3, route tables, security groups, and VPN access for internal operations.

### 3. Order Processing Data Flow

![Order Processing Data Flow](03%20DATA%20FLOW%20DIAGRAM.png)

This diagram explains how a customer order moves through the platform, including API routing, cart lookup, inventory validation, payment processing, order creation, asynchronous inventory updates, email notifications, and analytics event streaming.

### 4. Disaster Recovery Design

![Disaster Recovery Design](04%20DISASTER%20RECOVERY%20DESIGN%20DIAGRAM.png)

This diagram shows the failover design between the primary region and disaster recovery region. It includes Route 53 health checks, ECS Fargate standby workloads, Aurora cross-region replication, Aurora read replica promotion, and S3 Cross-Region Replication.

### 5. Data Storage and Database Architecture

![Data Storage and Database Architecture](05%20DATA%20STORAGE%20AND%20DATABASE%20ARCHITECTURE%20DIAGRAM.png)

This diagram explains the data layer design using Aurora PostgreSQL for transactional data, DynamoDB for carts and sessions, ElastiCache Redis for hot data, S3 for product images and receipts, and Kinesis for analytics data lake ingestion.

## AWS Services Used

| Category        | AWS Services                                                                |
| --------------- | --------------------------------------------------------------------------- |
| Compute         | Amazon ECS Fargate                                                          |
| Networking      | VPC, Subnets, Route Tables, Internet Gateway, NAT Gateway, Site-to-Site VPN |
| Load Balancing  | Application Load Balancer                                                   |
| Global Delivery | Amazon CloudFront, Amazon Route 53                                          |
| Databases       | Amazon Aurora PostgreSQL, Amazon DynamoDB                                   |
| Caching         | Amazon ElastiCache Redis                                                    |
| Storage         | Amazon S3, S3 Cross-Region Replication, Glacier                             |
| Messaging       | Amazon SQS, Amazon SNS                                                      |
| Analytics       | Amazon Kinesis Data Streams, Amazon S3 Data Lake                            |
| Security        | IAM, AWS WAF, AWS Shield, KMS, Secrets Manager                              |
| Monitoring      | CloudWatch, CloudTrail, AWS Config, X-Ray                                   |
| Migration       | AWS Database Migration Service                                              |
| Cost Management | AWS Pricing Calculator, Cost Explorer, Trusted Advisor                      |

## System Design Highlights

### Multi-Region Architecture

The solution uses multiple AWS regions to improve global performance and resilience:

* **US-East-1** as the primary North America region
* **EU-West-1** as an active Europe region for EU data residency
* **AP-Southeast-1** for low-latency Asia customer access
* **US-West-2** as disaster recovery standby region

### Microservices Decomposition

The monolithic application is decomposed into independently deployable services:

* User Service
* Product Catalog Service
* Shopping Cart Service
* Inventory Service
* Order Service
* Payment Service
* Notification Service
* Analytics Pipeline

Each service is designed to scale independently using ECS Fargate and managed AWS services.

### Data Strategy

The platform uses a hybrid data model:

* **Aurora PostgreSQL** for strongly consistent transactional workloads such as users, orders, inventory, and payments
* **DynamoDB** for high-scale, low-latency cart and session data
* **ElastiCache Redis** for hot reads and frequently accessed product/cart data
* **S3** for images, receipts, exports, logs, and backups
* **Kinesis** for real-time order analytics and data lake ingestion

### Disaster Recovery Strategy

The disaster recovery design includes:

* Route 53 failover routing with health checks
* Aurora cross-region replication
* Aurora read replica promotion during failover
* S3 Cross-Region Replication
* ECS Fargate warm/cold standby services
* DynamoDB global tables for critical low-latency data

### Security and Compliance

Security controls include:

* Least-privilege IAM roles
* AWS WAF for SQL injection, XSS, and bot protection
* AWS Shield for DDoS protection
* KMS encryption for Aurora, DynamoDB, and S3
* TLS encryption for data in transit
* CloudTrail and AWS Config for auditability
* EU-West regional data placement for GDPR-aligned data residency

## Migration Strategy

The migration follows a phased approach to reduce business risk and avoid downtime.

### Phase 1: Foundation

* Establish AWS Landing Zone
* Configure IAM, CloudTrail, AWS Config, and VPC baseline
* Set up networking, security groups, and monitoring
* Prepare CI/CD and container deployment foundation

### Phase 2: Pilot Migration

* Extract Product Catalog from the monolith
* Deploy first ECS Fargate microservice
* Migrate initial data using AWS Database Migration Service
* Use Route 53 weighted routing for controlled traffic shifting

### Phase 3: Core Service Migration

* Migrate User, Cart, Inventory, Order, and Payment services
* Introduce SQS/SNS for asynchronous workflows
* Add caching and database scaling patterns
* Train engineering teams on AWS operations

### Phase 4: Cutover and Optimization

* Complete migration from legacy infrastructure
* Decommission physical servers
* Optimize cost using Savings Plans, Trusted Advisor, and Cost Explorer
* Finalize disaster recovery validation and operational runbooks

## Cost Estimate

The AWS Pricing Calculator estimate projects:

| Metric        |      Estimate |
| ------------- | ------------: |
| Upfront Cost  |         $0.00 |
| Monthly Cost  |   $293,278.82 |
| 12-Month Cost | $3,519,345.84 |

The estimate includes compute, databases, CloudFront, load balancing, S3, SQS, SNS, Kinesis, NAT Gateways, CloudWatch, WAF, KMS, Route 53, and multi-region infrastructure components.

## Key Deliverables

* Executive Summary Report
* Technical Architecture Document
* AWS Pricing Calculator Estimate
* Overall System Architecture Diagram
* Network Design and Security Zones Diagram
* Order Processing Data Flow Diagram
* Disaster Recovery Design Diagram
* Data Storage and Database Architecture Diagram

## Repository Structure

```text
trendmart-cloud-migration/
├── README.md
├── docs/
│   ├── TrendMart-Executive-Summary.pdf
│   ├── TrendMart-Technical-Architecture-Document.pdf
│   ├── TrendMart-AWS-Pricing-Calculator.pdf
│   └── diagrams/
│       ├── 01-overall-system-architecture.png
│       ├── 02-network-design-and-security-zones.png
│       ├── 03-data-flow-diagram.png
│       ├── 04-disaster-recovery-design.png
│       └── 05-data-storage-and-database-architecture.png
```

## Skills Demonstrated

* Cloud migration strategy
* AWS architecture design
* Multi-region system design
* Disaster recovery planning
* Cost estimation and ROI analysis
* Microservices decomposition
* Database architecture
* Security and compliance planning
* Event-driven architecture
* Executive technical communication
* Systems thinking and root-cause analysis

## Project Context

This project was completed as an academic cloud computing case study for **CS6620: Fundamentals of Cloud Computing**. It is not a production deployment, but it demonstrates enterprise-level architecture planning, AWS service selection, migration strategy, cost estimation, and executive-ready technical documentation.

## Author

**Harish Nandha Kumar**
M.S. Computer Science, Northeastern University
B.Tech Mechanical Engineering, Vellore Institute of Technology

* LinkedIn: https://www.linkedin.com/in/harish-n-kumar/
* GitHub: https://github.com/HarishNandhaKumar/
* Email: [harishkumar2171@gmail.com](mailto:harishkumar2171@gmail.com)
