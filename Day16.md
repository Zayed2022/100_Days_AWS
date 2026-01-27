# 📘 Day 16: Create an IAM User

**Create an IAM user**

When establishing infrastructure on the AWS cloud, Identity and Access Management (IAM) is among the first and most critical services to configure. IAM facilitates the creation and management of user accounts, groups, roles, policies, and other access controls. The Nautilus DevOps team is currently in the process of configuring these resources and has outlined the following requirements:

For this task, create an IAM user named **iamuser_kareem**.

---

## 🧠 Concept Explanation (Beginner Friendly)

### What is IAM?

**IAM (Identity and Access Management)** controls:

✔ Who can access AWS
✔ What they can access
✔ What actions they can perform

It’s the **security foundation** of AWS.

---

### What is an IAM User?

An **IAM User** represents a **person or application** that logs into AWS.

They can have:

* Console access (username + password)
* Programmatic access (Access keys)
* Permissions via policies

---

### Simple Analogy 🔐

Think of:

* AWS account = Company building
* IAM user = Employee
* IAM policy = Access badge

---

## 🏗️ Why This Task Is Important (WHY)

In real-world AWS:

❌ Never use root account daily
✅ Always use IAM users

IAM helps with:

* Security best practices
* Least privilege
* Auditing
* Team access control

---

## 🧭 What We Are Doing (WHAT)

We will:

1. Go to IAM service
2. Create user named `iamuser_kareem`
3. (No permissions needed unless mentioned)

---

## 🛠️ Solution — Method 1: AWS Console (UI)

### Step 1: Login & Open IAM

* Login to AWS Console
* Search **IAM**
* Click **IAM**

---

### Step 2: Go to Users

* Left menu → **Users**
* Click **Create user**

---

### Step 3: User Details

Enter:

* **User name**:

  ```
  iamuser_kareem
  ```

Leave access type unchecked (unless specified).

Click **Next**

---

### Step 4: Permissions

Since task doesn’t mention any:

👉 Select **Attach policies later**
(or skip permissions)

Click **Next**

---

### Step 5: Review & Create

* Review details
* Click **Create user**

✅ IAM user created.

---

## ⌨️ Solution — Method 2: AWS CLI

### Step 1: Create IAM User

```bash
aws iam create-user \
--user-name iamuser_kareem
```

---

## ✅ Verification Steps

### Console Verification

* IAM → Users
* Confirm user exists:

  ```
  iamuser_kareem
  ```

### CLI Verification

```bash
aws iam list-users
```

---

## 🎯 Common Beginner Mistakes

❌ Creating user in wrong service (EC2 instead of IAM)
❌ Adding permissions when not required
❌ Typos in username
❌ Using root instead of IAM

---

## 💼 Interview & Real-World Insight

**Interview Question:**

> Why should you avoid using root account?

**Answer:**
Root has full access — if compromised, entire AWS account is at risk.

---

## 🧾 Quick Summary (Revision)

* IAM = AWS security service
* User created: `iamuser_kareem`
* No permissions assigned
* Used for secure access control

---
