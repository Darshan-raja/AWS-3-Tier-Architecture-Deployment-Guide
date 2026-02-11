# AWS-3-Tier-Architecture-Deployment-Guide
<img width="1627" height="858" alt="Architecture" src="https://github.com/user-attachments/assets/2b197637-d870-4fba-983f-5a3897840141" />

Below is a complete, professional step-by-step implementation guide you can upload
Good — now you need **crisp, one-line definitions + why it is used + what problem it solves**, along with the **creation step reference**.

This is perfect for GitHub README and LinkedIn technical explanation.

---

# 🔷 Core Components – One Line Definitions + Purpose + Problem Solved

---

## 1️⃣ VPC (Virtual Private Cloud)

**Definition (1 line):**
A logically isolated virtual network in AWS where you deploy your resources.

**Why used:**
To control IP addressing, routing, and security boundaries.

**Problem it solves:**
Prevents resources from being exposed to the public internet by default.

**Creation Step:**
VPC → Create VPC → CIDR (10.0.0.0/16)

---

## 2️⃣ Subnet

**Definition (1 line):**
A subnet is a segmented IP range within a VPC deployed inside a single Availability Zone.

**Why used:**
To logically separate public and private resources.

**Problem it solves:**
Controls which resources can access the internet and which remain isolated.

**Creation Step:**
VPC → Subnets → Create → Assign AZ + CIDR

---

### 🔹 Public Subnet

**Definition:**
Subnet that has a route to the Internet Gateway.

**Used for:**
ALB, NAT Gateway, Bastion Host.

**Problem solved:**
Allows controlled internet-facing access.

---

### 🔹 Private Subnet

**Definition:**
Subnet without direct internet route.

**Used for:**
EC2 App servers, RDS databases.

**Problem solved:**
Prevents direct external access (security layer).

---

## 3️⃣ Internet Gateway (IGW)

**Definition:**
A gateway that enables communication between VPC and the internet.

**Why used:**
To allow public subnet resources to access the internet.

**Problem solved:**
Without IGW, VPC cannot communicate externally.

**Creation Step:**
VPC → Internet Gateway → Create → Attach to VPC

---

## 4️⃣ Route Table

**Definition:**
A routing configuration that determines where network traffic is directed.

**Why used:**
To control traffic flow (IGW or NAT).

**Problem solved:**
Defines whether subnet is public or private.

**Creation Step:**
VPC → Route Tables → Add route 0.0.0.0/0

---

## 5️⃣ NAT Gateway

**Definition:**
A managed service that allows private subnet resources to access the internet securely.

**Why used:**
To allow EC2 updates without making them public.

**Problem solved:**
Prevents assigning public IPs to private instances.

**Creation Step:**
VPC → NAT Gateway → Create in Public Subnet + Attach Elastic IP

---

## 6️⃣ Security Group

**Definition:**
A stateful virtual firewall attached to AWS resources.

**Why used:**
To control inbound and outbound traffic.

**Problem solved:**
Prevents unauthorized access at instance level.

**Creation Step:**
EC2 → Security Groups → Add inbound rules

---

## 7️⃣ Application Load Balancer (ALB)

**Definition:**
A Layer 7 load balancer that distributes HTTP/HTTPS traffic across multiple targets.

**Why used:**
To distribute incoming traffic across EC2 instances.

**Problem solved:**
Prevents single point of failure and improves availability.

**Creation Step:**

1. EC2 → Target Groups → Create
2. EC2 → Load Balancer → Create ALB
3. Attach Public Subnets
4. Attach Security Group
5. Forward to Target Group

---

## 8️⃣ Target Group

**Definition:**
A logical group of backend EC2 instances registered behind ALB.

**Why used:**
To route traffic to healthy instances.

**Problem solved:**
Ensures traffic goes only to healthy servers.

**Creation Step:**
EC2 → Target Groups → Register instances

---

## 9️⃣ Availability Zone (AZ)

**Definition:**
A physically separate data center within an AWS Region.

**Why used:**
To increase fault tolerance.

**Problem solved:**
Prevents downtime if one data center fails.

---

## 🔟 VPC Endpoint (S3)

**Definition:**
A private connection between VPC and AWS services without using internet.

**Why used:**
To securely access S3 from private subnet.

**Problem solved:**
Removes need for NAT for S3 access.

**Creation Step:**
VPC → Endpoints → Select S3 → Attach Route Table

---

# 🔥 Full Architecture Flow (Short Version for LinkedIn)

> Internet → IGW → ALB (Public Subnet) → EC2 (Private Subnet) → S3 via VPC Endpoint
> Private EC2 internet access → NAT Gateway

---🔷 Amazon S3 (Simple Storage Service)
1️⃣ Amazon S3

Definition (1 line):
An object storage service designed to store and retrieve unlimited data with high durability and scalability.

Why used:
To store application assets, backups, logs, and static files securely.

Problem it solves:
Eliminates the need to manage storage servers while providing 99.999999999% durability.

Creation Step:

S3 → Create Bucket

Enter globally unique bucket name

Select Region (same as VPC)

Keep Block Public Access Enabled

Click Create

2️⃣ S3 Versioning

Definition:
A feature that keeps multiple versions of an object in a bucket.

Why used:
To recover accidentally deleted or overwritten files.

Problem solved:
Prevents permanent data loss.

Creation Step:
S3 → Bucket → Properties → Versioning → Enable

3️⃣ S3 Default Encryption

Definition:
Automatic encryption of objects stored in S3.

Why used:
To protect sensitive data at rest.

Problem solved:
Prevents unauthorized access to stored data.

Creation Step:
S3 → Bucket → Properties → Default Encryption → Enable (SSE-S3 or SSE-KMS)

4️⃣ IAM Role for S3 Access

Definition:
An AWS identity with permissions allowing EC2 to access S3 securely.

Why used:
To avoid storing access keys inside EC2.

Problem solved:
Improves security by using temporary credentials.

Creation Step:
IAM → Roles → Create Role → Select EC2 → Attach S3 Policy → Attach to EC2

# 💼 Short Professional GitHub Description

> Implemented a highly available multi-AZ AWS architecture using custom VPC, public and private subnets, Internet Gateway, NAT Gateway, Application Load Balancer, EC2 instances, Security Groups, and VPC Endpoint. Designed secure traffic routing and network isolation aligned with AWS Well-Architected best practices.

---

If you want next level:

* Add RDS definition in same format
* Add Auto Scaling Group explanation
* Add interview Q&A (advanced)
* Convert into clean README.md format

Tell me what you want.
