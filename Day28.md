# 📘 Day 28: Creating a Private ECR Repository

---

## 📝 Task Details

Day 28: Creating a Private ECR Repository
The Nautilus DevOps team has been tasked with setting up a containerized application. They need to create a private Amazon Elastic Container Registry (ECR) repository to store their Docker images. Once the repository is created, they will build a Docker image from a Dockerfile located on the aws-client host and push this image to the ECR repository. This process is essential for maintaining and deploying containerized applications in a streamlined manner.

Create a private ECR repository named nautilus-ecr. There is a Dockerfile under /root/pyapp directory on aws-client host, build a docker image using this Dockerfile and push the same to the newly created ECR repo, the image tag must be latest.

---

# 🎯 WHY This Task Is Important in AWS

Modern applications use **containers (Docker)**.

ECR provides:

✅ Secure private image storage<br>
✅ Integration with ECS/EKS/Kubernetes<br>
✅ Version management<br>
✅ CI/CD deployment pipelines

Without a registry:

❌ No centralized container storage<br>
❌ Hard deployments<br>
❌ Security risks

---

# 📆 WHEN This Is Used

Used when:

* Deploying microservices
* Using Kubernetes (EKS)
* ECS deployments
* CI/CD pipelines
* Container-based apps

---

# 🧠 HOW It Works (Concept)

```
Dockerfile → Build Image → Tag → Push → ECR
```

Later:

```
ECS / EKS pulls image from ECR
```

---

# 🚀 Method 1 — AWS Console + CLI (Most Common Real Workflow)

You create repo in console and push via CLI.

---

## ✅ Step 1 — Create Repository (Console)

Go to:

ECR → Repositories → Create repository

Configure:

| Setting         | Value        |
| --------------- | ------------ |
| Visibility      | Private      |
| Repository name | nautilus-ecr |

Create repository.

Copy **Repository URI**.

Example:

```
123456789012.dkr.ecr.us-east-1.amazonaws.com/nautilus-ecr
```

---

## ✅ Step 2 — Authenticate Docker to ECR (CLI)

On aws-client:

```bash
aws ecr get-login-password --region us-east-1 \
| docker login --username AWS --password-stdin <ACCOUNT_ID>.dkr.ecr.us-east-1.amazonaws.com
```

---

## ✅ Step 3 — Go to Dockerfile Directory

```bash
cd /root/pyapp
```

---

## ✅ Step 4 — Build Docker Image

```bash
docker build -t nautilus-ecr .
```

---

## ✅ Step 5 — Tag Image for ECR

```bash
docker tag nautilus-ecr:latest <ACCOUNT_ID>.dkr.ecr.us-east-1.amazonaws.com/nautilus-ecr:latest
```

---

## ✅ Step 6 — Push Image

```bash
docker push <ACCOUNT_ID>.dkr.ecr.us-east-1.amazonaws.com/nautilus-ecr:latest
```

---

## ✅ Step 7 — Verify

Console → ECR → Repository → Images

Tag:

```
latest
```

---

# 🚀 Method 2 — Fully CLI (DevOps Automation Way)

---

## Create Repository

```bash
aws ecr create-repository \
--repository-name nautilus-ecr \
--region us-east-1
```

---

## Authenticate Docker

```bash
aws ecr get-login-password --region us-east-1 \
| docker login --username AWS --password-stdin <ACCOUNT_ID>.dkr.ecr.us-east-1.amazonaws.com
```

---

## Build Image

```bash
cd /root/pyapp
docker build -t nautilus-ecr .
```

---

## Tag Image

```bash
docker tag nautilus-ecr:latest <ACCOUNT_ID>.dkr.ecr.us-east-1.amazonaws.com/nautilus-ecr:latest
```

---

## Push Image

```bash
docker push <ACCOUNT_ID>.dkr.ecr.us-east-1.amazonaws.com/nautilus-ecr:latest
```

---

# ✅ Verification Checklist

✔ Repo created
✔ Docker login successful
✔ Image built
✔ Tag latest present
✔ Push successful

---

# 💡 Best Practices (Real AWS)

✅ Use version tags (v1, v2)<br>
✅ Enable vulnerability scanning<br>
✅ Use IAM roles instead of credentials<br>
✅ Automate via CI/CD<br>
✅ Clean old images

---

# ⚠️ Common Pitfalls

❌ Wrong repository URI<br>
❌ Forgot docker login<br>
❌ Wrong region<br>
❌ Docker not running<br>
❌ Tag mismatch

---

# 🔗 Broader AWS Concepts

This connects to:

* Containers & microservices
* DevOps pipelines
* Kubernetes deployments
* ECS architecture
* CI/CD automation

---

# 🎤 Interview Questions & Answers

---

## Q1: What is Amazon ECR?

A managed Docker container registry service.

---

## Q2: Why tag images?

To manage versions and deployments.

---

## Q3: Difference between ECR and S3?

ECR stores container images, S3 stores files/objects.

---

## Q4: How authentication works?

Using IAM credentials with temporary tokens.

---

## Q5: Can ECR scan vulnerabilities?

Yes — image scanning feature exists.

---

# 📊 Real Production Flow

```
Developer → CI/CD → Docker Build → ECR → ECS/EKS Deployment
```

---

# 📌 Quick Revision Summary

```
Create Repo
→ Docker Login
→ Build Image
→ Tag
→ Push
```

---

