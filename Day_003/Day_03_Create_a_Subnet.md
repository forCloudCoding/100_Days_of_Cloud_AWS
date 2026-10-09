
# Day 03 : Create a Subnet


## 🎯 Task :
Create a Subnet under default VPC with the following requirements:

Name : ```datacenter-subnet```

#### AWS Menu Navigation :
AWS Console [ Region : N. Virginia ] --> VPC --> Subnets

## 🌍 Real-World Scenario : 3-Tier Web Application

The most common real-world design uses public, private, and isolated subnets to run a secure web application (like an e-commerce website).

* **Public Subnet (Internet Facing) :**
	* **What goes here :** Application Load Balancers (ALBs) or Bastion Hosts.
	* **Why :** Has a route table pointing directly to an Internet Gateway (IGW), allowing public users on the web to access the application entry point.
* **Private Subnet (Application & Database Tier) :**
	* **What goes here :** EC2 web/app servers and Amazon RDS databases.
	* **Why :** No direct inbound access from the internet. They talk to the load balancer internally, but they can still pull software updates or security patches outbound via a NAT Gateway.
* **Isolated Subnet (Maximum Security) :**
	* **What goes here :** Highly sensitive backend databases or financial processing jobs.
	* **Why :** No internet access and no NAT Gateway attached. They only communicate with servers inside the same VPC.

**Key Rules Used in Production**

* **High Availability :** Spread identical subnets across at least two Availability Zones (AZs) so if one data center fails, the application stays online.
* **Security Layering :** Control traffic using stateful Security Groups on individual instances and stateless Network ACLs (NACLs) as a firewall at the subnet boundary.


## ✅ AWS Subnet Best Practices :

Designing subnets in a VPC requires careful planning around sizing, security, and high availability.
Implementing these foundational best practices will help us avoid future network architecture overhauls and ensure enterprise-grade security.


1. **Sizing and IP Address Allocation**

	* **Account for AWS-Reserved IPs :** AWS automatically reserves 5 IP addresses in every subnet (first 4 and last 1). For example, a /28 subnet provides only 11 usable IPs instead of 16.
	* **Size for Growth :** Avoid allocating ultra-small subnets (like /28) for application tiers. Use a minimum of /24 (251 usable IPs) for general resource tiers so we don't run out of space during auto-scaling events.
	* **Prevent CIDR Overlap :** Ensure your VPC and subnet ranges do not overlap with other corporate VPCs or our on-premises data centers. This is critical for future AWS Transit Gateway or VPN integrations.
	* **Understand IPv6 Mechanics :** If we use IPv6, AWS subnets are always /64 by default.

2. **High Availability (Multi-AZ Architecture)**

	* **Span Multiple AZs :** Subnets are scoped to a single AZ. To build a fault-tolerant system, we must deploy duplicate subnets across at least 2 or 3 AZs within a region.
	* **Align Route Tables Uniformly :** We have to keep the Multi-AZ subnets symmetrical. If we have an App-Private-AZ1 subnet, we should also have an identical App-Private-AZ2 subnet with the same routing logic.

3. **Layered Network Isolation (Multi-Tier Strategy)**

	Separate the workloads into distinct functional tiers using dedicated subnets :

	**Public Subnet :** Has a direct route to an Internet Gateway (IGW). Commonly suitable Resources are ALBs, Bastion hosts, NAT Gateways.\

	**Private Subnet (App) :** No direct IGW route; uses a NAT Gateway for outbound-only internet access. Commonly suitable Resources are EC2 application instances, EKS worker nodes, microservices.\

	**Isolated Subnet (Data) :** No internet routes at all (completely internal). Commonly suitable Resources are RDS databases, ElastiCache clusters.

4. **Security and Traffic Control**

	* **Enforce Least Privilege with Firewalls :**
		* Use Security Groups (stateful) at the resource level (e.g., EC2, ELB) to manage fine-grained access between individual components.
		* Use Network ACLs (NACLs) (stateless) at the subnet boundary as a coarse-grained secondary defense layer to block or allow broad IP blocks.
	* **Leverage VPC Endpoints :** Instead of routing traffic through a NAT Gateway to talk to public AWS services (like Amazon S3 or DynamoDB), use VPC Endpoints (AWS PrivateLink). This keeps traffic entirely within the AWS internal network, lowering data transfer costs and improving security.

5. Monitoring and Governance

	* **Enable VPC Flow Logs :** Activate Flow Logs at the subnet or VPC level. Deliver them to an Amazon S3 bucket or CloudWatch Logs to audit network connection attempts and troubleshoot routing issues.
	* **Consistent Tagging :** Standardize the subnet names (e.g., vpc-prod-private-app-az1). This makes automation, Infrastructure as Code (IaC), and cost allocation tracking seamless as our footprint grows.


## 📚 What is an AWS Subnet ?

An AWS subnet is a logical range of IP addresses inside a VPC that lets us group and isolate our cloud resources.

#### Key Characteristics of Subnets :

* **Single Availability Zone (AZ) :** Each subnet must reside entirely within one AZ and cannot span multiple AZs.
* **IP Address Range :** Defined by a CIDR block (such as 10.0.1.0/24).
* **Reserved IPs :** AWS reserves 5 IP addresses in each IPv4 subnet CIDR block for internal networking and routing.
* **Route Tables :** Every subnet must be associated with a single route table, which dictates where network traffic is directed.

#### Main Types of Subnets :

* **Public Subnet :** Connected to a route table that routes traffic to an Internet Gateway, allowing resources like web servers to access the public internet.
* **Private Subnet :** Does not have a direct route to the internet; outbound traffic to the internet (for updates) is routed securely through a NAT Gateway.
* **Isolated / VPN-only Subnet :** Restricted entirely to internal VPC traffic or connected exclusively via a Site-to-Site VPN without internet routing.
