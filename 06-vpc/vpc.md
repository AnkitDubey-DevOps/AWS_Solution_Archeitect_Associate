## VPC
A VPC works like a private network in the cloud, where you can control IP address range, subnets, routing, and network access for your AWS resources.

![Alt Text](https://media.geeksforgeeks.org/wp-content/uploads/20260518162525673075/external_network.webp)

## Subnets
A Subnet is a segment of a VPC’s IP address range that resides entirely within a single Availability Zone (AZ).
Subnets serve two primary purposes:

#### Segmentation: They divide large networks into smaller, isolated zones for better organization and control.

#### High Availability: Distributing subnets across multiple AZs ensures fault tolerance and operational continuity.
Subnets are typically categorized as:

#### Public Subnets, which route internet-bound traffic through an Internet Gateway (IGW).

#### Private Subnets, which are isolated from direct internet access and communicate externally through a NAT Gateway.
This distinction forms the foundation of secure, multi-tier cloud architectures.

## Route Tables
Each VPC contains an implicit virtual router that relies on Route Tables to direct traffic. Every subnet must be associated with exactly one route table, and each route defines:

A destination CIDR block (where the traffic is headed).

A target (where the traffic should be sent, such as an Internet Gateway, NAT Gateway, or another instance).

Route tables determine whether traffic remains internal to the VPC or is sent to external networks. They are the digital roadmap of the cloud network.

## Network Access Control Lists

NACLs are stateless firewalls that control inbound and outbound traffic at the subnet level.

They check each packet separately without remembering previous traffic information.

Every VPC has a default NACL that can be modified but not deleted.

Rules must be explicitly defined for both inbound and outbound traffic.

NACLs are used to apply security rules at the subnet level, such as blocking specific IP addresses or controlling access across multiple resources.

## Internet Gateway(IGW)
An Internet Gateway enables bidirectional communication between a VPC’s public subnets and the internet and is horizontally scaled and redundant.

Attaching an IGW to a VPC and adding a 0.0.0.0/0 route to it in a public subnet’s route table allows instances in that subnet to access the internet.

A VPC can have only one Internet Gateway, and without it, instances cannot directly communicate with the public internet.

## Network Address Translation (NAT)
NAT Gateways allow instances in private subnets to initiate outbound internet connections while blocking unsolicited inbound traffic.

They enable secure communication with external services without exposing private resources to the internet.

NAT Gateways are typically placed in public subnets and act as controlled egress points for private subnets.

## Security Groups
Security Groups are stateful firewalls that operate at the instance level, while NACLs operate at the subnet level.

They define granular inbound rules based on IP ranges, ports, and protocols, and automatically allow return traffic for initiated connections.

Security Groups provide the primary layer of network protection for EC2, RDS, and other VPC resources.

## Security Groups vs. Network ACLs (NACLs)

AWS provides two layers of firewalls. Understanding the difference is critical for security exams and real-world operations.

| Feature | Security Group (SG) | Network ACL (NACL) |
|----------|----------|----------|
| **Level** | **Instance Level** (Virtual Firewall for EC2). | **Subnet Level** (Firewall for the whole subnet). |
| **State** | **Stateful:** If you allow an inbound request, the outbound response is automatically allowed. | **Stateless:** You must explicitly allow both inbound and outbound traffic. |
| **Rules** | **Allow Only.** You cannot explicitly **"Deny"** an IP. | **Allow and Deny.** You can block specific IPs (e.g., a known attacker). |
| **Use Case** | Primary defense. Used for every resource. | Secondary defense. Used for blocking specific threats or creating DMZs. |

## What is a Bastion Host?

A Bastion Host is a special EC2 instance placed in a **Public Subnet** that allows administrators to securely access EC2 instances in **Private Subnets**.

## What is a NAT Instance?

A NAT Instance is an EC2 instance configured to provide Internet access for resources in a Private Subnet.

NAT = Network Address Translation

Allows EC2 instances in private subnets to connect to the Internet.

Must be launched in a public subnet

Must have Elastic IP attached to it.
