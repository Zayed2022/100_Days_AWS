# 📘 Day 7: Change EC2 Instance Type


**Day 7: Change EC2 Type**

During the migration process, the Nautilus DevOps team created several EC2 instances in different regions. They are currently in the process of identifying the correct resources and utilization and are making continuous changes to ensure optimal resource utilization. Recently, they discovered that one of the EC2 instances was underutilized, prompting them to decide to change the instance type. Please make sure the Status check is completed (if its still in Initializing state) before making any changes to the instance.

1. Change the instance type from **t2.micro** to **t2.nano** for **xfusion-ec2** instance.
2. Make sure the ec2 instance **xfusion-ec2** is in **running** state after the change.

---

## 🧠 Concept Explanation (Beginner Friendly)

### What is an EC2 Instance Type?

An **instance type** defines:

* CPU
* Memory (RAM)
* Network performance

Examples:

* `t2.nano` → very small (low cost, low resources)
* `t2.micro` → small (free-tier eligible)

---

### Can We Change Instance Type?

✅ **Yes**, but with conditions:

* The instance must be **stopped**
* After changing, it must be **started again**

📌 This is called **vertical scaling** (scale up or down).

---

## 🏗️ Why This Task Is Important (WHY)

In real DevOps work:

* Over-provisioning = 💸 money waste
* Underutilized servers = opportunity to downsize
* Cloud allows **flexible resizing**, unlike on-prem

This task teaches:

* Cost optimization
* Resource right-sizing
* Safe instance modification

---

## 🧭 What We Are Doing (WHAT)

We will:

1. Check instance **status checks**
2. Stop the EC2 instance
3. Change instance type to **t2.nano**
4. Start the instance again
5. Verify it is **running**

---

## 🛠️ Solution — Method 1: AWS Console (UI)

### Step 1: Login & Open EC2

* Login to AWS Console
* Ensure correct region
* Go to **EC2 → Instances**

---

### Step 2: Check Instance Status

* Locate instance:

  ```
  xfusion-ec2
  ```
* Ensure:

  * **Status check: 2/2 checks passed**

⚠️ If status shows **Initializing**, wait until it completes.

---

### Step 3: Stop the Instance

* Select **xfusion-ec2**
* Click **Instance state → Stop**
* Confirm stop

⏳ Wait until state becomes:

```
stopped
```

---

### Step 4: Change Instance Type

* Select the stopped instance
* Click **Actions → Instance settings → Change instance type**
* Choose:

  ```
  t2.nano
  ```
* Click **Apply**

---

### Step 5: Start the Instance

* Select instance
* Click **Instance state → Start**
* Wait until state becomes:

```
running
```

✅ Console method complete.

---

## ⌨️ Solution — Method 2: AWS CLI

### Step 1: Stop the Instance

```bash
aws ec2 stop-instances \
--instance-ids <INSTANCE_ID>
```

Wait until stopped:

```bash
aws ec2 wait instance-stopped \
--instance-ids <INSTANCE_ID>
```

---

### Step 2: Change Instance Type

```bash
aws ec2 modify-instance-attribute \
--instance-id <INSTANCE_ID> \
--instance-type "{\"Value\":\"t2.nano\"}"
```

---

### Step 3: Start the Instance

```bash
aws ec2 start-instances \
--instance-ids <INSTANCE_ID>
```

Wait until running:

```bash
aws ec2 wait instance-running \
--instance-ids <INSTANCE_ID>
```

---

## ✅ Verification Steps

### Console Verification

* EC2 → Instances
* Check:

  * Name: `xfusion-ec2`
  * Instance type: `t2.nano`
  * State: `running`

### CLI Verification

```bash
aws ec2 describe-instances \
--filters Name=tag:Name,Values=xfusion-ec2 \
--query "Reservations[*].Instances[*].InstanceType"
```

Expected output:

```
t2.nano
```

---

## 🎯 Common Beginner Mistakes

❌ Trying to change instance type while running
❌ Not waiting for status checks
❌ Forgetting to restart the instance
❌ Changing wrong instance

---

## 💼 Interview & Real-World Insight

**Interview Question:**

> What type of scaling is changing EC2 instance type?

**Answer:**
Vertical scaling (scaling up or down).

---

## 🧾 Quick Summary (Revision)

* Instance resized from `t2.micro` → `t2.nano`
* Instance was stopped before modification
* Restarted successfully
* Final state: **running**

---

