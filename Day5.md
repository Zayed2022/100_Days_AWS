# 📘 Day 5: Create a gp3 Volume

## 📝 Task (As Provided – Unchanged)

**Create a gp3 volume**

The Nautilus DevOps team is strategizing the migration of a portion of their infrastructure to the AWS cloud. Recognizing the scale of this undertaking, they have opted to approach the migration in incremental steps rather than as a single massive transition. To achieve this, they have segmented large tasks into smaller, more manageable units. This granular approach enables the team to execute the migration in gradual phases, ensuring smoother implementation and minimizing disruption to ongoing operations. By breaking down the migration into smaller tasks, the Nautilus DevOps team can systematically progress through each stage, allowing for better control, risk mitigation, and optimization of resources throughout the migration process.

Create a volume with the following requirements:

* Name of the volume should be **xfusion-volume**
* Volume type must be **gp3**
* Volume size must be **2 GiB**

---

## 🧠 Concept Explanation (Beginner Friendly)

### What is an EBS Volume?

An **Amazon EBS (Elastic Block Store) volume** is like a **virtual hard disk** for EC2 instances.

Think of it as:

* EC2 = Laptop
* EBS Volume = Hard Disk / SSD attached to the laptop

---

### What is gp3?

**gp3** stands for **General Purpose SSD (3rd generation)**.

Key points:

* Latest and recommended general-purpose volume
* Better performance than gp2
* Cheaper and more flexible
* Used for:

  * OS disks
  * Application data
  * Databases (small/medium)

---

## 🏗️ Why This Task Is Important (WHY)

In **Project Nautilus**:

* App servers need storage
* Databases need disks
* Logs and application data must persist

Creating an EBS volume is a **core AWS skill** before:

* Attaching disks to EC2
* Expanding storage
* Migrating on-prem data to cloud

---

## 🧭 What We Are Doing (WHAT)

We will:

1. Create an **EBS volume**
2. Set volume type to **gp3**
3. Set size to **2 GiB**
4. Name it **xfusion-volume**
5. Keep it in **us-east-1**

📌 Note:
Volumes are created in **one Availability Zone (AZ)**.

---

## 🛠️ Solution — Method 1: AWS Console (UI)

### Step 1: Login to AWS Console

* Login using provided credentials
* Ensure region is **us-east-1 (N. Virginia)**

---

### Step 2: Open EC2 Dashboard

* Search **EC2**
* Click **EC2**

---

### Step 3: Go to Volumes

* Left sidebar → **Elastic Block Store**
* Click **Volumes**
* Click **Create volume**

---

### Step 4: Configure Volume

Fill in the following:

* **Volume type**:

  ```
  gp3
  ```

* **Size (GiB)**:

  ```
  2
  ```

* **Availability Zone**:
  Choose any (e.g., `us-east-1a`)

📌 AZ choice doesn’t matter unless attaching to EC2 later.

---

### Step 5: Add Name Tag

Under **Tags**:

* Key: `Name`
* Value:

  ```
  xfusion-volume
  ```

---

### Step 6: Create Volume

* Click **Create volume**

✅ Volume created successfully.

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

### Step 2: Create gp3 Volume

```bash
aws ec2 create-volume \
--availability-zone us-east-1a \
--size 2 \
--volume-type gp3 \
--tag-specifications 'ResourceType=volume,Tags=[{Key=Name,Value=xfusion-volume}]'
```

---

## ✅ Verification Steps

### Console Verification

* EC2 → Volumes
* Check:

  * Name: `xfusion-volume`
  * Type: `gp3`
  * Size: `2 GiB`
  * State: `available`

### CLI Verification

```bash
aws ec2 describe-volumes \
--filters Name=tag:Name,Values=xfusion-volume
```

---

## 🎯 Common Beginner Mistakes

❌ Creating volume in wrong region
❌ Choosing gp2 instead of gp3
❌ Forgetting to add Name tag
❌ Confusing GiB with GB

---

## 💼 Interview & Real-World Insight

**Interview Question:**

> Difference between gp2 and gp3?

**Answer:**

* gp3 is cheaper
* gp3 allows independent performance tuning
* gp3 is the recommended default now

---

## 🧾 Quick Summary (Revision)

* EBS Volume = Virtual disk
* Name: `xfusion-volume`
* Type: `gp3`
* Size: `2 GiB`
* Region: us-east-1
* State: available

---
