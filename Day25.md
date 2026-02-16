# 📘 AWS Lab Notes — EC2 Monitoring with CloudWatch Alarm & SNS

---

## 📝 Task Details

The Nautilus DevOps team has been tasked with setting up an EC2 instance for their application. To ensure the application performs optimally, they also need to create a CloudWatch alarm to monitor the instance's CPU utilization. The alarm should trigger if the CPU utilization exceeds 90% for one consecutive 5-minute period. To send notifications, use the SNS topic named nautilus-sns-topic which is already created.

Launch EC2 Instance: Create an EC2 instance named nautilus-ec2 using any appropriate Ubuntu AMI.

Create CloudWatch Alarm: Create a CloudWatch alarm named nautilus-alarm with the following specifications:

Statistic: Average
Metric: CPU Utilization
Threshold: >= 90% for 1 consecutive 5-minute period.
Alarm Actions: Send a notification to nautilus-sns-topic.

---

# 🎯 WHY This Task Is Important in AWS

Monitoring is a **critical pillar** of cloud operations.

Without monitoring:

❌ You don’t know when servers are overloaded
❌ Performance issues go unnoticed
❌ Downtime increases

CloudWatch alarms help:

✅ Detect problems early
✅ Trigger notifications
✅ Enable auto-scaling
✅ Maintain reliability

This is part of **AWS Well-Architected Framework — Operational Excellence**.

---

# 📆 WHEN This Is Used in Real Projects

CloudWatch alarms are used when:

* Monitoring CPU, memory, disk
* Auto-scaling triggers
* Incident alerts
* Production monitoring
* Cost anomaly detection

Every production system uses monitoring.

---

# 🧠 HOW It Works (Conceptual Flow)

```
EC2 Instance
     ↓
CloudWatch collects metrics
     ↓
Alarm evaluates threshold
     ↓
SNS Topic sends notification
     ↓
Email / SMS / System Alert
```

So:

> Metrics → Alarm → SNS → Notification

---

# 🚀 Step-by-Step Implementation (Console)

---

# ✅ Step 1 — Launch EC2 Instance

Go to:

EC2 → Launch Instance

Configure:

| Setting        | Value        |
| -------------- | ------------ |
| Name           | nautilus-ec2 |
| AMI            | Ubuntu       |
| Type           | t2.micro     |
| Security Group | Default      |

Launch instance.

Wait until **Running**.

---

# ✅ Step 2 — Create CloudWatch Alarm

Go to:

CloudWatch → Alarms → Create Alarm

---

## Select Metric

1. Click **Select Metric**
2. Choose:

```
EC2 → Per-Instance Metrics → CPUUtilization
```

3. Select your instance
4. Click **Select Metric**

---

## Configure Metric

Set:

| Field          | Value         |
| -------------- | ------------- |
| Statistic      | Average       |
| Period         | 5 minutes     |
| Threshold type | Static        |
| Condition      | Greater/Equal |
| Threshold      | 90            |

Evaluation Periods:

```
1 out of 1
```

Meaning:

> One 5-minute period above 90%

---

# ✅ Step 3 — Configure SNS Notification

In **Alarm Actions**:

Select:

```
nautilus-sns-topic
```

This sends notification when alarm triggers.

---

# ✅ Step 4 — Alarm Name

Name:

```
nautilus-alarm
```

Create alarm.

---

# ✅ Verification

✔ Alarm appears in CloudWatch
✔ State initially: OK
✔ SNS topic attached
✔ Instance metric visible

---

# 💡 Best Practices (Real-World)

✅ Use multiple alarms (CPU, memory, disk)
✅ Set realistic thresholds
✅ Enable auto-scaling triggers
✅ Use dashboards for visualization
✅ Monitor application metrics too

---

# ⚠️ Common Pitfalls

❌ Wrong metric selected
❌ Wrong instance ID
❌ SNS not subscribed
❌ Threshold too low/high
❌ Alarm period confusion

---

# 🔗 Broader AWS Concepts Connected

This task connects to:

* Observability & Monitoring
* Auto Scaling Groups
* High Availability
* Incident Management
* Cost Optimization

CloudWatch is core to AWS operations.

---

# 🎤 Interview Questions & Answers

---

## Q1: What is CloudWatch?

**Answer:**
AWS monitoring service that collects metrics, logs, and events from resources.

---

## Q2: Difference between metric and alarm?

**Answer:**

| Metric     | Alarm        |
| ---------- | ------------ |
| Data point | Rule on data |

---

## Q3: Can CloudWatch trigger scaling?

Yes — alarms can trigger Auto Scaling policies.

---

## Q4: What is SNS used for?

Notification service to send alerts via email, SMS, Lambda, etc.

---

## Q5: Why monitor CPU utilization?

High CPU indicates performance bottleneck or heavy load.

---

# 📌 Practical Tip (Exam + Real World)

CPU > 80–90% usually means:

* Need scaling
* Application inefficiency
* Resource bottleneck

---

# 📊 Example Scenario

Traffic spike occurs → CPU hits 95%

CloudWatch detects:

```
>= 90% for 5 minutes
```

Alarm triggers → SNS sends email → Engineer investigates.

---

# 📌 Quick Revision Summary

```
Launch EC2
→ Create CloudWatch Alarm
→ Set threshold 90%
→ Attach SNS topic
→ Monitor system
```

---
