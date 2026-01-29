# 📘 Day 18: Create an IAM Policy (Read-Only EC2 Access)



When establishing infrastructure on the AWS cloud, Identity and Access Management (IAM) is among the first and most critical services to configure. IAM facilitates the creation and management of user accounts, groups, roles, policies, and other access controls. The Nautilus DevOps team is currently in the process of configuring these resources and has outlined the following requirements.

Create an IAM policy named **iampolicy_rose** in **us-east-1** region, it must allow **read-only access to the EC2 console**, i.e this policy must allow users to **view all instances, AMIs, and snapshots** in the Amazon EC2 console.

---

## 🧠 Concept Explanation (Beginner Friendly)

### What is an IAM Policy?

An **IAM Policy** is a JSON document that defines:

✔ What actions are allowed/denied
✔ On which AWS services/resources

It is the **permission rulebook** in AWS.

---

### Read-Only EC2 Access Means:

Users can:

👀 View:

* EC2 instances
* AMIs
* Snapshots

❌ But cannot:

* Create
* Modify
* Delete

---

### Simple Analogy 📜

Think of:

* IAM Policy = Office rule sheet
* “You can view files but not edit them”

---

## 🏗️ Why This Task Is Important (WHY)

In real projects:

* Developers often need view access only
* Auditors need read-only visibility
* Security follows **least privilege principle**

This prevents:

* Accidental deletion
* Unauthorized changes

---

## 🧭 What We Are Doing (WHAT)

We will:

1. Create a custom IAM policy
2. Allow EC2 describe (read) actions
3. Name it `iampolicy_rose`

---

## 🛠️ Solution — Method 1: AWS Console (UI)

### Step 1: Login & Open IAM

* Login to AWS Console
* Search **IAM**
* Click **IAM**

---

### Step 2: Go to Policies

* Left menu → **Policies**
* Click **Create policy**

---

### Step 3: Choose JSON Editor

* Click **JSON**
* Delete existing content
* Paste this 👇

```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Action": [
        "ec2:DescribeInstances",
        "ec2:DescribeImages",
        "ec2:DescribeSnapshots",
        "ec2:DescribeVolumes",
        "ec2:DescribeTags"
      ],
      "Resource": "*"
    }
  ]
}
```

Click **Next**

---

### Step 4: Policy Details

* **Policy name**:

```
iampolicy_rose
```

(Optional description)

Click **Create policy**

✅ Policy created.

---

## ⌨️ Solution — Method 2: AWS CLI

### Step 1: Create Policy File

```bash
cat <<EOF > ec2-readonly.json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Action": [
        "ec2:DescribeInstances",
        "ec2:DescribeImages",
        "ec2:DescribeSnapshots",
        "ec2:DescribeVolumes",
        "ec2:DescribeTags"
      ],
      "Resource": "*"
    }
  ]
}
EOF
```

---

### Step 2: Create IAM Policy

```bash
aws iam create-policy \
--policy-name iampolicy_rose \
--policy-document file://ec2-readonly.json
```

---

## ✅ Verification Steps

### Console Verification

* IAM → Policies
* Search:

```
iampolicy_rose
```

### CLI Verification

```bash
aws iam list-policies --scope Local
```

---

## 🎯 Common Beginner Mistakes

❌ Using write permissions instead of read
❌ JSON syntax errors
❌ Forgetting policy name
❌ Attaching before verifying

---

## 💼 Interview & Real-World Insight

**Interview Question:**

> What does `Describe` mean in AWS IAM?

**Answer:**
It allows viewing/listing resources without modifying them.

---

## 🧾 Quick Summary (Revision)

* Policy name: `iampolicy_rose`
* Access: EC2 read-only
* Actions: Describe instances, AMIs, snapshots
* Region: us-east-1
* Used for least privilege access

---
