
# Day 02 : Create a Security Group



## 🎯 Task :
Create a Security Group under default VPC with the following requirements:
Name : ```datacenter-sg```\
Description : ```Security Group for Nautilus App Servers```\

In-Bound Traffic : 
  1. Type : ```HTTP``` Port Range : ```80``` Source CIDR Range : ```0.0.0.0/0```
  2. Type : ```SSH``` Port Range : ```22``` Source CIDR Range : ```0.0.0.0/0```


#### AWS Menu Navigation :
AWS Console [ Region : N. Virginia ] --> EC2 --> Network & Security --> Security Groups

## 🌍 Real-World Scenario : A Standard Web Application

Imagine an On-Line Store hosted on AWS. Our Architecture has two layers:
1. ```Web Servers (EC2)``` : Handle public customer traffic (HTTP/HTTPS).
2. ```Database Servers (RDS/EC2)``` : Store sensitive customer data and passwords.
How Security Groups Protect this Setup:
• The Web Security Group:
	• Inbound Rule: Allows ```Port 80 (HTTP)``` and ```Port 443 (HTTPS)``` from anywhere ( ```0.0.0.0/0``` ) so customers can visit our website.
	• Inbound Rule (Admin): Allows ```Port 22 (SSH)``` only from our company's office IP address, preventing hackers from trying to log in from the public internet.
• **The Database Security Group:**
	• Inbound Rule: Blocks all internet traffic. It contains a rule allowing Port ```3306 (MySQL)``` only from the Web Server's Security Group ID (not even from all internal IPs, just the web tier).

#### Why This Matters in Reality :

If a Hacker compromises one of our web servers, the Hacker still cannot access our Database because the Database Security Group explicitly rejects traffic from anywhere except the specific Web Server Security Group.



## ⚠️ Security Compliance Violation : 

Security Group rules which are overly permissive i.e., exposing administrative Ports like ```SSH (Port 22)``` or ```RDP (Port 3389)``` to the entire internet ```(0.0.0.0/0)``` would result in violation of Security Frameworks like **CIS, PCI-DSS, or SOC 2.**

## ⚠️ Common Causes of Security Group Violations :

1. **Unrestricted Access :** Opening sensitive ports ```(TCP/UDP) to 0.0.0.0/0 (IPv4)``` or ```::/0 (IPv6)```.
2. **Default Security Group Misuse :** Using the default VPC security group with modified, open inbound rules.
3. **Stale Rules :** Retaining rules referencing deleted security groups or peer resources that no longer exist.
4. **Over-privileged Load Balancers :** Direct internet access allowed to backend targets bypassing the Application Load Balancer.

## 📚 What is an AWS Security Group ?

A Security Group in AWS is a virtual firewall that controls In-Bound and Out-Bound Traffic for our Cloud Resources, such as Amazon **EC2** instances & others, like Managed Databases ( **RDS & Aurora** ), Load Balancers ( **ALB & NLB** ), Container Services ( **ECS Tasks & EKS Pods** ), Serverless Compute ( **AWS Lambda** ), Storage Services ( **EFS** ).

