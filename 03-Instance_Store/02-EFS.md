# AWS EFS (Elastic File System)

---

## What is EFS?

**Amazon EFS (Elastic File System)** is a fully managed, elastic, serverless file storage service that can be mounted simultaneously on multiple EC2 instances.

Think of EFS as a **shared network drive** that many servers can access at the same time.

---

## Simple Analogy

Imagine a shared company folder:

```text
Employees
    |
    v
Shared Drive
```

Everyone can access the same files.

Similarly:

```text
EC2 Instances
      |
      v
      EFS
```

Multiple EC2 instances can read and write the same data.

---

## Key Characteristics

- Shared File Storage
- Multiple EC2 instances can access simultaneously
- Automatically grows and shrinks
- Fully managed by AWS
- Highly available
- Multi-AZ by design
- Uses NFS protocol

---

## EFS Architecture Diagram

```text
                +------------+
                |   EFS      |
                +------------+
                  /    |    \
                 /     |     \
                /      |      \
               /       |       \
              v        v        v

       +---------+ +---------+ +---------+
       | EC2-A   | | EC2-B   | | EC2-C   |
       +---------+ +---------+ +---------+

        All instances access
          the same files
```

---

## Multi-AZ Architecture

```text
Availability Zone A
     |
+-----------+
| EC2-A     |
+-----------+
      |
      |
      +------------------+
                         |
                         v
                    +---------+
                    |   EFS   |
                    +---------+
                         ^
                         |
      +------------------+
      |
+-----------+
| EC2-B     |
+-----------+

Availability Zone B
```

EFS is designed to be available across multiple Availability Zones.

---

## How EFS Works

Suppose:

```text
EC2-A creates:

report.pdf
```

Stored in:

```text
EFS
```

Now:

```text
EC2-B
EC2-C
```

can immediately access the same file.

---

## Use Case 1: Shared Web Content

### Problem

Multiple web servers need the same images and files.

```text
Web Server 1
Web Server 2
Web Server 3
```

All need access to:

```text
logo.png
images/
videos/
```

### Solution

```text
          EFS
           |
  -------------------
  |       |         |
  v       v         v

EC2-1  EC2-2   EC2-3
```

All servers use the same shared files.

---

## Use Case 2: Content Management Systems

Examples:

- WordPress
- Drupal
- Joomla

Architecture:

```text
Users
   |
Load Balancer
   |
----------------
|              |
v              v

EC2-A       EC2-B
     \      /
      \    /
       \  /
        EFS
```

Uploaded files are instantly available to all servers.

---

## EFS Performance Modes

EFS provides performance modes that control latency and throughput.

---

### 1. General Purpose (Default)

Best for most workloads.

### Features

- Low latency
- Fast response time

### Use Cases

- Web applications
- CMS systems
- Home directories

---

## 2. Max I/O

Supports massive scale.

### Features

- Higher throughput
- Higher latency

### Use Cases

- Big data analytics
- Large-scale processing

---

# EFS Throughput Modes

---

## 1. Elastic Throughput

AWS automatically adjusts throughput.

### Best For

Most workloads.

---

## 2. Provisioned Throughput

Specify throughput manually.

### Best For

Predictable workloads.

---

## EFS Storage Classes

---

### 1. EFS Standard

Frequently accessed files.

#### Characteristics

- Highest performance
- Lowest latency

#### Example

```text
Website Files
Application Data
User Uploads
```

---

### 2. EFS Infrequent Access (EFS IA)

Files accessed less often.

#### Characteristics

- Lower storage cost
- Retrieval charges apply

#### Example

```text
Old Reports
Historical Data
```

---

### 3. EFS Archive

Lowest cost storage class.

#### Characteristics

- Rarely accessed files
- Long retrieval time

#### Example

```text
Compliance Records
Archived Logs
```

---

#### Lifecycle Management

EFS can automatically move files:

```text
EFS Standard
      |
      v
EFS IA
      |
      v
EFS Archive
```

This reduces storage costs.

---

### EBS vs EFS

| Feature | EBS | EFS |
|----------|----------|----------|
| Storage Type | Block Storage | File Storage |
| Access Method | Attached as Disk | Mounted as Shared File System |
| Multiple EC2 Access | No (except Multi-Attach) | Yes |
| Storage Scaling | Manual | Automatic |
| Availability | Single AZ | Multi-AZ |
| Protocol | Block Device | NFS |
| Best For | Databases, OS, Applications | Shared Files |
| Performance | Very High | High |
| Shared Storage | Limited | Native |
| Snapshot Support | Yes | AWS Backup |
| Mount on Multiple Servers | No | Yes |
