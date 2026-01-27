# 📘 Day 15: Create Snapshot of EBS Volume

The Nautilus DevOps team has some volumes in different regions in their AWS account. They are going to setup some automated backups so that all important data can be backed up on regular basis. For now they shared some requirements to take a snapshot of one of the volumes they have.

Create a snapshot of an existing volume named **devops-vol** in **us-east-1** region.

1. The name of the snapshot must be **devops-vol-ss**.
2. The description must be **devops Snapshot**.
3. Make sure the snapshot status is **completed** before submitting the task.

---

## 🧠 Concept Explanation (Beginner Friendly)

### What is an EBS Snapshot?

An **EBS snapshot** is a **backup of an EBS volume** stored in Amazon S3.

It captures:

* All data on the disk at that time
* Can be used to:

  * Restore volume
  * Create new volumes
  * Disaster recovery

---

### Simple Analogy 💾

Think of:

* Volume = Hard disk
* Snapshot = Full disk backup image

---

## 🏗️ Why This Task Is Important (WHY)

In real production systems:

* Backups are mandatory
* Hardware failure happens
* Human mistakes happen

Snapshots help with:

* 🔄 Recovery
* 🔐 Data safety
* 📦 Migration
* 📈 Scaling

---

## 🧭 What We Are Doing (WHAT)

We will:

1. Locate volume `devops-vol`
2. Create a snapshot from it
3. Name it `devops-vol-ss`
4. Add description
5. Wait until status becomes **completed**

---

## 🛠️ Solution — Method 1: AWS Console (UI)

### Step 1: Login & Region Check

* Login to AWS Console
* Ensure region is **us-east-1**

---

### Step 2: Go to Volumes

* Open **EC2**
* Left menu → **Elastic Block Store → Volumes**
* Select:

  ```
  devops-vol
  ```

---

### Step 3: Create Snapshot

1. Click **Actions**
2. Click **Create snapshot**

---

### Step 4: Snapshot Details

Fill:

* **Description**:

  ```
  devops Snapshot
  ```

* Under **Tags**:

  * Key: `Name`
  * Value:

    ```
    devops-vol-ss
    ```

Click **Create snapshot**

---

### Step 5: Wait for Completion

* Go to **EC2 → Snapshots**
* Find:

  ```
  devops-vol-ss
  ```
* Wait until:

  ```
  Status = completed
  ```

✅ Snapshot ready.

---

## ⌨️ Solution — Method 2: AWS CLI

### Step 1: Get Volume ID

```bash
aws ec2 describe-volumes \
--filters Name=tag:Name,Values=devops-vol \
--query "Volumes[0].VolumeId" \
--output text
```

---

### Step 2: Create Snapshot

```bash
aws ec2 create-snapshot \
--volume-id <VOLUME_ID> \
--description "devops Snapshot" \
--tag-specifications 'ResourceType=snapshot,Tags=[{Key=Name,Value=devops-vol-ss}]'
```

---

### Step 3: Wait for Snapshot Completion

```bash
aws ec2 wait snapshot-completed \
--snapshot-ids <SNAPSHOT_ID>
```

---

## ✅ Verification Steps

### Console Verification

* EC2 → Snapshots
* Check:

  * Name: `devops-vol-ss`
  * Description: `devops Snapshot`
  * Status: `completed`

### CLI Verification

```bash
aws ec2 describe-snapshots \
--snapshot-ids <SNAPSHOT_ID> \
--query "Snapshots[0].State"
```

Expected:

```
completed
```

---

## 🎯 Common Beginner Mistakes

❌ Not waiting for snapshot completion
❌ Forgetting Name tag
❌ Snapshotting wrong volume
❌ Using wrong region

---

## 💼 Interview & Real-World Insight

**Interview Question:**

> Are EBS snapshots incremental?

**Answer:**
Yes. After first full snapshot, only changed blocks are saved.

---

## 🧾 Quick Summary (Revision)

* Volume: `devops-vol`
* Snapshot name: `devops-vol-ss`
* Description: `devops Snapshot`
* Region: us-east-1
* Status: `completed`
* Used for backups & recovery

---
