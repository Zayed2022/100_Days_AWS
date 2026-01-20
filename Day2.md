# 📘 Day 2: Create Security Group

## 📝 Task (As Provided – Unchanged)

**Day 2: Create Security Group**

The Nautilus DevOps team is strategizing the migration of a portion of their infrastructure to the AWS cloud. Recognizing the scale of this undertaking, they have opted to approach the migration in incremental steps rather than as a single massive transition. To achieve this, they have segmented large tasks into smaller, more manageable units. This granular approach enables the team to execute the migration in gradual phases, ensuring smoother implementation and minimizing disruption to ongoing operations. By breaking down the migration into smaller tasks, the Nautilus DevOps team can systematically progress through each stage, allowing for better control, risk mitigation, and optimization of resources throughout the migration process.

For this task, create a security group under default VPC with the following requirements:

* Name of the security group is **xfusion-sg**.
* The description must be **Security group for Nautilus App Servers**
* Add the inbound rule of type **HTTP**, with port range of **80**. Enter the source CIDR range of **0.0.0.0/0**.
* Add another inbound rule of type **SSH**, with port range of **22**. Enter the source CIDR range of **0.0.0.0/0**.

Use below given AWS Credentials:
(You can run the showcreds command on aws-client host to retrieve these credentials)

**Notes:**

* Create the resources only in **us-east-1** region.

---

## 🧠 Concept Explanation (Beginner Friendly)

### What is a Security Group?

A **Security Group (SG)** in AWS is like a **virtual firewall** for your resources (EC2, Load Balancer, etc.).

Think of it as:

> 🚪 *A gatekeeper that decides which traffic is allowed IN and OUT of your server.*

### Key Points (Very Important)

* Security Groups are **stateful**

  * If inbound traffic is allowed → outbound response shows automatically allowed
* They work at **instance level**
* They contain **rules**, not deny rules (only allow rules)

---

## 🏗️ Why This Task Is Important (WHY)

In **Project Nautilus architecture**:

* App servers will be hosted on EC2
* Users will access the app using **HTTP (port 80)**
* DevOps engineers need **SSH (port 22)** access for management

So this SG will later be attached to:

* Nautilus App Servers (stapp01, stapp02, stapp03 equivalent in AWS)

---

## 🧭 What We Are Doing (WHAT)

We will:

1. Use the **default VPC**
2. Create a **Security Group**
3. Allow:

   * 🌐 HTTP access from anywhere
   * 🔐 SSH access from anywhere

⚠️ Note:
In real production, SSH from `0.0.0.0/0` is **not recommended**, but KodeKloud uses it for learning.

---

## 🛠️ Solution — Method 1: AWS Console (UI)

### Step 1: Login to AWS Console

* Open the **Console URL**
* Enter **Username & Password**
* Make sure region is **us-east-1 (N. Virginia)** ⬅️ very important

---

### Step 2: Go to EC2 Dashboard

* Search **EC2** in AWS search bar
* Click **EC2**

---

### Step 3: Navigate to Security Groups

* Left sidebar → **Network & Security**
* Click **Security Groups**
* Click **Create security group**

---

### Step 4: Basic Security Group Details

Fill in:

* **Security group name**:

  ```
  xfusion-sg
  ```

* **Description**:

  ```
  Security group for Nautilus App Servers
  ```

* **VPC**:
  Select **default VPC**

---

### Step 5: Add Inbound Rules

Click **Add rule**

#### Rule 1: HTTP

* Type: **HTTP**
* Protocol: TCP (auto)
* Port: **80**
* Source: **Anywhere-IPv4**
* CIDR: `0.0.0.0/0`

#### Rule 2: SSH

* Type: **SSH**
* Protocol: TCP (auto)
* Port: **22**
* Source: **Anywhere-IPv4**
* CIDR: `0.0.0.0/0`

---

### Step 6: Outbound Rules

* Leave **default outbound rule**
* (Allow all traffic)

---

### Step 7: Create Security Group

* Click **Create security group**

✅ Done from Console.

---

## ⌨️ Solution — Method 2: AWS CLI (Hands-on)

### Step 1: Open AWS Client Terminal

Use the **expand toggle button** to open terminal.

Run:

```bash
showcreds
```

Configure AWS CLI:

```bash
aws configure
```

Enter:

* AWS Access Key
* AWS Secret Key
* Region: `us-east-1`
* Output format: `json`

---

### Step 2: Get Default VPC ID

```bash
aws ec2 describe-vpcs \
--filters Name=isDefault,Values=true \
--query "Vpcs[0].VpcId" \
--output text
```

📌 Save this VPC ID.

---

### Step 3: Create Security Group

```bash
aws ec2 create-security-group \
--group-name xfusion-sg \
--description "Security group for Nautilus App Servers" \
--vpc-id <VPC_ID>
```

📌 Note the **GroupId** from output.

---

### Step 4: Add HTTP Rule

```bash
aws ec2 authorize-security-group-ingress \
--group-id <SG_ID> \
--protocol tcp \
--port 80 \
--cidr 0.0.0.0/0
```

---

### Step 5: Add SSH Rule

```bash
aws ec2 authorize-security-group-ingress \
--group-id <SG_ID> \
--protocol tcp \
--port 22 \
--cidr 0.0.0.0/0
```

---

## ✅ Verification Steps

### Console

* EC2 → Security Groups
* Click **xfusion-sg**
* Check:

  * Inbound rules → HTTP (80) & SSH (22)
  * VPC → Default VPC

### CLI

```bash
aws ec2 describe-security-groups \
--group-names xfusion-sg
```

---

## 🎯 Common Beginner Mistakes

❌ Creating SG in wrong region
❌ Forgetting default VPC
❌ Missing inbound rules
❌ Typo in security group name

---

## 💼 Interview & Real-World Insight

**Interview Question:**

> Difference between Security Group and NACL?

**Answer (Short):**

* SG → Stateful, instance-level
* NACL → Stateless, subnet-level

---

## 🧾 Quick Summary (Revision)

* Security Group = Virtual firewall
* Created **xfusion-sg**
* Allowed ports: **80, 22**
* Region: **us-east-1**
* VPC: **default**

---
