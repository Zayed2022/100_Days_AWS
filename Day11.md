# 📘 Day 11: Attach Elastic Network Interface (ENI) to EC2 Instance


The Nautilus DevOps team has been creating a couple of services on AWS cloud. They have been breaking down the migration into smaller tasks, allowing for better control, risk mitigation, and optimization of resources throughout the migration process. Recently they came up with requirements mentioned below.

An instance named **xfusion-ec2** and an elastic network interface named **xfusion-eni** already exists in **us-east-1** region.

* Attach the **xfusion-eni** network interface to the **xfusion-ec2** instance.
* Make sure status is **attached** before submitting the task.
* Please make sure instance **initialisation has been completed** before submitting this task.

---

## 🧠 Concept Explanation (Beginner Friendly)

### What is an Elastic Network Interface (ENI)?

An **Elastic Network Interface (ENI)** is a **virtual network card** in AWS.

It provides:

* Private IP address
* MAC address
* Security groups
* Optional public IP

---

### Simple Analogy 🖧

Think of:

* EC2 instance = Computer
* ENI = Network card (LAN/WiFi adapter)

A computer can have:

* One network card (default)
* Multiple network cards (advanced networking)

---

## 🏗️ Why This Task Is Important (WHY)

In real-world AWS environments:

* ENIs allow **multiple IPs per instance**
* Useful for:

  * High availability
  * Network isolation
  * Firewalls & appliances
  * Multi-tier applications

This task teaches:

* Advanced EC2 networking
* Safe attachment of network interfaces
* Dependency checks (instance state)

---

## 🧭 What We Are Doing (WHAT)

We will:

1. Ensure EC2 instance is fully initialized
2. Attach existing ENI to the instance
3. Confirm ENI status is **attached**

📌 ENI and EC2 **must be in the same AZ**.

---

## 🛠️ Solution — Method 1: AWS Console (UI)

### Step 1: Login & Check Region

* Login to AWS Console
* Confirm region is **us-east-1 (N. Virginia)**

---

### Step 2: Verify Instance Status

* Go to **EC2 → Instances**
* Select:

  ```
  xfusion-ec2
  ```
* Ensure:

  * Instance state: **running**
  * Status checks: **2/2 checks passed**

⚠️ If status is *Initializing*, wait.

---

### Step 3: Go to Network Interfaces

* In EC2 left menu → **Network Interfaces**
* Select:

  ```
  xfusion-eni
  ```

---

### Step 4: Attach Network Interface

1. Click **Actions**
2. Select **Attach**
3. Instance:

   ```
   xfusion-ec2
   ```
4. Device index:

   ```
   1
   ```

   (0 is default ENI, so use 1)
5. Click **Attach**

✅ ENI attached successfully.

---

## ⌨️ Solution — Method 2: AWS CLI

### Step 1: Get Instance ID

```bash
aws ec2 describe-instances \
--filters Name=tag:Name,Values=xfusion-ec2 \
--query "Reservations[0].Instances[0].InstanceId" \
--output text
```

---

### Step 2: Get Network Interface ID

```bash
aws ec2 describe-network-interfaces \
--filters Name=tag:Name,Values=xfusion-eni \
--query "NetworkInterfaces[0].NetworkInterfaceId" \
--output text
```

---

### Step 3: Attach ENI

```bash
aws ec2 attach-network-interface \
--network-interface-id <ENI_ID> \
--instance-id <INSTANCE_ID> \
--device-index 1
```

---

## ✅ Verification Steps

### Console Verification

* EC2 → Network Interfaces
* Select `xfusion-eni`
* Check:

  ```
  Status: in-use
  Attachment: attached
  Instance: xfusion-ec2
  ```

### CLI Verification

```bash
aws ec2 describe-network-interfaces \
--network-interface-ids <ENI_ID> \
--query "NetworkInterfaces[0].Status"
```

Expected output:

```
in-use
```

---

## 🎯 Common Beginner Mistakes

❌ Attaching ENI before instance is ready
❌ Using device index 0
❌ ENI and instance in different AZs
❌ Attaching wrong network interface

---

## 💼 Interview & Real-World Insight

**Interview Question:**

> Why would you use multiple ENIs on one EC2 instance?

**Answer:**
For multi-network architecture, security separation, high availability, and firewall appliances.

---

## 🧾 Quick Summary (Revision)

* ENI = Virtual network card
* ENI: `xfusion-eni`
* Instance: `xfusion-ec2`
* Region: us-east-1
* Status: **attached / in-use**
---
