# AWS-simple-web-application-infrastructure-Deployment-Guide
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


---

Output:
<img width="1885" height="809" alt="vpc1" src="https://github.com/user-attachments/assets/386d8ec5-d207-4404-8c80-dfe98aa38660" />
-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
<img width="1809" height="648" alt="nat" src="https://github.com/user-attachments/assets/319b342f-31e5-4209-8641-4ee7d17c2e23" />
-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
<img width="1903" height="808" alt="security group" src="https://github.com/user-attachments/assets/83c0a326-514c-437c-aab3-3f2a9214e074" />
-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
<img width="1890" height="802" alt="ec2instance 3" src="https://github.com/user-attachments/assets/86c13c36-6002-458d-87b1-3cffb88c8292" />
-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
<img width="935" height="196" alt="instance4" src="https://github.com/user-attachments/assets/5c79d8f1-775b-4f3f-b685-9054ccec8f46" />
-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
<img width="1906" height="765" alt="iam5" src="https://github.com/user-attachments/assets/e4c1189d-e7f2-43d2-9db8-3f2fc864ddf9" />
-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
<img width="1886" height="870" alt="load6" src="https://github.com/user-attachments/assets/664b021e-1d6e-4d26-ae06-92ecbb8a41ff" />
-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
<img width="1885" height="872" alt="webhealt7" src="https://github.com/user-attachments/assets/6b2a70f1-0e4d-4bf9-9797-8da9dc5ce89d" />
-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
<img width="1895" height="867" alt="s3" src="https://github.com/user-attachments/assets/6d55edc5-8925-4e6a-b633-0f64b503afa9" />
-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
<img width="1900" height="905" alt="output1" src="https://github.com/user-attachments/assets/f92e0cfe-732c-416a-8530-5e1ddac1bfce" />
-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
<img width="1910" height="1020" alt="outerror" src="https://github.com/user-attachments/assets/c51cbd3c-9e84-41ad-9f00-e63d20072921" />
-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
<img width="1918" height="906" alt="output" src="https://github.com/user-attachments/assets/0a27533c-dde9-473f-8c85-84834f4d7c52" />
-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------


