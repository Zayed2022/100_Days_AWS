# 📘 Day 21: Setting Up an EC2 Instance with an Elastic IP for Application Hosting

The Nautilus DevOps Team has received a new request from the Development Team to set up a new EC2 instance. This instance will be used to host a new application that requires a stable IP address. To ensure that the instance has a consistent public IP, an Elastic IP address needs to be associated with it. The instance will be named **devops-ec2**, and the Elastic IP will be named **devops-eip**. This setup will help the Development Team to have a reliable and consistent access point for their application.

Create an EC2 instance named **devops-ec2** using any linux AMI like **Ubuntu**, the Instance type must be **t2.micro** and associate an Elastic IP address with this instance, name it as **devops-eip**.

---

## 🧠 Concept Explanation (Beginner Friendly)

### What is EC2?

EC2 is a **virtual server in the cloud** where applications run.

---

### What is an Elastic IP?

An **Elastic IP (EIP)** is a **permanent public IP address** in AWS.

Normal EC2 IP:
❌ Changes when restarted

Elastic IP:
✅ Stays forever

---

### Simple Analogy 🌐

* Normal IP = Temporary phone number
* Elastic IP = Permanent phone number

---

## 🏗️ Why This Task Is Important (WHY)

Real applications need:

* Stable access (DNS, users, APIs)
* No IP change during restart
* High availability setups

Elastic IP ensures:
✔ Reliability
✔ Production readiness

---

## 🧭 What We Are Doing (WHAT)

We will:

1. Launch EC2 instance `devops-ec2`
2. Use Ubuntu Linux AMI
3. Instance type `t2.micro`
4. Allocate Elastic IP
5. Name it `devops-eip`
6. Attach it to EC2 instance

---

# 🛠️ Method 1 — AWS Console (UI)

## Step 1: Launch EC2 Instance

### Go to:

EC2 → Launch instance

### Set:

**Name:**

```
devops-ec2
```

**AMI:**
Select **Ubuntu Linux**

**Instance type:**

```
t2.micro
```

**Key pair:**
Create or select any (for lab purpose)

**Security group:**
Default is fine

Click **Launch instance**

---

## Step 2: Allocate Elastic IP

Go to:
EC2 → Elastic IPs → Allocate Elastic IP

Click **Allocate**

---

## Step 3: Tag Elastic IP

Select new EIP → Actions → Manage tags

Add:

Key: `Name`
Value:

```
devops-eip
```

Save.

---

## Step 4: Associate Elastic IP

Select `devops-eip`

Actions → Associate Elastic IP

Choose:

Instance:

```
devops-ec2
```

Click **Associate**

✅ Done!

---

# ⌨️ Method 2 — AWS CLI

### Step 1: Launch EC2 Instance

(First get Ubuntu AMI)

```bash
aws ec2 describe-images \
--owners 099720109477 \
--filters "Name=name,Values=ubuntu/images/hvm-ssd/ubuntu-focal-20.04-amd64-server-*" \
--query "Images[0].ImageId" \
--output text
```

Launch instance:

```bash
aws ec2 run-instances \
--image-id <AMI_ID> \
--instance-type t2.micro \
--tag-specifications 'ResourceType=instance,Tags=[{Key=Name,Value=devops-ec2}]'
```

---

### Step 2: Allocate Elastic IP

```bash
aws ec2 allocate-address --domain vpc
```

Save **AllocationId**

---

### Step 3: Tag EIP

```bash
aws ec2 create-tags \
--resources <ALLOCATION_ID> \
--tags Key=Name,Value=devops-eip
```

---

### Step 4: Associate EIP

```bash
aws ec2 associate-address \
--instance-id <INSTANCE_ID> \
--allocation-id <ALLOCATION_ID>
```

---

## ✅ Verification Steps

### Console

* EC2 → Instances → devops-ec2

* Public IPv4 should show Elastic IP

* EC2 → Elastic IPs → devops-eip

* Associated instance: devops-ec2

---

## 🎯 Common Beginner Mistakes

❌ Forgetting to associate EIP
❌ Confusing public IP with Elastic IP
❌ Launching wrong instance type
❌ Not tagging EIP

---

## 💼 Interview & Real-World Insight

**Interview Question:**

> Why not use normal public IP instead of Elastic IP?

**Answer:**
Normal public IP changes on stop/start; Elastic IP remains static.

---

## 🧾 Quick Summary

* EC2 created: `devops-ec2`
* AMI: Ubuntu Linux
* Type: `t2.micro`
* Elastic IP created: `devops-eip`
* EIP attached successfully

---


