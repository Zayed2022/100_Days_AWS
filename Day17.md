# 📘 Day 17: Create an IAM Group

The Nautilus DevOps team has been creating a couple of services on AWS cloud. They have been breaking down the migration into smaller tasks, allowing for better control, risk mitigation, and optimization of resources throughout the migration process. Recently they came up with requirements mentioned below.

Create an IAM group named **iamgroup_john**.

---

## 🧠 Concept Explanation (Beginner Friendly)

### What is an IAM Group?

An **IAM Group** is a collection of IAM users.

Instead of:

* Assigning permissions to each user individually

You:

* Assign permissions to a group
* Add users to that group

---

### Simple Analogy 👥

Think of:

* IAM User = Employee
* IAM Group = Department (DevOps, Admin, QA)
* Permissions = Department access level

Much easier to manage!

---

## 🏗️ Why This Task Is Important (WHY)

In real AWS environments:

* Teams have many users
* Permissions should be role-based
* Groups simplify management

Benefits:

* Centralized access control
* Fewer mistakes
* Easy onboarding/offboarding

---

## 🧭 What We Are Doing (WHAT)

We will:

1. Open IAM service
2. Create group named `iamgroup_john`
3. (No policies attached unless specified)

---

## 🛠️ Solution — Method 1: AWS Console (UI)

### Step 1: Login & Open IAM

* Login to AWS Console
* Search **IAM**
* Click **IAM**

---

### Step 2: Go to User Groups

* Left menu → **User groups**
* Click **Create group**

---

### Step 3: Group Details

Enter:

* **Group name**:

  ```
  iamgroup_john
  ```

Skip policy attachment (not mentioned).

Click **Create group**

✅ Group created.

---

## ⌨️ Solution — Method 2: AWS CLI

### Step 1: Create IAM Group

```bash
aws iam create-group \
--group-name iamgroup_john
```

---

## ✅ Verification Steps

### Console Verification

* IAM → User groups
* Confirm:

  ```
  iamgroup_john
  ```

### CLI Verification

```bash
aws iam list-groups
```

---

## 🎯 Common Beginner Mistakes

❌ Creating group under wrong service
❌ Assigning policies when not required
❌ Typos in group name
❌ Confusing group with role

---

## 💼 Interview & Real-World Insight

**Interview Question:**

> Why use IAM groups instead of attaching policies directly to users?

**Answer:**
For scalable, manageable, and consistent permission control.

---

## 🧾 Quick Summary (Revision)

* IAM Group created: `iamgroup_john`
* Used for managing multiple users
* No permissions attached
* Improves security & organization

---
