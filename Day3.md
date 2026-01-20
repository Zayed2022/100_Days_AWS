# 📘 Day 3: Create a Subnet

**Day 3: Create a Subnet**

The Nautilus DevOps team is strategizing the migration of a portion of their infrastructure to the AWS cloud. Recognizing the scale of this undertaking, they have opted to approach the migration in incremental steps rather than as a single massive transition.

For this task, create one subnet named **nautilus-subnet** under **default VPC**.

Use below given AWS Credentials:
(You can run the showcreds command on aws-client host to retrieve these credentials)


**Notes:**

* Create the resources only in **us-east-1** region.
* To display or hide the terminal of the AWS client machine, you can use the expand toggle button.

---

## 🧠 Concept Explanation (Beginner Friendly)

### What is a Subnet?

A **Subnet** is a **logical subdivision of a VPC**.

Think of:

* **VPC** = Big land (city)
* **Subnet** = Area/sector inside the city
* **EC2 instances** = Houses built inside that area

---

### Key Points You Must Know

* Subnets belong to **exactly one VPC**
* A subnet is created in **one Availability Zone (AZ)**
* CIDR block of subnet must be **inside VPC CIDR**

📌 Default VPC usually has CIDR:

```
172.31.0.0/16
```

---

## 🏗️ Why This Task Is Important (WHY)

In **Project Nautilus**:

* EC2 App Servers will live inside subnets
* Load Balancer targets subnets
* Proper network isolation is required

This is the **foundation step** before:

* Launching EC2
* Attaching Load Balancers
* Setting up routing

---

## 🧭 What We Are Doing (WHAT)

We will:

1. Use **default VPC**
2. Create **one subnet**
3. Name it **nautilus-subnet**
4. Place it in **us-east-1 AZ**

---

## 🛠️ Solution — Method 1: AWS Console (UI)

### Step 1: Login to AWS Console

* Open the **Console URL**
* Login using provided credentials
* Confirm region is **us-east-1 (N. Virginia)**

---

### Step 2: Open VPC Dashboard

* Search **VPC** in AWS search bar
* Click **VPC**

---

### Step 3: Go to Subnets

* Left menu → **Subnets**
* Click **Create subnet**

---

### Step 4: Subnet Configuration

#### VPC Selection

* VPC: **Default VPC**

#### Subnet Settings

* **Subnet name**:

  ```
  nautilus-subnet
  ```

* **Availability Zone**:
  Choose any (e.g., `us-east-1a`)

* **IPv4 CIDR block**:
  Example:

  ```
  172.31.50.0/24
  ```

📌 Any valid /24 inside default VPC is fine.

---

### Step 5: Create Subnet

* Click **Create subnet**

✅ Subnet created successfully.

---

## ⌨️ Solution — Method 2: AWS CLI

### Step 1: Open AWS Client Terminal

Click the **expand toggle button**.

Run:

```bash
showcreds
```

Configure AWS:

```bash
aws configure
```

* Region: `us-east-1`
* Output format: `json`

---

### Step 2: Get Default VPC ID

```bash
aws ec2 describe-vpcs \
--filters Name=isDefault,Values=true \
--query "Vpcs[0].VpcId" \
--output text
```

Save the **VPC ID**.

---

### Step 3: Create Subnet

```bash
aws ec2 create-subnet \
--vpc-id <VPC_ID> \
--cidr-block 172.31.50.0/24 \
--availability-zone us-east-1a \
--tag-specifications 'ResourceType=subnet,Tags=[{Key=Name,Value=nautilus-subnet}]'
```

---

## ✅ Verification Steps

### Console Verification

* VPC → Subnets
* Check:

  * Name: `nautilus-subnet`
  * VPC: Default
  * AZ: us-east-1x

### CLI Verification

```bash
aws ec2 describe-subnets \
--filters Name=tag:Name,Values=nautilus-subnet
```

---

## 🎯 Common Beginner Mistakes

❌ Creating subnet in wrong region
❌ CIDR outside VPC range
❌ Forgetting to name subnet
❌ Confusing VPC vs Subnet

---

## 💼 Interview & Real-World Insight

**Interview Question:**

> Can a subnet span multiple AZs?

**Answer:**
❌ No. A subnet belongs to **one AZ only**.

---

## 🧾 Quick Summary

* Subnet = network segment inside VPC
* Name: `nautilus-subnet`
* VPC: Default
* Region: us-east-1
* CIDR: /24 inside VPC

---
