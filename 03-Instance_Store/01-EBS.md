## What is EBS?

**Amazon Elastic Block Store (EBS)** is a persistent block storage service for EC2 instances.

Think of EBS as a **virtual hard disk** attached to an EC2 instance.

- Data persists even after instance stop/start.
- Can be detached and attached to another EC2 instance (within same AZ).
- Used to store operating systems, applications, databases, and files.

---

# EBS Volume Example

```text
+-------------------+
|   EC2 Instance    |
+-------------------+
          |
          |
     EBS Volume
    (Virtual Disk)
          |
          |
      Data Stored
```

---

# Real Architecture Example

```text
+-------------------+
|   EC2 Instance    |
|   Web Server      |
+-------------------+
          |
          |
+-------------------+
|  EBS Volume 100GB |
+-------------------+
| Operating System  |
| Application Files |
| Logs              |
| Database Files    |
+-------------------+
```

---

# Key Characteristics

- Persistent Storage
- AZ Scoped
- Can be detached/attached
- Supports encryption
- Supports snapshots
- High durability
- Used as Root Volume

---

# EBS Delete on Termination Attribute

This setting controls what happens to the EBS volume when the EC2 instance is terminated.

---

## Root Volume (Default)

```text
EC2 Terminated
      |
Root EBS Deleted
```

Default:

```text
DeleteOnTermination = TRUE
```

---

## Additional EBS Volumes

Default:

```text
DeleteOnTermination = FALSE
```

Volume survives.

```text
EC2 Terminated
      |
Additional EBS Remains
```

---

## Example

```text
EC2
 |
 +-- Root Volume (Deleted)
 |
 +-- Data Volume (Retained)
```

---

# EBS Snapshots

## What is a Snapshot?

An EBS Snapshot is a backup of an EBS volume stored in Amazon S3.

```text
EBS Volume
      |
Create Snapshot
      |
Stored in S3
```

---

# Snapshot Architecture

```text
+------------+
| EBS Volume |
+------------+
       |
       |
 Snapshot
       |
       v
 Amazon S3
```

---

# Snapshot Features

### Incremental Backup

Only changed blocks are copied.

```text
Snapshot 1
100 GB

Snapshot 2
Only Changed Blocks
```

Reduces storage costs.

---

### Point-in-Time Backup

Captures volume state at a specific moment.

---

### Disaster Recovery

Restore volumes quickly.

---

### Cross-Region Copy

```text
Mumbai Region
      |
Copy Snapshot
      |
Singapore Region
```

Useful for DR.

---

### Create New Volumes

```text
Snapshot
    |
New EBS Volume
```

---

### Used for AMI Creation

AMI uses snapshots internally.

---

# Snapshot Workflow

```text
EBS Volume
      |
      v
 Snapshot
      |
      v
 New Volume
      |
      v
 New EC2 Instance
```

---

# AMI (Amazon Machine Image)

## What is an AMI?

An AMI is a template used to launch EC2 instances.

Contains:

- Operating System
- Application Software
- Configuration
- EBS Snapshot Information

---

# AMI Architecture

```text
AMI
 |
 +-- Operating System
 +-- Application
 +-- Configuration
 +-- EBS Snapshot
```

---

# Launch Process

```text
AMI
 |
Launch
 |
EC2 Instance
```

---

# Why Use AMIs?

### Fast Deployment

Launch identical servers quickly.

### Standardization

Same configuration everywhere.

### Auto Scaling

Used by Auto Scaling Groups.

### Backup & Recovery

Launch replacement servers rapidly.

---

# EC2 Instance Store

## What is Instance Store?

Instance Store is temporary storage physically attached to the host machine.

Also called:

```text
Ephemeral Storage
```

---

# Architecture

```text
+-------------------+
|   EC2 Instance    |
+-------------------+
          |
          |
 Instance Store
 (Local Disk)
```

---

# Key Characteristics

### Extremely Fast

Physically attached to server.

### Temporary Storage

Data lost if:

```text
Instance Stops
Instance Terminates
Hardware Failure
```

---

# Instance Store vs EBS

| Feature | EBS | Instance Store |
|----------|----------|----------|
| Persistent | Yes | No |
| Survives Stop/Start | Yes | No |
| Backup via Snapshot | Yes | No |
| Can Detach | Yes | No |
| Performance | High | Very High |
| Data Durability | High | Low |

---

# Instance Store Use Cases

### Cache Servers

```text
Redis
Memcached
```

### Temporary Processing

```text
Video Rendering
Image Processing
```

### Scratch Space

Temporary data.

### Buffers

Short-lived storage.

---

# EBS Volume Types

AWS provides two categories:

```text
SSD Volumes
HDD Volumes
```

---

# 1. General Purpose SSD (gp3)

Most commonly used volume.

### Features

- Low latency
- Cost effective
- Independent IOPS and throughput

### Use Cases

- Web Servers
- Application Servers
- Development Environments
- Small/Medium Databases

---

# 2. General Purpose SSD (gp2)

Older generation SSD.

### Use Cases

- Existing workloads
- Legacy deployments

**gp3 is preferred for new workloads.**

---

# 3. Provisioned IOPS SSD (io2)

High-performance SSD.

### Features

- Very high IOPS
- Mission-critical workloads
- Highest durability

### Use Cases

- Oracle Database
- SQL Server
- SAP
- Financial Systems

---

# 4. Provisioned IOPS SSD (io1)

Older generation of io2.

### Use Cases

Legacy high-performance applications.

---

# 5. Throughput Optimized HDD (st1)

Low-cost HDD optimized for throughput.

### Use Cases

- Big Data
- Hadoop
- Log Processing
- Data Warehouses

---

# 6. Cold HDD (sc1)

Lowest-cost HDD.

### Use Cases

- Infrequently accessed data
- Archives
- Long-term storage

---

# Quick Exam Table

| Volume Type | Storage | Best For |
|-------------|----------|----------|
| gp3 | SSD | General Purpose |
| gp2 | SSD | Legacy General Purpose |
| io2 | SSD | Critical Databases |
| io1 | SSD | Legacy High IOPS |
| st1 | HDD | Big Data / Throughput |
| sc1 | HDD | Archive Storage |

---

# EBS Multi-Attach

## What is Multi-Attach?

Allows a single EBS volume to be attached to multiple EC2 instances simultaneously.

Supported on:

```text
io1
io2
```

---

# Architecture

```text
           +-----------+
           | EC2-A     |
           +-----------+
                 |
                 |
          +-------------+
          | EBS Volume  |
          +-------------+
                 |
                 |
           +-----------+
           | EC2-B     |
           +-----------+
```

---

# Use Cases

### Clustered Applications

- Oracle RAC
- Shared Storage Clusters

### High Availability Systems

Multiple instances access same storage.

---

# Exam Tip

If question says:

> Same EBS volume must be attached to multiple EC2 instances

Answer:

```text
EBS Multi-Attach
(io1/io2 only)
```

---

# EBS Encryption

## What is EBS Encryption?

Protects data stored in EBS volumes using AWS KMS.

Encryption happens automatically.

---

# What Gets Encrypted?

### Data at Rest

```text
Stored Data
```

### Data in Transit

```text
EC2 <--> EBS
```

### Snapshots

```text
Encrypted Volume
      |
Encrypted Snapshot
```

### New Volumes Created from Snapshots

Remain encrypted.

---

# Architecture

```text
EC2
 |
Encrypted EBS
 |
AWS KMS Key
```

---

# Benefits

### Security

Protects sensitive data.

### Compliance

Helps meet security standards.

### Automatic

No application changes required.

---

# Exam Tips

### EBS Encryption Uses

```text
AWS KMS
```

### Encryption Covers

- Volume Data
- Snapshots
- Data in Transit
- Volumes Created from Snapshots
