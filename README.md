# 🚀 AWS Route 53 Failover Routing Lab

![AWS](https://img.shields.io/badge/AWS-Route%2053-orange)
![EC2](https://img.shields.io/badge/Amazon-EC2-yellow)
![CloudWatch](https://img.shields.io/badge/Amazon-CloudWatch-blue)
![SNS](https://img.shields.io/badge/Amazon-SNS-red)
![High Availability](https://img.shields.io/badge/Architecture-High%20Availability-green)

## 📖 Project Overview

This project demonstrates the implementation of a highly available web application using Amazon Route 53 Failover Routing Policy.

The architecture uses two EC2 instances deployed across different Availability Zones, Route 53 Health Checks, CloudWatch Alarms, and Amazon SNS notifications to provide automatic failover and continuous service availability.

---

# 🎯 Objectives

* Configure Route 53 Failover Routing
* Monitor application health using Health Checks
* Implement automatic failover between EC2 instances
* Configure CloudWatch alarms
* Send notifications using Amazon SNS
* Improve application availability and resiliency

---

# 🏗 Solution Architecture

```text
                             Users
                               │
                               ▼
                        Amazon Route 53
                         Failover Policy
                               │
             ┌─────────────────┴────────────────┐
             │                                  │
             ▼                                  ▼
       Primary Record                     Secondary Record
             │                                  │
             ▼                                  ▼
      CafeInstance1                       CafeInstance2
        Primary EC2                        Backup EC2
             │
             ▼
        Route 53 Health Check
             │
             ▼
        CloudWatch Alarm
             │
             ▼
           Amazon SNS
      Email Notifications
```

---

# ⚙️ AWS Services Used

| Service                | Purpose                  |
| ---------------------- | ------------------------ |
| Amazon EC2             | Web servers              |
| Amazon Route 53        | DNS and Failover Routing |
| Route 53 Health Checks | Endpoint monitoring      |
| Amazon CloudWatch      | Alarm generation         |
| Amazon SNS             | Notifications            |
| Apache Web Server      | Hosting the application  |

---

# 🔧 Environment Configuration

## Primary Instance

| Parameter     | Value          |
| ------------- | -------------- |
| Instance Name | CafeInstance1  |
| Role          | Primary Server |
| Health Check  | Enabled        |

---

## Secondary Instance

| Parameter     | Value         |
| ------------- | ------------- |
| Instance Name | CafeInstance2 |
| Role          | Backup Server |
| Health Check  | Not required  |

---

# 🛠 Implementation Steps

## Step 1 - Validate EC2 Instances

Verified:

* CafeInstance1 running
* CafeInstance2 running
* Application accessible through:

```text
http://<public-ip>/cafe/
```

---

## Step 2 - Create Route 53 Health Check

Configuration:

| Parameter         | Value                  |
| ----------------- | ---------------------- |
| Name              | Primary-Website-Health |
| Protocol          | HTTP                   |
| Path              | /cafe                  |
| Interval          | 10 seconds             |
| Failure Threshold | 2                      |

Purpose:

Continuously monitor the primary web server.

---

## Step 3 - Configure Amazon SNS

Created SNS Topic:

```text
Primary-Website-Health
```

Added email subscription and confirmed the endpoint.

Purpose:

Receive notifications whenever the primary server becomes unavailable.

---

## Step 4 - Configure Route 53 Failover Routing

### Primary Record

| Parameter      | Value      |
| -------------- | ---------- |
| Record Type    | A          |
| Routing Policy | Failover   |
| Failover Type  | Primary    |
| Health Check   | Enabled    |
| TTL            | 15 seconds |

Points to:

```text
CafeInstance1
```

---

### Secondary Record

| Parameter      | Value      |
| -------------- | ---------- |
| Record Type    | A          |
| Routing Policy | Failover   |
| Failover Type  | Secondary  |
| Health Check   | Disabled   |
| TTL            | 15 seconds |

Points to:

```text
CafeInstance2
```

---

# 🧪 Failover Testing

## Normal Operation

Traffic flow:

```text
User → Route 53 → CafeInstance1
```

Application available through:

```text
http://www.<domain>.vocareum.training/cafe/
```

---

## Failure Simulation

Primary EC2 instance was stopped manually.

Route 53 Health Check detected the outage after approximately 1–2 minutes.

Status changed to:

```text
Unhealthy
```

---

## Automatic Failover

Traffic was automatically redirected to:

```text
CafeInstance2
```

Service remained available without manual intervention.

---

# 📊 Results

✅ Automatic failover configured successfully

✅ High availability achieved

✅ Continuous monitoring implemented

✅ Email notifications configured

✅ Multi-AZ redundancy established

---

# 📚 Skills Demonstrated

* Amazon Route 53
* Failover Routing Policies
* DNS Management
* Health Checks
* CloudWatch Alarms
* Amazon SNS
* High Availability Architecture
* Disaster Recovery Concepts
* Multi-AZ Deployments
* Linux and Apache Administration

---

# 🎓 Learning Outcomes

This project provided hands-on experience with:

* Designing highly available architectures
* Implementing DNS-based failover
* Monitoring application health
* Configuring automated notifications
* Increasing application resiliency
* Applying disaster recovery concepts

---

# 📂 Repository Structure

```text
route53-failover-routing-lab
│
├── README.md
├── images
│     ├── architecture.png
│     ├── route53-health-check.png
│     ├── cloudwatch-alarm.png
│     ├── sns-topic.png
│     └── failover-success.png
│
└── docs
      └── lab-notes.md
```

---

# 📌 Author

**Paulo Henrique

AWS Cloud Practitioner Candidate | Cloud Computing Enthusiast

---

## ⭐ If you found this project useful, feel free to star the repository.
