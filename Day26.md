# 📘 AWS Lab Notes — Launch EC2 with User Data (Nginx Web Server)

---

## 📝 Task Details

The Nautilus DevOps Team is working on setting up a new web server for a critical application. The team lead has requested you to create an EC2 instance that will serve as a web server using Nginx. This instance will be part of the initial infrastructure setup for the Nautilus project. Ensuring that the server is correctly configured and accessible from the internet is crucial for the upcoming deployment phase.

As a member of the Nautilus DevOps Team, your task is to create an EC2 instance with the following specifications:

Instance Name: The EC2 instance must be named devops-ec2.

AMI: Use any available Ubuntu AMI to create this instance.

User Data Script: Configure the instance to run a user data script during its launch. This script should:

Install the Nginx package.
Start the Nginx service.

Security Group: Ensure that the instance allows HTTP traffic on port 80 from the internet.

---

# 🎯 WHY This Task Is Important in AWS

This task demonstrates **infrastructure automation**, which is a core DevOps skill.

Instead of manually installing software after launching a server:

✅ Server config happens automatically
✅ Faster deployments
✅ Consistent environments
✅ No human errors

User Data is widely used in:

* Auto Scaling
* CI/CD pipelines
* Infrastructure as Code
* Immutable infrastructure

---

# 📆 WHEN This Is Used in Real Projects

User data scripts are used when:

* Bootstrapping servers automatically
* Installing applications on launch
* Configuring environments
* Deploying containers or services
* Scaling environments dynamically

Production environments rely heavily on this.

---

# 🧠 HOW User Data Works (Conceptual)

```
EC2 Launch
     ↓
User Data Script Runs (First Boot)
     ↓
Software Installed
     ↓
Server Ready Automatically
```

User Data runs **only once by default** during first launch.

---

# 🚀 Step-by-Step Implementation (AWS Console)

---

# ✅ Step 1 — Launch EC2 Instance

Go to:

EC2 → Launch Instance

Configure:

| Setting       | Value      |
| ------------- | ---------- |
| Name          | devops-ec2 |
| AMI           | Ubuntu     |
| Instance Type | t2.micro   |
| Key Pair      | Any        |
| VPC           | Default    |

---

# ✅ Step 2 — Configure Security Group

Create or modify security group:

Inbound Rules:

| Type | Port | Source    |
| ---- | ---- | --------- |
| HTTP | 80   | 0.0.0.0/0 |
| SSH  | 22   | Your IP   |

This allows internet users to access Nginx.

---

# ✅ Step 3 — Add User Data Script

Scroll to:

**Advanced Details → User Data**

Paste:

```bash
#!/bin/bash

apt update -y
apt install nginx -y
systemctl start nginx
systemctl enable nginx
```

---

# ✅ Step 4 — Launch Instance

Click **Launch Instance**.

Wait until status = Running.

---

# ✅ Step 5 — Verify Web Server

Copy **Public IP** and open browser:

```
http://<EC2-PUBLIC-IP>
```

You should see:

```
Welcome to nginx!
```

🎉 Success.

---

# 💡 Best Practices (Real-World)

✅ Always use user data for automation
✅ Use version-controlled scripts
✅ Keep scripts idempotent
✅ Use configuration tools (Ansible, Terraform) later
✅ Enable logging for debugging

---

# ⚠️ Common Pitfalls

❌ Wrong AMI package manager
❌ Missing port 80 rule
❌ Script syntax errors
❌ Forgetting shebang (`#!/bin/bash`)
❌ Nginx not started

---

# 🔗 Broader AWS Concepts Connected

This task connects to:

* Infrastructure as Code (IaC)
* Auto Scaling Groups
* DevOps automation
* Immutable infrastructure
* CI/CD pipelines

This is foundational DevOps knowledge.

---

# 🎤 Interview Questions & Answers

---

## Q1: What is EC2 User Data?

**Answer:**
A script executed automatically when an EC2 instance launches to configure the system.

---

## Q2: Does User Data run every reboot?

**Answer:**
No, it runs only on first boot by default.

---

## Q3: Why use User Data instead of manual setup?

**Answer:**
Automation ensures consistency, speed, and scalability.

---

## Q4: Where is User Data stored in EC2?

**Answer:**
It is stored as instance metadata.

---

## Q5: Can User Data fail?

Yes — errors in script or permissions can cause failure.

---

# 📊 Real Production Example

Auto Scaling launches 10 servers during traffic spike.

Each server automatically:

* Installs app
* Configures environment
* Joins load balancer

No human action required.

---

# 📌 Quick Revision Summary

```
Launch EC2
→ Add User Data Script
→ Allow HTTP
→ Nginx installs automatically
→ Access via browser
```

---
