# 📘 Day 6: Launch an EC2 Instance


**Launch EC2 instance**

The Nautilus DevOps team is strategizing the migration of a portion of their infrastructure to the AWS cloud. Recognizing the scale of this undertaking, they have opted to approach the migration in incremental steps rather than as a single massive transition. To achieve this, they have segmented large tasks into smaller, more manageable units.

For this task, create an EC2 instance with following requirements:

1. The name of the instance must be **datacenter-ec2**.
2. You can use the **Amazon Linux AMI** to launch this instance.
3. The Instance type must be **t2.micro**.
4. Create a new **RSA key pair** named **datacenter-kp**.
5. Attach the **default (available by default) security group**.

---

## 🧠 Concept Explanation (Beginner Friendly)

### What is an EC2 Instance?

An **EC2 instance** is a **virtual server** in AWS.

Think of it as:

* EC2 = A computer in the cloud
* AMI = Operating system (Amazon Linux here)
* Instance type = Hardware configuration (CPU, RAM)
* Key pair = Login key (like a password, but more secure)
* Security group = Firewall

---

### Key Terms You Must Know

* **AMI (Amazon Machine Image)** → OS template
* **t2.micro** → Free-tier eligible instance (1 vCPU, 1 GB RAM)
* **Key Pair (RSA)** → Used to SSH into the instance
* **Security Group** → Controls inbound/outbound traffic

---

## 🏗️ Why This Task Is Important (WHY)

In **Project Nautilus**:

* App servers will be EC2 instances
* Databases, Jenkins, backup servers → all EC2-based
* This is the **core building block** of AWS infrastructure

If you know EC2 well → you understand **70% of AWS basics**.

---

## 🧭 What We Are Doing (WHAT)

We will:

1. Launch an EC2 instance
2. Use Amazon Linux AMI
3. Select t2.micro instance type
4. Create a new RSA key pair
5. Attach default security group
6. Name the instance correctly

---

## 🛠️ Solution — Method 1: AWS Console (UI)

### Step 1: Login to AWS Console

* Login using provided credentials
* Ensure region is **us-east-1 (N. Virginia)**

---

### Step 2: Open EC2 Dashboard

* Search **EC2**
* Click **EC2**
* Click **Launch instance**

---

### Step 3: Name the Instance

At the top:

* **Name**:

  ```
  datacenter-ec2
  ```

---

### Step 4: Choose AMI

* Select **Amazon Linux**
* Amazon Linux 2 / Amazon Linux 2023 (default is fine)

---

### Step 5: Choose Instance Type

* Select:

  ```
  t2.micro
  ```

---

### Step 6: Create Key Pair

* Click **Create new key pair**
* Key pair name:

  ```
  datacenter-kp
  ```
* Key pair type: **RSA**
* File format: `.pem`
* Click **Create key pair**

📌 The key will download automatically — **do not lose it**.

---

### Step 7: Network & Security

* VPC: Default
* Subnet: Default (auto-selected)
* Security Group:

  * Select **default security group**

📌 Do **NOT** create a new SG.

---

### Step 8: Launch Instance

* Review all settings
* Click **Launch instance**

✅ EC2 instance is launched.

---

## ⌨️ Solution — Method 2: AWS CLI

### Step 1: Open AWS Client Terminal

Use the **expand toggle button**.

Run:

```bash
showcreds
```

Configure AWS CLI:

```bash
aws configure
```

* Region: `us-east-1`
* Output format: `json`

---

### Step 2: Create Key Pair

```bash
aws ec2 create-key-pair \
--key-name datacenter-kp \
--key-type rsa \
--query 'KeyMaterial' \
--output text > datacenter-kp.pem
```

Secure the key:

```bash
chmod 400 datacenter-kp.pem
```

---

### Step 3: Get Amazon Linux AMI ID

```bash
aws ec2 describe-images \
--owners amazon \
--filters "Name=name,Values=al2023-ami-*" \
--query "Images[0].ImageId" \
--output text
```

Save the **AMI ID**.

---

### Step 4: Launch EC2 Instance

```bash
aws ec2 run-instances \
--image-id <AMI_ID> \
--instance-type t2.micro \
--key-name datacenter-kp \
--security-groups default \
--tag-specifications 'ResourceType=instance,Tags=[{Key=Name,Value=datacenter-ec2}]'
```

---

## ✅ Verification Steps

### Console Verification

* EC2 → Instances
* Check:

  * Name: `datacenter-ec2`
  * State: `running`
  * Instance type: `t2.micro`
  * Key pair: `datacenter-kp`
  * Security group: `default`

### CLI Verification

```bash
aws ec2 describe-instances \
--filters Name=tag:Name,Values=datacenter-ec2
```

---

## 🎯 Common Beginner Mistakes

❌ Forgetting to name the instance
❌ Selecting wrong instance type
❌ Losing the key pair file
❌ Creating a new security group instead of default

---

## 💼 Interview & Real-World Insight

**Interview Question:**

> Why is key pair required for EC2?

**Answer:**
It provides secure, password-less SSH access using public–private key cryptography.

---

## 🧾 Quick Summary (Revision)

* EC2 instance launched
* Name: `datacenter-ec2`
* AMI: Amazon Linux
* Type: `t2.micro`
* Key pair: `datacenter-kp`
* Security Group: default
* Region: us-east-1

---
