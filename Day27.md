# 📘 AWS Lab Notes — Day 27: Configuring a Public VPC with an EC2 Instance for Internet Access

---

## 📝 Task Details 

The Nautilus DevOps Team has received a request from the Networking Team to set up a new public VPC to support a set of public-facing services. This VPC will host various resources that need to be accessible over the internet. As part of this setup, you need to ensure the VPC has public subnets with automatic IP assignment for resources. Additionally, a new EC2 instance will be launched within this VPC to host public applications that require SSH access. This setup will enable the Networking Team to deploy and manage public-facing applications.

Create a public VPC named devops-pub-vpc, and a subnet named devops-pub-subnet under the same, make sure public IP is being auto assigned to resources under this subnet. Further, create an EC2 instance named devops-pub-ec2 under this VPC with instance type t2.micro. Make sure SSH port 22 is open for this instance and accessible over the internet.

---

# 🎯 WHY This Task Is Important in AWS

VPC is the **network foundation** of AWS infrastructure.

This task teaches:

✅ Network isolation<br>
✅ Public internet connectivity<br>
✅ Subnet configuration<br>
✅ Secure access to cloud servers

Without proper VPC setup:

❌ Instances cannot reach internet<br>
❌ Users cannot access services<br>
❌ Security boundaries fail

Every AWS architecture starts with VPC design.

---

# 📆 WHEN This Is Used in Real Projects

You create public VPC setups when:

* Hosting websites or APIs
* Deploying public applications
* Creating cloud environments
* Designing multi-tier architectures
* Running DevOps infrastructure

Used in nearly **all production systems**.

---

# 🧠 HOW Public VPC Architecture Works

```
User
 ↓
Internet
 ↓
Internet Gateway
 ↓
Route Table
 ↓
Public Subnet
 ↓
Security Group
 ↓
EC2 Instance
```

For public access, ALL components must exist:

✔ Internet Gateway<br>
✔ Route to IGW<br>
✔ Public IP<br>
✔ Security Group rule

---

# 🚀 Step-by-Step Implementation (Console)

---

# ✅ Step 1 — Create VPC

VPC → Create VPC

| Setting | Value          |
| ------- | -------------- |
| Name    | devops-pub-vpc |
| CIDR    | 10.0.0.0/16    |

Create.

---

# ✅ Step 2 — Create Subnet

VPC → Subnets → Create

| Setting | Value             |
| ------- | ----------------- |
| Name    | devops-pub-subnet |
| VPC     | devops-pub-vpc    |
| CIDR    | 10.0.1.0/24       |

Create.

---

# ✅ Step 3 — Enable Auto Public IP

Select subnet → Edit settings

Enable:

```
Auto-assign public IPv4 address
```

This is critical.

---

# ✅ Step 4 — Create Internet Gateway

VPC → Internet Gateways → Create

Name:

```
devops-igw
```

Attach to:

```
devops-pub-vpc
```

---

# ✅ Step 5 — Configure Route Table

VPC → Route Tables → Edit routes

Add:

```
0.0.0.0/0 → Internet Gateway
```

Associate with:

```
devops-pub-subnet
```

Subnet becomes public.

---

# ✅ Step 6 — Launch EC2 Instance

EC2 → Launch Instance

| Setting       | Value                |
| ------------- | -------------------- |
| Name          | devops-pub-ec2       |
| VPC           | devops-pub-vpc       |
| Subnet        | devops-pub-subnet    |
| Instance Type | t2.micro             |
| AMI           | Any Linux            |
| Key Pair      | Create or select key |

---

# ✅ Step 7 — Configure Security Group

Inbound:

| Type | Port | Source    |
| ---- | ---- | --------- |
| SSH  | 22   | 0.0.0.0/0 |

Launch instance.

---

# ✅ Step 8 — Connect via SSH

Example:

```bash
ssh -i key.pem ec2-user@PUBLIC_IP
```

(or ubuntu user)

---

# ✅ Verification Checklist

✔ Public IP assigned
✔ Route table correct
✔ IGW attached
✔ SSH rule open
✔ Login successful

---

# 💡 Best Practices (Real-World)

✅ Restrict SSH to your IP (not 0.0.0.0/0)<br>
✅ Use Bastion host in production<br>
✅ Use private subnets for databases<br>
✅ Plan CIDR ranges carefully<br>
✅ Use multiple AZs

---

# ⚠️ Common Pitfalls

❌ Forgetting Internet Gateway<br>
❌ Missing route to IGW<br>
❌ Public IP disabled<br>
❌ No key pair selected<br>
❌ Security group blocking SSH

These are the most common networking mistakes.

---

# 🔗 Broader AWS Concepts Connected

This lab connects to:

* Cloud networking architecture
* Security design
* High availability
* Multi-tier applications
* Hybrid cloud networking

VPC knowledge is critical for AWS certifications.

---

# 🎤 Interview Questions & Answers

---

## Q1: What makes a subnet public?

A subnet is public if:

✔ Route to Internet Gateway exists
✔ Instance has public IP

---

## Q2: Difference between public and private subnet?

| Public          | Private            |
| --------------- | ------------------ |
| Internet access | No direct internet |

---

## Q3: Why is Internet Gateway needed?

To allow communication between VPC and internet.

---

## Q4: Can EC2 access internet without public IP?

Yes, via NAT Gateway (for private subnets).

---

## Q5: What happens if you launch EC2 without key pair?

You cannot SSH into it unless recovery methods are used.

---

# 📊 Real-World Architecture Example

```
Public Subnet:
   Load Balancer
   Bastion Host

Private Subnet:
   App Servers
   Database
```

Your lab is the first step toward this architecture.

---

# 📌 Quick Revision Summary

```
Create VPC
→ Create Subnet
→ Enable Public IP
→ Attach IGW
→ Add Route
→ Launch EC2 with Key
→ Allow SSH
```

---
