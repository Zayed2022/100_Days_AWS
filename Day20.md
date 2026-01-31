# 📘 Day 20: Create IAM Role for EC2 and Attach Policy

**Create an IAM role for EC2 and attach policy**

When establishing infrastructure on the AWS cloud, Identity and Access Management (IAM) is among the first and most critical services to configure. IAM facilitates the creation and management of user accounts, groups, roles, policies, and other access controls. The Nautilus DevOps team is currently in the process of configuring these resources and has outlined the following requirements:

Create an IAM role as below:

1. IAM role name must be **iamrole_javed**.
2. Entity type must be **AWS Service** and use case must be **EC2**.
3. Attach a policy named **iampolicy_javed**.

---

## 🧠 Concept Explanation (Beginner Friendly)

### What is an IAM Role?

An **IAM Role** is a secure way for AWS services (like EC2) to get permissions **without using access keys**.

Unlike IAM users:

* Roles are assumed by services
* Temporary credentials are provided automatically

---

### Why EC2 Needs IAM Roles?

EC2 instances may need to:

* Access S3 buckets
* Read from DynamoDB
* Write logs to CloudWatch

IAM role allows this securely.

---

### Simple Analogy 🎭

Think of:

* IAM Role = Temporary ID badge
* EC2 assumes role → gets permissions → performs tasks → badge expires

Much safer than passwords.

---

## 🏗️ Why This Task Is Important (WHY)

In real AWS:

❌ Never store access keys inside servers
✅ Always use IAM roles

This provides:

* Security
* Automatic rotation
* Least privilege

---

## 🧭 What We Are Doing (WHAT)

We will:

1. Create IAM role for EC2 service
2. Name it `iamrole_javed`
3. Attach existing policy `iampolicy_javed`
4. Verify role creation

---

## 🛠️ Solution — Method 1: AWS Console (UI)

### Step 1: Login & Open IAM

* Login to AWS Console
* Search **IAM**
* Click **IAM**

---

### Step 2: Go to Roles

* Left menu → **Roles**
* Click **Create role**

---

### Step 3: Select Trusted Entity

Choose:

✅ **AWS service**
Use case: **EC2**

Click **Next**

---

### Step 4: Attach Policy

Search:

```
iampolicy_javed
```

Select it ✔

Click **Next**

---

### Step 5: Role Name

Enter:

```
iamrole_javed
```

Click **Create role**

✅ Role created.

---

## ⌨️ Solution — Method 2: AWS CLI

### Step 1: Create Trust Policy for EC2

```bash
cat <<EOF > trust-policy.json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Principal": {
        "Service": "ec2.amazonaws.com"
      },
      "Action": "sts:AssumeRole"
    }
  ]
}
EOF
```

---

### Step 2: Create IAM Role

```bash
aws iam create-role \
--role-name iamrole_javed \
--assume-role-policy-document file://trust-policy.json
```

---

### Step 3: Attach Policy to Role

```bash
aws iam attach-role-policy \
--role-name iamrole_javed \
--policy-arn arn:aws:iam::<ACCOUNT_ID>:policy/iampolicy_javed
```

---

## ✅ Verification Steps

### Console Verification

* IAM → Roles
* Open:

  ```
  iamrole_javed
  ```
* Check attached policy:

  ```
  iampolicy_javed
  ```

### CLI Verification

```bash
aws iam list-attached-role-policies \
--role-name iamrole_javed
```

---

## 🎯 Common Beginner Mistakes

❌ Choosing IAM user instead of AWS service
❌ Forgetting to attach policy
❌ Using wrong service (Lambda instead of EC2)
❌ Typos in role name

---

## 💼 Interview & Real-World Insight

**Interview Question:**

> Why use IAM roles instead of access keys on EC2?

**Answer:**
Roles provide temporary credentials, better security, and automatic rotation.

---

## 🧾 Quick Summary (Revision)

* Role created: `iamrole_javed`
* Trusted service: EC2
* Policy attached: `iampolicy_javed`
* Used for secure EC2 access to AWS services

---


