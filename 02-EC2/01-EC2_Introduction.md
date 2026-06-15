## Amazon EC2 (Elastic Compute Cloud)

### What is Amazon EC2?

Amazon EC2 (Elastic Compute Cloud) is a web service provided by AWS that allows users to launch and manage virtual servers in the cloud.

Instead of purchasing and maintaining physical servers, you can rent virtual machines called **EC2 Instances** and pay only for what you use.

> EC2 = Infrastructure as a Service (IaaS)


### Core EC2 Ecosystem

EC2 rarely works alone.

```text
                +------------+
                |    EC2     |
                +------------+
                      |
    -----------------------------------
    |                |               |
    v                v               v
   EBS             ELB             ASG
(Storage)   (Load Balancer) (Auto Scaling)
```

---

### EC2 Components

### 1. EC2 Instance

An EC2 Instance is a virtual machine running in AWS.

Example:

- Ubuntu Server
- 2 vCPU
- 4 GB RAM
- 50 GB Storage

You can:

- Host websites
- Run applications
- Install databases
- Execute scripts

---

### 2. EBS (Elastic Block Store)

EBS is persistent storage attached to an EC2 instance.

Think of it as:

```text
Laptop = EC2
Hard Disk = EBS
```

### Features

- Persistent storage
- Data survives reboot
- Supports snapshots
- Can be detached and reattached

---

### 3. ELB (Elastic Load Balancer)

Distributes incoming traffic across multiple EC2 instances.

Without ELB:

```text
Users
  |
  v
EC2-1
```

If EC2-1 fails:

```text
Website Down
```

With ELB:

```text
             ELB
              |
    ---------------------
    |         |         |
    v         v         v
  EC2-1     EC2-2     EC2-3
```

Benefits:

- High availability
- Better performance
- Fault tolerance

---

### 4. Auto Scaling Group (ASG)

Automatically adjusts the number of EC2 instances based on traffic.

Normal Traffic:

```text
EC2-1
EC2-2
```

High Traffic:

```text
EC2-1
EC2-2
EC2-3
EC2-4
EC2-5
```

Benefits:

- Cost optimization
- Automatic scaling
- Improved reliability


## Security Groups

A Security Group acts as a virtual firewall for an EC2 instance.

Example Rules:

| Type | Port |
|--------|------|
| SSH | 22 |
| HTTP | 80 |
| HTTPS | 443 |

Example:

```text
Internet
    |
    v
Security Group
    |
    v
EC2 Instance
```

Only allowed traffic can enter.

---

## EC2 User Data

### What is User Data?

User Data is a startup script that runs automatically when an EC2 instance launches.

```text
Launch Instance
       ↓
Execute User Data
       ↓
Install Software
       ↓
Configure Server
```

---

### Why Use User Data?

Automation.

Instead of manually:

```bash
sudo yum update -y
sudo yum install httpd -y
sudo systemctl start httpd
```

You can automate everything during launch.

---

### Example User Data Script

```bash
#!/bin/bash

yum update -y

yum install httpd -y

systemctl start httpd

systemctl enable httpd

echo "Hello from EC2" > /var/www/html/index.html
```

---

### What Happens?

```text
Instance Launches
        ↓
Updates Packages
        ↓
Installs Apache
        ↓
Starts Apache
        ↓
Creates Web Page
```

Visiting:

```text
http://PUBLIC_IP
```

Displays:

```text
Hello from EC2
```

---

### Important Note

By default, User Data runs:

```text
Only Once
```

Specifically during:

```text
First Boot
```

---

### EC2 Instance Types

AWS offers different instance families optimized for different workloads.

Example:

```text
m5.2xlarge
```

Breakdown:

| Component | Meaning |
|------------|----------|
| m | Instance Family |
| 5 | Generation |
| 2xlarge | Instance Size |

---

### General Purpose Instances

Examples:

- t2.micro
- t3.micro
- m5.large

Balanced between:

- CPU
- Memory
- Networking

Use Cases:

- Web servers
- Development environments
- Small databases
- Application servers

---

### Compute Optimized Instances

Examples:

- C5
- C6i
- C7g

Designed for CPU-intensive workloads.

Use Cases:

- Video encoding
- Scientific computing
- Gaming servers
- Machine learning inference
- Batch processing

---

### Memory Optimized Instances

Examples:

- R5
- R6i
- X1

Designed for memory-intensive workloads.

Use Cases:

- Databases
- Redis
- SAP
- Analytics

---

### Instance Family Comparison

| Family | Optimized For |
|----------|--------------|
| T | Burstable Workloads |
| M | General Purpose |
| C | Compute Intensive |
| R | Memory Intensive |
| I | Storage Intensive |
| P | GPU Workloads |
| G | Graphics Workloads |

## AWS EC2 Purchasing Options

### Introduction

AWS provides multiple purchasing options for EC2 instances to help customers optimize costs based on workload requirements.

Choosing the correct purchasing model can save anywhere from **30% to 90%** compared to standard On-Demand pricing.

Think of EC2 purchasing options like booking a hotel room:

| AWS Option | Hotel Analogy |
|------------|---------------|
| On-Demand | Book a room whenever you want and pay full price |
| Reserved Instance | Reserve a room for 1-3 years and get a discount |
| Savings Plan | Commit to spending a fixed amount regularly |
| Spot Instance | Bid for unused hotel rooms at huge discounts |
| Dedicated Host | Rent the entire hotel building |
| Dedicated Instance | Rent a private floor in the hotel |
| Capacity Reservation | Reserve a room even if you don't stay there |


### 1. On-Demand Instances

### What is On-Demand?

On-Demand instances allow you to pay only for the resources you use.

No contracts.

No commitments.

No upfront payment.

```text
Launch Instance
      ↓
Use It
      ↓
Pay Only For Usage
```

---

### Billing

Typically billed:

- Per second (Linux)
- Per hour (some operating systems)

### Drawbacks

❌ Highest cost

❌ No discount

---

### 2. Reserved Instances (RI)

### What are Reserved Instances?

Reserved Instances provide significant discounts in exchange for a commitment.

Commitment periods:

- 1 Year
- 3 Years

---

### Discount

Can save up to:

```text
72% compared to On-Demand
```


### Payment Options

### No Upfront

Pay monthly.

### Partial Upfront

Pay some amount now.

### All Upfront

Pay entire reservation cost.

Most discount.

### Convertible Reserved Instances

### What Are Convertible RIs?

A special type of Reserved Instance that allows modifications later.

You can change:

- Instance Family
- Instance Type
- Operating System
- Tenancy

### Standard vs Convertible RI

| Feature | Standard RI | Convertible RI |
|-----------|------------|----------------|
| Highest Discount | ✅ | ❌ |
| Change Instance Family | ❌ | ✅ |
| Change OS | ❌ | ✅ |
| Flexibility | Low | High |

---

### 3. Savings Plans

### What Are Savings Plans?

Savings Plans provide discounts in exchange for committing to a specific amount of spending.

Instead of reserving an instance:

```text
I will spend $10/hour
```

AWS applies discounts automatically.

---

### Commitment Period

- 1 Year
- 3 Years

---

### Benefits

Up to:

```text
72% discount
```

---

### Flexibility

Can change:

- Instance Size
- Instance Type
- Operating System
- Tenancy

Depending on Savings Plan type.


### Reserved Instances vs Savings Plans

| Feature | Reserved Instance | Savings Plan |
|-----------|------------------|--------------|
| Flexibility | Low | High |
| Cost Savings | High | High |
| Instance Specific | Yes | No |
| Easy Management | No | Yes |

---

### 4. Spot Instances

### What Are Spot Instances?

AWS often has unused EC2 capacity.

AWS sells this spare capacity at huge discounts.

Discount:

```text
Up to 90%
```

### Spot Fleet

### What is Spot Fleet?

Spot Fleet allows AWS to launch multiple Spot Instances automatically.

Instead of:

```text
1 Instance Type
```

Use:

```text
Multiple Instance Types
Multiple AZs
Multiple Pricing Pools
```

AWS chooses the cheapest combination.

---

### Benefits

- Better availability
- Lower costs
- Automatic optimization

### 5. Dedicated Hosts

### What is a Dedicated Host?

A physical server dedicated entirely to your organization.

### 6. Dedicated Instances

### What Are Dedicated Instances?

Instances run on hardware dedicated to a single customer.

### Dedicated Instance vs Dedicated Host

| Feature | Dedicated Instance | Dedicated Host |
|-----------|-------------------|----------------|
| Dedicated Hardware | ✅ | ✅ |
| Physical Host Visibility | ❌ | ✅ |
| BYOL Support | Limited | Excellent |
| Cost | Lower | Higher |

---

### 7. Capacity Reservations

### What is Capacity Reservation?

Guarantees EC2 capacity in a specific Availability Zone.


### Pricing Comparison Example

| Option | Approximate Savings |
|----------|------------------|
| On-Demand | 0% |
| Reserved Instance | Up to 72% |
| Savings Plan | Up to 72% |
| Spot Instance | Up to 90% |
| Dedicated Host | No Savings |
| Dedicated Instance | Minimal Savings |
| Capacity Reservation | No Savings |
