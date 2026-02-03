# 📘 AWS Lab Notes – S3 Data Migration & Synchronization

---

## 📝 Task Details

As part of a data migration project, the team lead has tasked the team with migrating data from an existing S3 bucket to a new S3 bucket. The existing bucket contains a substantial amount of data that must be accurately transferred to the new bucket. The team is responsible for creating the new S3 bucket and ensuring that all data from the existing bucket is copied or synced to the new bucket completely and accurately. It is imperative to perform thorough verification steps to confirm that all data has been successfully transferred to the new bucket without any loss or corruption.

As a member of the Nautilus DevOps Team, your task is to perform the following:

Create a New Private S3 Bucket: Name the bucket nautilus-sync-28753.

Data Migration: Migrate the entire data from the existing nautilus-s3-28922 bucket to the new nautilus-sync-28753 bucket.

Ensure Data Consistency: Ensure that both buckets have the same data.

---

# 🎯 WHY This Task Is Important in AWS

Data migration between S3 buckets is extremely common in:

* Cloud modernization projects
* Backup & disaster recovery
* Environment separation (dev → prod)
* Region migration
* Security restructuring

### Key reasons:

✅ Move data without downtime
✅ Create backups
✅ Restructure storage
✅ Improve security or compliance

Without proper migration:

* Data loss can occur
* Applications can break
* Compliance risks increase

---

# 📆 WHEN This Is Typically Used

You use S3 sync/copy when:

* Migrating workloads to new AWS accounts<br>
* Creating disaster recovery replicas<br>
* Changing bucket policies or encryption<br>
* Reorganizing environments<br>
* Archiving data

---

# ⚙️ HOW This Works (Conceptually)

### S3 Migration usually follows this flow:

```
Old Bucket  ──>  New Bucket
     │
   Objects copied/synced
     │
Verification to confirm match
```

AWS provides:

* aws s3 cp (copy)
* aws s3 sync (best for full migration)

👉 `sync` ensures both sides remain identical.

---

# 🚀 Step-by-Step Solution (CLI – Real DevOps Way)

---

## ✅ Step 1: Create the New Private Bucket

```bash
aws s3 mb s3://nautilus-sync-28753
```

📌 By default, S3 buckets are private.

---

## ✅ Step 2: Migrate Data Using Sync (Best Practice)

```bash
aws s3 sync s3://nautilus-s3-28922 s3://nautilus-sync-28753
```

### What this does:

* Copies all objects<br>
* Preserves folder structure<br>
* Transfers only missing/changed files

---

## ✅ Step 3: Verify Data Consistency

### Option 1 — List both buckets:

```bash
aws s3 ls s3://nautilus-s3-28922 --recursive
aws s3 ls s3://nautilus-sync-28753 --recursive
```

Compare output.

---

### Option 2 — Object count (quick check):

```bash
aws s3 ls s3://nautilus-s3-28922 --recursive | wc -l
aws s3 ls s3://nautilus-sync-28753 --recursive | wc -l
```

Counts should match.

---

# ✅ Expected Result

✔ New bucket created<br>
✔ All data copied<br>
✔ Both buckets contain identical objects

---

# 💡 Best Practices (Real-World AWS)

✅ Use `aws s3 sync` instead of `cp` for large data<br>
✅ Verify after migration<br>
✅ Use versioning for safety<br>
✅ Consider encryption (SSE-S3 or SSE-KMS)<br>
✅ Log operations for audits

---

# ⚠️ Common Pitfalls to Avoid

❌ Forgetting verification<br>
❌ Using `cp` for huge buckets<br>
❌ Overwriting critical data<br>
❌ Migrating without permissions<br>
❌ Public bucket exposure

---

# 🔐 Security & Scalability Insight

### This task connects to:

* IAM permissions (least privilege)<br>
* S3 encryption<br>
* Disaster recovery planning<br>
* Cross-account migration<br>
* Cost optimization (storage classes)

---

# 🎤 Interview Questions & Answers

### Q1: Difference between `aws s3 cp` and `aws s3 sync`?

**Answer:**
`cp` copies objects once.
`sync` keeps source and destination consistent by copying only new or changed files.

---

### Q2: How do you verify S3 migration?

**Answer:**
By listing objects, comparing counts, or using checksum tools.

---

### Q3: Is S3 sync incremental?

**Answer:**
Yes — it only transfers changed or missing files.

---

### Q4: How to migrate very large buckets efficiently?

**Answer:**
Use multipart transfers, S3 sync, parallelism, or AWS DataSync.

---

### Q5: How do you secure migrated data?

**Answer:**
Using private buckets, IAM policies, encryption, and access logging.

---

# 📌 Quick Revision Summary

```
Create new bucket
→ Sync old bucket to new bucket
→ Verify object match
→ Migration complete
```

---

