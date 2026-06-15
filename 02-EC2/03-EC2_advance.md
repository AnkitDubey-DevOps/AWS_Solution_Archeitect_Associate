## Private IP vs Public IP in AWS

### Introduction

Every device connected to a network needs an IP (Internet Protocol) address.

An IP address acts like a home address for a computer, allowing data to be sent and received over a network.

There are two main types of IP addresses:

1. Public IP
2. Private IP

## IPv4 vs IPv6

AWS supports both IPv4 and IPv6.

## Public IP Address

### What is a Public IP?

A Public IP is an IP address that is reachable over the internet.

Anyone on the internet can communicate with a device that has a public IP (assuming firewall rules allow it).

### Characteristics of Public IP

### Globally Unique

No two devices on the internet can have the same public IP.

### Internet Accessible

Public IPs can be reached from anywhere in the world.

### Geolocation

Public IPs can often be traced to:

## Private IP Address

### What is a Private IP?

A Private IP is an IP address used inside a private network.

It is not directly accessible from the internet.

## Characteristics of Private IP

### Internal Communication Only

Private IPs work only within a private network.

### Reusable

Multiple organizations can use the same private IP ranges.

### Public vs Private IP

| Feature | Public IP | Private IP |
|----------|------------|------------|
| Internet Accessible | Yes | No |
| Globally Unique | Yes | No |
| Reachable Anywhere | Yes | No |
| Used Inside VPC | Sometimes | Yes |
| Cost | May incur charges | Free |
| Security | Less Secure | More Secure |

### Elastic IP (EIP)

An Elastic IP is a static (fixed) public IPv4 address that belongs to your AWS account, not to a specific EC2 instance.

You can attach it to any EC2 instance whenever needed.

An Elastic IP can be associated with only one resource at a time.

### 4. Supports High Availability

Elastic IPs help in:

- Disaster Recovery (DR)
- Failover Architectures
- Bastion Hosts
- NAT Instances
- Legacy Applications requiring fixed IP addresses

### Elastic IP vs Public IP

| Feature | Public IP | Elastic IP |
|----------|-----------|------------|
| Changes after stop/start | Yes | No |
| Static | No | Yes |
| Transferable between instances | No | Yes |
| Suitable for production fixed access | No | Yes |
| Belongs to AWS account | No | Yes |

## What is a Placement Group?

A **Placement Group** is an AWS feature that controls **how EC2 instances are physically placed on AWS hardware**.

Normally, AWS decides where to place EC2 instances.

## Types of Placement Groups

AWS provides **3 types**:

1. Cluster Placement Group
2. Spread Placement Group
3. Partition Placement Group

### 1. Cluster Placement Group

### Definition

Places EC2 instances **very close together** inside the same Availability Zone.

### Architecture

```text
+--------------------------------+
|      Cluster Placement Group   |
|                                |
|  EC2-A  EC2-B  EC2-C  EC2-D    |
|                                |
+--------------------------------+
```

Instances are physically near each other.

### Benefits

- Very low network latency
- High throughput
- Fast communication

### 2. Spread Placement Group

### Definition

Places each EC2 instance on separate hardware.

### Architecture

```text
Availability Zone

Rack A --> EC2-A

Rack B --> EC2-B

Rack C --> EC2-C

Rack D --> EC2-D
```

Each instance is isolated.

### 3. Partition Placement Group

### Definition

Instances are divided into multiple partitions.

Each partition uses separate hardware.

### Architecture

```text
Partition 1
 ├─ EC2-A
 ├─ EC2-B
 └─ EC2-C
```

### Comparison Table

| Feature | Cluster | Spread | Partition |
|----------|----------|----------|----------|
| Goal | Performance | Availability | Performance + Availability |
| Physical Placement | Close Together | Separate Hardware | Multiple Partitions |
| Latency | Lowest | Higher | Medium |
| Throughput | Highest | Medium | Medium |
| Fault Isolation | Low | Highest | High |
| Typical Workloads | HPC, ML | Critical Servers | Hadoop, Kafka, Cassandra |

## What is an ENI?

An **Elastic Network Interface (ENI)** is a **virtual network card (NIC)** attached to an EC2 instance.

Think of it as the network adapter of a computer.

Without a network card, a computer cannot communicate.
Similarly, EC2 instances use ENIs to communicate within AWS networks.

### Architecture Example

```text
+------------------+
|   EC2 Instance   |
+------------------+
          |
          |
+------------------+
|       ENI        |
+------------------+
| Private IP       |
| Security Group   |
| MAC Address      |
+------------------+
```

### Primary ENI

Every EC2 instance gets a default ENI.
Cannot be detached while the instance is running.

### Secondary ENI

You can attach additional ENIs.

### Why Use Multiple ENIs?

Example:

```text
Application Server

ENI-1 --> Public Traffic
ENI-2 --> Database Traffic
```

Helps separate network traffic.

## What is EC2 Hibernate?

EC2 Hibernate allows an instance to be stopped while preserving everything in RAM (memory).

EC2 Hibernate = Save RAM to EBS and resume later from the same state.

---
