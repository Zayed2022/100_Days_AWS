# 📘 Day 19: Attach IAM Policy to IAM User


The Nautilus DevOps team has been creating a couple of services on AWS cloud. They have been breaking down the migration into smaller tasks, allowing for better control, risk mitigation, and optimization of resources throughout the migration process. Recently they came up with requirements mentioned below.

An IAM user named **iamuser_mariyam** and a policy named **iampolicy_mariyam** already exist. Attach the IAM policy **iampolicy_mariyam** to the IAM user **iamuser_mariyam**.

---

## 🧠 Concept Explanation (Beginner Friendly)

### What does “attach policy” mean in IAM?

Attaching a policy means:

👉 Granting permissions defined in that policy to a user.

Without policy:

* User exists
* But has **no access to AWS resources**

With policy:

* User can perform allowed actions

---

### Simple Analogy 🔑

Think of:

* IAM user = Person
* IAM policy = Permission card

Attach card → person gets access.

---

## 🏗️ Why This Task Is Important (WHY)

In real DevOps:

* Users never get direct access by default
* Permissions are controlled by policies
* Helps enforce least privilege

This ensures:

* Security
* Compliance
* Controlled access

---

## 🧭 What We Are Doing (WHAT)

We will:

1. Open IAM service
2. Select user `iamuser_mariyam`
3. Attach policy `iampolicy_mariyam`
4. Verify attachment

---

## 🛠️ Solution — Method 1: AWS Console (UI)

### Step 1: Login & Open IAM

* Login to AWS Console
* Search **IAM**
* Click **IAM**

---

### Step 2: Go to Users

* Left menu → **Users**
* Click:

  ```
  iamuser_mariyam
  ```

---

### Step 3: Attach Policy

1. Go to **Permissions** tab
2. Click **Add permissions**
3. Choose **Attach policies directly**
4. Search:

   ```
   iampolicy_mariyam
   ```
5. Select the policy
6. Click **Next → Add permissions**

✅ Policy attached.

---

## ⌨️ Solution — Method 2: AWS CLI

### Step 1: Attach Policy to User

```bash
aws iam attach-user-policy \
--user-name iamuser_mariyam \
--policy-arn arn:aws:iam::<ACCOUNT_ID>:policy/iampolicy_mariyam
```

📌 Replace `<ACCOUNT_ID>` with your AWS account number.

---

## ✅ Verification Steps

### Console Verification

* IAM → Users → iamuser_mariyam
* Permissions tab should show:

  ```
  iampolicy_mariyam
  ```

### CLI Verification

```bash
aws iam list-attached-user-policies \
--user-name iamuser_mariyam
```

---

## 🎯 Common Beginner Mistakes

❌ Attaching policy to group instead of user
❌ Choosing wrong policy
❌ Not clicking Add permissions
❌ Typos in names

---

## 💼 Interview & Real-World Insight

**Interview Question:**

> Should permissions be attached to users or groups?

**Answer:**
Best practice is to attach to groups for scalable management.

---

## 🧾 Quick Summary (Revision)

* User: `iamuser_mariyam`
* Policy: `iampolicy_mariyam`
* Action: Attached successfully
* Result: User now has defined permissions

---
