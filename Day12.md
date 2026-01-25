# 📘 Day 12: Attach EBS Volume to EC2 Instance

The Nautilus DevOps team has been creating a couple of services on AWS cloud. They have been breaking down the migration into smaller tasks, allowing for better control, risk mitigation, and optimization of resources throughout the migration process. Recently they came up with requirements mentioned below.

An instance named **devops-ec2** and a volume named **devops-volume** already exists in **us-east-1** region. Attach the **devops-volume** volume to the **devops-ec2** instance, make sure to set the device name to **/dev/sdb** while attaching the volume.

---

## 🧠 Concept Explanation (Beginner Friendly)

### What is an EBS Volume?

An **EBS (Elastic Block Store) volume** is AWS storage that acts like a **hard disk** for EC2.

It can be:

* Created separately
* Attached/detached anytime
* Used for data, logs, databases

---

### What does “attach volume” mean?

It means:
➡️ Connecting an extra disk to your EC2 server

Just like:

* Plugging a new hard drive into your computer.

---

### What is `/dev/sdb`?

This is the **Linux device name**.

Common examples:

* `/dev/sda` → main OS disk
* `/dev/sdb` → second attached disk
* `/dev/sdc` → third disk

---

## 🏗️ Why This Task Is Important (WHY)

In real cloud systems:

* OS disk is small
* Data disks are separate
* Volumes can be:

  * Expanded
  * Replaced
  * Backed up

This allows:

* Better performance
* Easier recovery
* Cost control

---

## 🧭 What We Are Doing (WHAT)

We will:

1. Locate existing EBS volume `devops-volume`
2. Attach it to instance `devops-ec2`
3. Set device name as `/dev/sdb`
4. Confirm volume status is **in-use**

📌 EC2 and volume must be in the **same AZ**.

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
  devops-volume
  ```

---

### Step 3: Attach Volume

1. Click **Actions**
2. Click **Attach volume**
3. Instance:

   ```
   devops-ec2
   ```
4. Device name:

   ```
   /dev/sdb
   ```
5. Click **Attach**

✅ Volume attached.

---

## ⌨️ Solution — Method 2: AWS CLI

### Step 1: Get Volume ID

```bash
aws ec2 describe-volumes \
--filters Name=tag:Name,Values=devops-volume \
--query "Volumes[0].VolumeId" \
--output text
```

---

### Step 2: Get Instance ID

```bash
aws ec2 describe-instances \
--filters Name=tag:Name,Values=devops-ec2 \
--query "Reservations[0].Instances[0].InstanceId" \
--output text
```

---

### Step 3: Attach Volume

```bash
aws ec2 attach-volume \
--volume-id <VOLUME_ID> \
--instance-id <INSTANCE_ID> \
--device /dev/sdb
```

---

## ✅ Verification Steps

### Console Verification

* EC2 → Volumes
* Select `devops-volume`
* Check:

  ```
  State: in-use
  Attached to: devops-ec2
  Device: /dev/sdb
  ```

### CLI Verification

```bash
aws ec2 describe-volumes \
--volume-ids <VOLUME_ID> \
--query "Volumes[0].State"
```

Expected:

```
in-use
```

---

## 🎯 Common Beginner Mistakes

❌ Attaching volume in different AZ
❌ Using wrong device name
❌ Attaching to wrong instance
❌ Forgetting to verify state

---

## 💼 Interview & Real-World Insight

**Interview Question:**

> Can you detach and reattach an EBS volume to another EC2?

**Answer:**
Yes — as long as it’s in the same AZ.

---

## 🧾 Quick Summary (Revision)

* Volume: `devops-volume`
* Instance: `devops-ec2`
* Device: `/dev/sdb`
* Region: us-east-1
* Status: `in-use`

---
