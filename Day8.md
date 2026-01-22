# 📘 Day 8: Enable Stop Protection for EC2 Instance

## 📝 Task (As Provided – Unchanged)

As part of the migration, there were some components added to the AWS account. Team created one of the EC2 instances where they need to make some changes now.

There is an EC2 instance named **xfusion-ec2** under **us-east-1** region, enable the **stop protection** for this instance.

---

## 🧠 Concept Explanation (Beginner Friendly)

### What is Stop Protection?

**Stop Protection** prevents an EC2 instance from being **stopped accidentally**.

If enabled:

* ❌ You cannot stop the instance from Console or CLI
* ✅ You must disable stop protection first

📌 This is different from **Termination Protection**:

* Stop protection → prevents **Stop**
* Termination protection → prevents **Delete**

---

### Simple Analogy 🚨

Think of it as:

* **Stop protection** = Child lock on a switch
  So no one can turn it OFF accidentally.

---

## 🏗️ Why This Task Is Important (WHY)

In real projects like **Project Nautilus**:

* Critical instances must stay running
* Accidental stop can cause:

  * Application downtime
  * Service outages
  * Business impact

Stop protection helps ensure:

* 🔒 Availability
* ⚙️ Operational safety
* 🧠 Controlled changes

---

## 🧭 What We Are Doing (WHAT)

We will:

1. Identify EC2 instance `xfusion-ec2`
2. Enable **Stop protection**
3. Verify it is enabled

📌 Instance **must be running or stopped** — both are allowed.

---

## 🛠️ Solution — Method 1: AWS Console (UI)

### Step 1: Login to AWS Console

* Login using provided credentials
* Confirm region is **us-east-1 (N. Virginia)**

---

### Step 2: Open EC2 Dashboard

* Search **EC2**
* Click **EC2**
* Go to **Instances**

---

### Step 3: Select the Instance

* Select instance:

  ```
  xfusion-ec2
  ```

---

### Step 4: Enable Stop Protection

1. Click **Actions**
2. Select **Instance settings**
3. Click **Change stop protection**
4. Check **Enable**
5. Click **Save**

✅ Stop protection enabled.

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

### Step 2: Enable Stop Protection

```bash
aws ec2 modify-instance-attribute \
--instance-id <INSTANCE_ID> \
--disable-api-stop false
```

📌 Explanation:

* `false` → means **do NOT disable** stop → hence **enabled**

---

## ✅ Verification Steps

### Console Verification

* Select `xfusion-ec2`
* Go to **Details tab**
* Look for:

  ```
  Stop protection: Enabled
  ```

### CLI Verification

```bash
aws ec2 describe-instances \
--instance-ids <INSTANCE_ID> \
--query "Reservations[*].Instances[*].DisableApiStop.Value"
```

Expected output:

```
false
```

(`false` means stop protection is ON)

---

## 🎯 Common Beginner Mistakes

❌ Confusing stop protection with termination protection
❌ Looking for option under Security instead of Instance settings
❌ Forgetting correct region
❌ Enabling on wrong instance

---

## 💼 Interview & Real-World Insight

**Interview Question:**

> Can you stop an EC2 instance if stop protection is enabled?

**Answer:**
No. You must disable stop protection first.

---

## 🧾 Quick Summary (Revision)

* Instance: `xfusion-ec2`
* Region: us-east-1
* Feature enabled: **Stop protection**
* Prevents accidental stop
* Good for critical workloads

---

