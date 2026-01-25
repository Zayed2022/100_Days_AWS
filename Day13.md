# 📘 Day 13: Create AMI from EC2 Instance

The Nautilus DevOps team is strategizing the migration of a portion of their infrastructure to the AWS cloud. Recognizing the scale of this undertaking, they have opted to approach the migration in incremental steps rather than as a single massive transition. To achieve this, they have segmented large tasks into smaller, more manageable units. This granular approach enables the team to execute the migration in gradual phases, ensuring smoother implementation and minimizing disruption to ongoing operations. By breaking down the migration into smaller tasks, the Nautilus DevOps team can systematically progress through each stage, allowing for better control, risk mitigation, and optimization of resources throughout the migration process.

For this task, create an AMI from an existing EC2 instance named **datacenter-ec2** with the following requirement:

* Name of the AMI should be **datacenter-ec2-ami**
* Make sure AMI is in **available** state

---

## 🧠 Concept Explanation (Beginner Friendly)

### What is an AMI?

An **AMI (Amazon Machine Image)** is a **snapshot/template of an EC2 instance**.

It contains:

* Operating system
* Installed software
* Configurations
* Attached volumes (by default)

---

### Simple Analogy 📸

Think of:

* AMI = **Full system backup image**
* You can use it to:

  * Clone servers
  * Launch identical instances
  * Recover systems quickly

---

## 🏗️ Why This Task Is Important (WHY)

In real DevOps workflows:

* AMIs are used for:

  * Auto Scaling
  * Disaster recovery
  * Fast deployments
  * Immutable infrastructure

Instead of configuring servers again:
👉 Launch from AMI in seconds.

---

## 🧭 What We Are Doing (WHAT)

We will:

1. Locate EC2 instance `datacenter-ec2`
2. Create an AMI from it
3. Name it `datacenter-ec2-ami`
4. Wait until status becomes **available**

📌 During creation, instance may briefly reboot (default behavior).

---

## 🛠️ Solution — Method 1: AWS Console (UI)

### Step 1: Login & Region Check

* Login to AWS Console
* Ensure region is **us-east-1**

---

### Step 2: Go to EC2 Instances

* Open **EC2**
* Click **Instances**
* Select:

  ```
  datacenter-ec2
  ```

---

### Step 3: Create Image (AMI)

1. Click **Actions**
2. Click **Image and templates**
3. Click **Create image**

---

### Step 4: AMI Details

Fill:

* **Image name**:

  ```
  datacenter-ec2-ami
  ```

(Optional: description can be empty)

Leave other settings default.

Click **Create image**

---

### Step 5: Wait for AMI to Become Available

* Go to **EC2 → AMIs**
* Find:

  ```
  datacenter-ec2-ami
  ```
* Wait until **Status = available**

✅ AMI created successfully.

---

## ⌨️ Solution — Method 2: AWS CLI

### Step 1: Get Instance ID

```bash
aws ec2 describe-instances \
--filters Name=tag:Name,Values=datacenter-ec2 \
--query "Reservations[0].Instances[0].InstanceId" \
--output text
```

---

### Step 2: Create AMI

```bash
aws ec2 create-image \
--instance-id <INSTANCE_ID> \
--name datacenter-ec2-ami \
--no-reboot
```

📌 `--no-reboot` avoids instance restart (safe for labs).

---

### Step 3: Check AMI Status

```bash
aws ec2 describe-images \
--owners self \
--filters Name=name,Values=datacenter-ec2-ami \
--query "Images[0].State"
```

Wait until output:

```
available
```

---

## ✅ Verification Steps

### Console Verification

* EC2 → AMIs
* Check:

  * Name: `datacenter-ec2-ami`
  * Status: `available`

### CLI Verification

Same command above showing `available`

---

## 🎯 Common Beginner Mistakes

❌ Closing page before AMI finishes creating
❌ Wrong instance selected
❌ Not waiting for available state
❌ Typos in AMI name

---

## 💼 Interview & Real-World Insight

**Interview Question:**

> What’s the difference between AMI and snapshot?

**Answer:**

* Snapshot = backup of a volume
* AMI = complete instance template (uses snapshots internally)

---

## 🧾 Quick Summary (Revision)

* AMI created from `datacenter-ec2`
* Name: `datacenter-ec2-ami`
* Region: us-east-1
* Status: `available`
* Used for cloning & recovery

---

