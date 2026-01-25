# 📘 Day 14: Terminate EC2 Instance

During the migration process, several resources were created under the AWS account. Later on, some of these resources became obsolete as alternative solutions were implemented. Similarly, there is an instance that needs to be deleted as it is no longer in use.

1. Delete the ec2 instance named **nautilus-ec2** present in **us-east-1** region.
2. Before submitting your task, make sure instance is in **terminated** state.

---

## 🧠 Concept Explanation (Beginner Friendly)

### What does “Terminate EC2 instance” mean?

**Termination** means:

👉 Permanently deleting the EC2 virtual server.

Once terminated:

* ❌ Data on root disk is lost (unless backed up)
* ❌ Instance cannot be recovered
* ✅ Resources are freed

---

### Simple Analogy 🗑️

Think of:

* Stop = turning off your computer
* Terminate = throwing the computer away

Termination is **final**.

---

## 🏗️ Why This Task Is Important (WHY)

In cloud environments:

* Unused servers = unnecessary cost 💰
* Old resources clutter infrastructure
* Cleanup is part of DevOps responsibility

This ensures:

* Cost optimization
* Clean architecture
* Good cloud hygiene

---

## 🧭 What We Are Doing (WHAT)

We will:

1. Find instance `nautilus-ec2`
2. Terminate it
3. Confirm state is **terminated**

⚠️ If termination protection is enabled → disable first.

---

## 🛠️ Solution — Method 1: AWS Console (UI)

### Step 1: Login & Region Check

* Login to AWS Console
* Ensure region is **us-east-1**

---

### Step 2: Open EC2 Instances

* Go to **EC2**
* Click **Instances**

---

### Step 3: Select Instance

Select:

```
nautilus-ec2
```

---

### Step 4: Terminate Instance

1. Click **Instance state**
2. Click **Terminate instance**
3. Confirm termination

---

### Step 5: Wait for Termination

Watch instance state change:

```
shutting-down → terminated
```

✅ When it shows **terminated**, task is complete.

---

## ⌨️ Solution — Method 2: AWS CLI

### Step 1: Get Instance ID

```bash
aws ec2 describe-instances \
--filters Name=tag:Name,Values=nautilus-ec2 \
--query "Reservations[0].Instances[0].InstanceId" \
--output text
```

---

### Step 2: Terminate Instance

```bash
aws ec2 terminate-instances \
--instance-ids <INSTANCE_ID>
```

---

### Step 3: Wait Until Terminated

```bash
aws ec2 wait instance-terminated \
--instance-ids <INSTANCE_ID>
```

---

## ✅ Verification Steps

### Console Verification

* EC2 → Instances
* Instance should show:

  ```
  State: terminated
  ```

### CLI Verification

```bash
aws ec2 describe-instances \
--instance-ids <INSTANCE_ID> \
--query "Reservations[0].Instances[0].State.Name"
```

Expected:

```
terminated
```

---

## 🎯 Common Beginner Mistakes

❌ Deleting wrong instance
❌ Forgetting region
❌ Confusing stop with terminate
❌ Termination protection enabled

---

## 💼 Interview & Real-World Insight

**Interview Question:**

> Can you recover a terminated EC2 instance?

**Answer:**
No — termination is permanent unless you created an AMI or snapshot earlier.

---

## 🧾 Quick Summary (Revision)

* Instance: `nautilus-ec2`
* Region: us-east-1
* Action: Terminated
* Final state: `terminated`
* Frees AWS resources and cost

---
