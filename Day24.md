# 📘 Day 24: Setting Up an Application Load Balancer for an EC2 Instance


The Nautilus DevOps team is currently working on setting up a simple application on the AWS cloud. They aim to establish an Application Load Balancer (ALB) in front of an EC2 instance where an Nginx server is currently running. While the Nginx server currently serves a sample page, the team plans to deploy the actual application later.

Set up an Application Load Balancer named nautilus-alb.
Create a target group named nautilus-tg.
Create a security group named nautilus-sg to open port 80 for the public.
Attach this security group to the ALB.
The ALB should route traffic on port 80 to port 80 of the nautilus-ec2 instance.
Make appropriate changes in the default security group attached to the EC2 instance if necessary.

---

# 🎯 WHY This Task Is Important in AWS

Application Load Balancers are the foundation of:

* Highly available web applications
* Scalable architectures
* Secure backend systems

They:

✅ Distribute traffic<br>
✅ Protect backend servers<br>
✅ Handle traffic spikes<br>
✅ Enable future auto-scaling

Without ALB:

* Single EC2 = single point of failure
* Poor scalability
* Security risks

---

# 📆 WHEN This Is Used in Real Projects

* Hosting production websites
* Running microservices
* Blue/green deployments
* Auto Scaling Groups
* High traffic applications

Almost every AWS production system uses a load balancer.

---

# 🧠 Conceptual Architecture (Traffic Flow)

```
Users (Internet)
      ↓
Application Load Balancer (nautilus-alb)
      ↓
Target Group (nautilus-tg)
      ↓
EC2 Instance (nautilus-ec2 running Nginx)
```

### 🔐 Security Best Practice:

EC2 does NOT allow public access.

It only accepts traffic from:
👉 ALB Security Group

This is how production systems are built.

---

# ⚙️ HOW It Works (Simple Logic)

| Component        | Role                         |
| ---------------- | ---------------------------- |
| ALB              | Public entry point           |
| SG (nautilus-sg) | Allows HTTP from internet    |
| Target Group     | Knows backend servers        |
| EC2 SG           | Allows traffic only from ALB |

---

# 🚀 Step-by-Step Implementation (Console)

---

## ✅ Step 1: Verify EC2 Instance

Ensure:

✔ EC2 is running<br>
✔ Web server (Nginx) is serving a page<br>
✔ Instance is in correct VPC

This will act as backend target.

---

## ✅ Step 2: Create Security Group for ALB

Go to **EC2 → Security Groups → Create**

### Settings:

**Name:**

```
nautilus-sg
```

### Inbound Rule:

| Type | Port | Source    |
| ---- | ---- | --------- |
| HTTP | 80   | 0.0.0.0/0 |

Outbound: Allow all.

👉 This allows public users to reach ALB.

---

## ✅ Step 3: Create Target Group

Go to **EC2 → Target Groups → Create**

### Configure:

* Target type: Instance
* Name:

```
nautilus-tg
```

* Protocol: HTTP
* Port: 80
* VPC: Same as EC2

### Register target:

✔ Select `nautilus-ec2`
✔ Include as pending
✔ Create target group

---

## ✅ Step 4: Create Application Load Balancer

Go to **EC2 → Load Balancers → Create**

Choose:

✔ Application Load Balancer<br>
✔ Internet-facing

### Configure:

**Name:**

```
nautilus-alb
```

**Network:**

* VPC: Default
* Subnets: At least 2 AZs

**Security Group:**
✔ nautilus-sg

**Listener:**

* HTTP : 80
* Forward to:

```
nautilus-tg
```

Create ALB.

---

## ✅ Step 5: Secure the EC2 Instance (Important)

Go to EC2’s **Security Group**

### Modify inbound rules:

❌ Remove public HTTP access
✅ Add:

| Type | Port | Source      |
| ---- | ---- | ----------- |
| HTTP | 80   | nautilus-sg |

👉 Only ALB can reach EC2 now.

---

## ✅ Step 6: Health Checks & Testing

Go to **Target Groups → Health**

Wait until:

```
Status: Healthy
```

Copy ALB DNS Name:

```
http://<alb-dns-name>
```

Open in browser.

🎉 Nginx page loads through ALB!

---

# ✅ Verification Checklist

✔ ALB active<br>
✔ Target healthy<br>
✔ SG properly restricted<br>
✔ Public access via ALB only<br>
✔ EC2 protected

---

# 💡 Best Practices (Real AWS)

✅ Never expose EC2 directly<br>
✅ Always use ALB SG as source<br>
✅ Use HTTPS in production<br>
✅ Enable health checks<br>
✅ Use multiple AZs<br>
✅ Add Auto Scaling later

---

# ⚠️ Common Pitfalls

❌ EC2 still open to internet<br>
❌ Wrong port mapping<br>
❌ Target unhealthy<br>
❌ ALB in private subnet<br>
❌ No listener

---

# 🔗 Broader AWS Concepts Connected

* High Availability<br>
* Zero downtime deployments<br>
* Security isolation<br>
* Auto Scaling<br>
* Cloud architecture design

---

# 🎤 Interview Questions & Answers

### Q1: Why allow EC2 traffic only from ALB SG?

**Answer:**
To prevent public exposure and ensure all traffic flows securely through the load balancer.

---

### Q2: What happens if ALB fails?

**Answer:**
AWS automatically manages ALB redundancy across AZs.

---

### Q3: How does ALB know if EC2 is healthy?

**Answer:**
Via HTTP health check probes.

---

### Q4: Can ALB route to multiple EC2s?

**Answer:**
Yes, using target groups (for load balancing & scaling).

---

### Q5: Difference between ALB and Classic LB?

**Answer:**
ALB supports layer 7 routing and modern features.

---

# 📌 Quick Revision Summary

```
Create SG → Create Target Group → Create ALB → Secure EC2 → Health Check → Test DNS
```

---

---

# 🌐 Real-Life Example of ALB Flow (Simple & Clear)

Imagine:

👤 User = Rahul
🌍 Website = `www.nautilusapp.com`
🖥 Backend server = EC2 running Nginx

---

## 🧭 Step-by-Step What Actually Happens

---

### 👤 1. User Sends Request (Internet)

Rahul opens his browser and types:

```
http://www.nautilusapp.com
```

(or ALB DNS name)

This request travels over the internet.

---

### 🚦 2. Request Hits Application Load Balancer (nautilus-alb)

The ALB receives Rahul’s request first.

ALB does:

✔ Accepts traffic on port 80
✔ Checks which backend server is healthy
✔ Applies routing rules

Think of ALB like a **traffic police officer** 🚓:

> “Okay Rahul’s request came in — let me forward it to a healthy server.”

---

### 📦 3. ALB Forwards to Target Group (nautilus-tg)

The Target Group is a **list of backend servers**.

In your lab:

```
nautilus-tg → contains nautilus-ec2
```

ALB looks inside this list and selects:

👉 nautilus-ec2

---

### 🖥 4. EC2 Instance Processes Request

The EC2 instance:

* Receives HTTP request
* Nginx processes it
* Sends back web page

Example response:

```
Welcome to Nginx on Nautilus Server!
```

---

### 🔁 5. Response Goes Back to User

EC2 → ALB → Internet → Rahul’s browser

Rahul sees the webpage 🎉

---

# 📊 Visual With Meaning

```
Rahul opens website
      ↓
ALB receives request (public entry)
      ↓
Chooses healthy server
      ↓
EC2 processes request
      ↓
Response returned to Rahul
```

---

# 🧠 Why This Is Powerful

Without ALB:

```
User → EC2 directly (dangerous + not scalable)
```

With ALB:

```
User → ALB → many EC2s (secure + scalable)
```

Later you can add:

```
ALB → EC2-1
ALB → EC2-2
ALB → EC2-3
```

Traffic is automatically balanced 🔁

---

# 🎯 Real Company Example

Think of Amazon.com:

Millions of users → Load Balancers → thousands of servers

Users never touch servers directly.

---

# 🔐 Security Benefit (Important)

Users can only access:

✅ ALB

They CANNOT access:

❌ EC2 directly

So hackers can’t hit backend servers.

---

# 📌 Interview-friendly explanation

> “Users send requests to an Application Load Balancer which acts as a public entry point. The ALB forwards traffic to healthy EC2 instances registered in a target group, ensuring high availability, scalability, and security.”

---

---

# 🎯 Why Do We Need Target Groups in AWS ALB?

Short answer:

👉 **Target Groups are how ALB knows WHERE to send traffic.**

But there’s much more power behind them.

---

# 🧠 Simple Analogy (Real World)

Imagine:

🏢 Office building (ALB)<br>
📋 Reception desk (Target Group list)<br>
👩‍💻 Employees (EC2 servers)

When a visitor comes:

Reception checks the list:

✔ Who is available<br>
✔ Who is healthy<br>
✔ Who handles this type of request<br>

Then sends visitor to one person.

---

# 📦 In AWS Terms:

| Component    | Role                     |
| ------------ | ------------------------ |
| ALB          | Receives traffic         |
| Target Group | List of backend services |
| Targets      | EC2 / IPs / Lambda       |

---

# 🚀 What Target Groups Actually Do

### 1️⃣ Hold Backend Servers

Example:

```
nautilus-tg:
   - nautilus-ec2
   - ec2-2
   - ec2-3 (later)
```

ALB doesn’t talk to EC2 directly — it talks to **target groups**.

---

### 2️⃣ Perform Health Checks ❤️

Target Group constantly asks:

```
Are you alive?
Can you serve requests?
```

If EC2 fails:

❌ Removed automatically<br>
✔ Traffic goes to healthy ones

---

### 3️⃣ Enable Scaling 📈

When Auto Scaling adds new EC2:

➡️ Automatically registered into Target Group<br>
➡️ ALB starts sending traffic immediately

No manual config needed.

---

### 4️⃣ Enable Smart Routing 🧭

Later you can do:

| URL Path | Target Group |
| -------- | ------------ |
| /api     | api-tg       |
| /images  | image-tg     |
| /app     | app-tg       |

This powers microservices.

---

# ❌ Why ALB Can’t Just Send Traffic Directly to EC2

Without target groups:

* No health checks
* No scaling
* No routing rules
* No automation

Target groups = control layer.

---

# 📊 Flow With Purpose

```
User → ALB → Target Group → Healthy EC2
```

Not:

```
User → ALB → Random EC2
```

---

# 🎯 Interview-Ready Answer (Short & Strong)

> Target Groups define the backend resources for an ALB and handle health checks, load distribution, scaling, and routing. ALB forwards traffic to healthy targets inside a target group.

---

# 💡 Real-world usage

Companies use Target Groups to:

✅ Run microservices<br>
✅ Handle version deployments<br>
✅ Scale automatically<br>
✅ Fail over instantly

---

# ⚠️ Common beginner mistake

❌ Thinking ALB connects directly to EC2<br>
✔ It always goes via Target Groups

---


