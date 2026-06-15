# AWS Security Groups

## What is a Security Group?

A Security Group (SG) is a virtual firewall that controls inbound and outbound traffic for AWS resources such as EC2 instances.

Think of a Security Group as a gatekeeper that decides:

- Who can connect to your server
- Which ports are accessible
- Which traffic is allowed to leave the server

> Security Groups are the first line of network security in AWS.

---

# Why Do We Need Security Groups?

Imagine you launch a web server on AWS.

Without Security Groups:

```text
Internet
    |
    v
EC2 Instance
```

Anyone can attempt to connect.

With Security Groups:

```text
Internet
    |
    v
Security Group
    |
    v
EC2 Instance
```

The Security Group filters traffic before it reaches the EC2 instance.

---

# Security Group Fundamentals

## Security Groups Contain Only ALLOW Rules

Unlike traditional firewalls:

❌ No DENY rules

✅ Only ALLOW rules

Example:

```text
Allow SSH (22)
Allow HTTP (80)
Allow HTTPS (443)
```

Everything else is automatically blocked.

---

## Security Groups Are Stateful

One of the most important interview concepts.

### What Does Stateful Mean?

If inbound traffic is allowed, the response is automatically allowed.

Example:

```text
Your Laptop
     |
 SSH Port 22
     |
     v
EC2 Instance
```

When EC2 sends a response back:

```text
EC2 Instance
     |
     v
Your Laptop
```

AWS automatically allows the return traffic.

You do NOT need an outbound rule for the response.

---

# Security Group Traffic Flow

## Inbound Traffic

Traffic coming TO the EC2 instance.

Examples:

- SSH connection
- HTTP request
- HTTPS request
- Database connection

```text
Internet
    |
    v
Security Group
    |
    v
EC2 Instance
```

---

## Outbound Traffic

Traffic leaving the EC2 instance.

Examples:

- Downloading software
- Calling APIs
- Connecting to databases
- Sending emails

```text
EC2 Instance
    |
    v
Security Group
    |
    v
Internet
```

---

# Security Group Rules

A Security Group rule consists of:

| Component | Description |
|------------|-------------|
| Type | SSH, HTTP, HTTPS, Custom TCP |
| Protocol | TCP, UDP, ICMP |
| Port | Network Port |
| Source/Destination | Allowed IP Range |
| Description | Optional Notes |

Example:

| Type | Protocol | Port | Source |
|--------|----------|--------|---------|
| SSH | TCP | 22 | My IP |
| HTTP | TCP | 80 | 0.0.0.0/0 |
| HTTPS | TCP | 443 | 0.0.0.0/0 |

---

# Understanding Security Group Rules

## Example 1: SSH Access

Rule:

```text
Type: SSH
Port: 22
Source: 122.149.196.85/32
```

Meaning:

Only that specific IP address can SSH into the server.

```text
122.149.196.85
      |
      v
     EC2
```

Any other IP:

```text
Blocked
```

---

## Example 2: Public Website

Rule:

```text
Type: HTTP
Port: 80
Source: 0.0.0.0/0
```

Meaning:

Anyone on the internet can access the website.

```text
Internet
    |
    v
 Website
```

---

# Common Security Group Example

A Linux Web Server usually requires:

| Type | Port | Source |
|--------|--------|---------|
| SSH | 22 | Your IP |
| HTTP | 80 | 0.0.0.0/0 |
| HTTPS | 443 | 0.0.0.0/0 |

Visualization:

```text
                Internet
                    |
      -----------------------------
      |             |             |
      v             v             v
   SSH 22       HTTP 80      HTTPS 443
      |             |             |
      --------------------------------
                    |
                    v
              EC2 Instance
```

---

# Security Groups Deep Dive

Security Groups control:

## 1. Port Access

Example:

```text
22  -> SSH
80  -> HTTP
443 -> HTTPS
3306 -> MySQL
5432 -> PostgreSQL
```

---

## 2. Authorized IP Addresses

Example:

Allow:

```text
192.168.1.10
```

Block:

```text
All Other IPs
```

---

## 3. Inbound Network Traffic

Controls:

```text
Who Can Reach My Server?
```

---

## 4. Outbound Network Traffic

Controls:

```text
Where Can My Server Connect?
```
# Security Group Diagram

```text
                  INTERNET
                       |
                       |
                +-------------+
                | Security SG |
                +-------------+
                       |
          ---------------------------
          |            |            |
          v            v            v
       Port 22      Port 80     Port 443
        SSH         HTTP        HTTPS
                       |
                       v
                 EC2 Instance
```

# Classic Ports Every DevOps Engineer Should Know

| Port | Protocol | Purpose |
|--------|----------|----------|
| 20/21 | FTP | File Transfer |
| 22 | SSH | Linux Remote Access |
| 22 | SFTP | Secure File Transfer |
| 23 | Telnet | Remote Access |
| 25 | SMTP | Mail Sending |
| 53 | DNS | Name Resolution |
| 80 | HTTP | Web Traffic |
| 110 | POP3 | Email Retrieval |
| 143 | IMAP | Email Retrieval |
| 443 | HTTPS | Secure Web Traffic |
| 3306 | MySQL | Database |
| 5432 | PostgreSQL | Database |
| 6379 | Redis | Cache |
| 27017 | MongoDB | Database |
| 3389 | RDP | Windows Remote Desktop |

---

# SSH into EC2

SSH allows remote access to Linux servers.

```text
Your Laptop
      |
 SSH Port 22
      |
      v
EC2 Instance
```

---

## Linux / Mac

Connect using:

```bash
ssh -i my-key.pem ec2-user@PUBLIC_IP
```

Example:

```bash
ssh -i my-key.pem ec2-user@54.123.45.67
```

---

## Windows

Common Tools:

- PuTTY
- Windows Terminal
- OpenSSH

Example:

```powershell
ssh -i my-key.pem ec2-user@54.123.45.67
```

---

# Troubleshooting Security Groups

## Scenario 1

Error:

```text
Connection Timed Out
```

Usually means:

✅ Security Group Issue

Check:

- Port Open?
- Correct Source IP?
- Correct Protocol?

---

## Scenario 2

Error:

```text
Connection Refused
```

Usually means:

✅ Application Issue

Security Group is open but:

- Application not running
- Service crashed
- Wrong port configured
