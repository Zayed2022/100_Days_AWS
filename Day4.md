# 📘 Day 4: Enable Versioning for S3 Bucket

**Day 4: Enable Versioning for S3 Bucket**

Data protection and recovery are fundamental aspects of data management. It's essential to have systems in place to ensure that data can be recovered in case of accidental deletion or corruption. The DevOps team has received a requirement for implementing such measures for one of the S3 buckets they are managing.

The s3 bucket name is **devops-s3-27895**, enable versioning for this bucket.

Use below given AWS Credentials:
(You can run the showcreds command on aws-client host to retrieve these credentials)



**Notes:**

* Create the resources only in **us-east-1** region.
* To display or hide the terminal of the AWS client machine, you can use the expand toggle button.

---

## 🧠 Concept Explanation (Beginner Friendly)

### What is S3 Versioning?

**S3 Versioning** keeps **multiple versions of the same object** in a bucket.

If you:

* Upload a file again → old version is preserved
* Delete a file → it is **not permanently deleted**
* Accidentally overwrite data → you can restore it

---

### Simple Analogy 🧾

Think of **Google Docs version history**:

* Every save = new version
* You can roll back anytime

That’s exactly what S3 versioning does.

---

## 🏗️ Why This Task Is Important (WHY)

In real projects like **Project Nautilus**:

* Data loss can be **very expensive**
* Accidental deletes happen
* Applications overwrite files often

Enabling versioning ensures:

* 🔐 Data protection
* 🔄 Easy recovery
* 📜 Audit history

---

## 🧭 What We Are Doing (WHAT)

We will:

1. Locate the existing S3 bucket
2. Enable **Versioning**
3. Verify it is turned **ON**

⚠️ Note:
Versioning is **bucket-level** and **cannot be applied to individual files**.

---

## 🛠️ Solution — Method 1: AWS Console (UI)

### Step 1: Login to AWS Console

* Open the **Console URL**
* Login using provided credentials
* Ensure region is **us-east-1 (N. Virginia)**

---

### Step 2: Open S3 Service

* Search **S3** in AWS search bar
* Click **S3**

---

### Step 3: Select the Bucket

* Click bucket name:

  ```
  devops-s3-27895
  ```

---

### Step 4: Enable Versioning

1. Go to **Properties** tab
2. Scroll to **Bucket Versioning**
3. Click **Edit**
4. Select **Enable**
5. Click **Save changes**

✅ Versioning is now enabled.

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

### Step 2: Enable Versioning

```bash
aws s3api put-bucket-versioning \
--bucket devops-s3-27895 \
--versioning-configuration Status=Enabled
```

---

## ✅ Verification Steps

### Console Verification

* S3 → devops-s3-27895
* Properties tab
* Bucket Versioning should show:

  ```
  Enabled
  ```

### CLI Verification

```bash
aws s3api get-bucket-versioning \
--bucket devops-s3-27895
```

Expected output:

```json
{
  "Status": "Enabled"
}
```

---

## 🎯 Common Beginner Mistakes

❌ Trying to enable versioning on objects instead of bucket
❌ Using wrong region
❌ Assuming versioning is enabled by default
❌ Forgetting to save changes in console

---

## 💼 Interview & Real-World Insight

**Interview Question:**

> What happens if you delete an object from a versioned bucket?

**Answer:**
A **delete marker** is added. The object is not permanently deleted and can be restored.

---

## 🧾 Quick Summary (Revision)

* S3 Versioning = multiple object versions
* Protects against accidental deletion
* Bucket: `devops-s3-27895`
* Region: us-east-1
* Status: Enabled

---
