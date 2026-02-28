Provide your solution here:

## Overview:
## Built on AWS, 500 requests/second with a p99 response time of under 100ms.

3 microservices running on EKS:
Order Service: handles buy/sell orders
Market Data Service: handles live prices
User Service: handles authentication and accounts.

## Services Used

**Route53**
DNS service that routes users to the nearest endpoint.
Alternative considered: Cloudflare DNS

**CloudFront**
CDN that caches static assets like JS and images close to users.
This reduces load on our servers and improves response time.
Alternative considered: Cloudflare CDN

**ALB (Application Load Balancer)**
Distributes incoming traffic across EKS pods.
Ensures no single pod gets overwhelmed.
Alternative considered: Load balancer on EC2

**EKS (Elastic Kubernetes Service)**
Runs our 3 microservices in containers.
Automatically scales pods up/down based on traffic.
Alternative considered: none	

**ElastiCache (Redis)**
Caches live market prices so we dont hit the database every time.
Critical for achieving <100ms response time.

**RDS (PostgreSQL)**
Stores all persistent data - users, orders, trades.
Multi-AZ deployment for high availability.
Alternative considered: DynamoDB

**SQS**
Queues orders for processing so no orders are lost during traffic spikes.

## Scaling Plan - Bro, this is exactly why im all in KB8s cka cks certs on the wway!

**Current Setup**
EKS with minimum 2 pods per service
RDS Multi-AZ with read replicas
ElastiCache cluster mode

**When traffic grows beyond 500 req/sec**
EKS Horizontal Pod Autoscaler (HPA) automatically adds more pods
EKS Cluster Autoscaler automatically adds more EC2 nodes
RDS read replicas handle increased read traffic
ElastiCache scales horizontally with more nodes

**When product grows globally**
Deploy to multiple AWS regions
Route53 latency based routing sends users to nearest region
CloudFront caches content globally
RDS Global Database replicates across regions

**Cost optimization**
Use Spot instances for non-critical workloads
Auto Scaling scales down during low traffic periods
CloudFront reduces load on origin servers
