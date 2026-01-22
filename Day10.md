# 📘 Day 10: Attach Elastic IP to EC2 Instance

## 📝 Task (As Provided – Unchanged)

The Nautilus DevOps team has been creating a couple of services on AWS cloud. They have been breaking down the migration into smaller tasks, allowing for better control, risk mitigation, and optimization of resources throughout the migration process. Recently they came up with requirements mentioned below.

There is an instance named **xfusion-ec2** and an elastic-ip named **xfusion-ec2-eip** in **us-east-1** region. Attach the **xfusion-ec2-eip** elastic-ip to the **xfusion-ec2** instance.

---

## 🧠 Concept Explanation (Beginner Friendly)

### What is an Elastic IP (EIP)?

An **Elastic IP** is a **static public IPv4 address** provided by AWS.

Normally:

* EC2 public IP changes when instance stops/starts

With EIP:

* Public IP **never changes**
* You can move it between instances

---

### Simple Analogy 🌐

Think of:

* Normal public IP → Temporary phone number
* Elastic IP → Permanent phone number

---

## 🏗️ Why This Task Is Important (WHY)

In real applications like **Project Nautilus**:

* DNS records depend on static IPs
* External clients need a fixed endpoint
* High availability requires quick IP reassignment

Elastic IP helps with:

* 🔁 Failover
* 🌍 Public access stability
* 🚀 Production readiness

---

## 🧭 What We Are Doing (WHAT)

We will:

1. Locate the existing Elastic IP
2. Attach it to the EC2 instance `xfusion-ec2`
3. Verify the association

📌 Elastic IP must be **in the same region** as the instance.

---

## 🛠️ Solution — Method 1: AWS Console (UI)

### Step 1: Login to AWS Console

* Login to AWS Console
* Ensure region is **us-east-1 (N. Virginia)**

---

### Step 2: Open EC2 Dashboard

* Search **EC2**
* Click **EC2**

---

### Step 3: Go to Elastic IPs

* Left sidebar → **Network & Security**
* Click **Elastic IPs**

---

### Step 4: Select the Elastic IP

* Select:

  ```
  xfusion-ec2-eip
  ```

---

### Step 5: Associate Elastic IP

1. Click **Actions**
2. Select **Associate Elastic IP address**
3. Resource type: **Instance**
4. Instance:

   ```
   xfusion-ec2
   ```
5. Private IP: (leave default)
6. Click **Associate**

✅ Elastic IP attached successfully.

---

## ⌨️ Solution — Method 2: AWS CLI

### Step 1: Find Allocation ID

```bash
aws ec2 describe-addresses \
--filters Name=tag:Name,Values=xfusion-ec2-eip \
--query "Addresses[0].AllocationId" \
--output text
```

Save the **Allocation ID**.

---

### Step 2: Find Instance ID

```bash
aws ec2 describe-instances \
--filters Name=tag:Name,Values=xfusion-ec2 \
--query "Reservations[0].Instances[0].InstanceId" \
--output text
```

Save the **Instance ID**.

---

### Step 3: Associate Elastic IP

```bash
aws ec2 associate-address \
--instance-id <INSTANCE_ID> \
--allocation-id <ALLOCATION_ID>
```

---

## ✅ Verification Steps

### Console Verification

* EC2 → Elastic IPs
* Check:

  * Elastic IP: `xfusion-ec2-eip`
  * Associated instance: `xfusion-ec2`

### EC2 Instance Check

* EC2 → Instances → xfusion-ec2
* Public IPv4 address should show the Elastic IP

---

## 🎯 Common Beginner Mistakes

❌ Trying to associate EIP in wrong region
❌ Forgetting to allocate EIP first
❌ Confusing Public IP vs Elastic IP
❌ Attaching to wrong instance

---

## 💼 Interview & Real-World Insight

**Interview Question:**

> What happens to an Elastic IP when EC2 stops?

**Answer:**
The EIP remains allocated and can be reattached when the instance starts again.

---

## 🧾 Quick Summary (Revision)

* Elastic IP = static public IP
* EIP: `xfusion-ec2-eip`
* Instance: `xfusion-ec2`
* Region: us-east-1
* Successfully associated

---
