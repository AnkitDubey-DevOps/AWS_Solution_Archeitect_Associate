# AWS Load Balancing & Auto Scaling — Complete Notes

---

## 1. Horizontal Scalability

**Definition:** Adding **more instances/machines** to handle increased load (scale **out/in**).

- Example: Going from 1 server → 3 servers
- No downtime required
- Works great with distributed systems
- Common in modern cloud architecture (EC2, containers)

> "Add more machines" = Horizontal Scaling

---

## 2. Vertical Scalability

**Definition:** Increasing the **size/power of an existing instance** (scale **up/down**).

- Example: Upgrading from t2.micro → t2.large
- Has a hardware limit (you can't scale infinitely)
- Common for non-distributed systems like databases (RDS, ElastiCache)
- Usually involves downtime during upgrade

> "Make the machine bigger" = Vertical Scaling

---

## 3. High Availability (HA)

**Definition:** Running your application in **at least 2 Availability Zones (AZs)** so it survives a data center failure.

- Goal: **Survive disasters**, minimize downtime
- Works hand-in-hand with horizontal scaling
- Example: Auto Scaling Group across multi-AZ, Multi-AZ RDS

> HA = Survive AZ failures by spreading across multiple zones

---

## 4. Load Balancing

**Definition:** A **Load Balancer** is a server/service that **forwards incoming traffic** to multiple backend servers (EC2 instances, containers, etc.).

### Why use a Load Balancer?
- Spread load across multiple downstream instances
- Expose a **single DNS endpoint** to your application
- Seamlessly handle failures (health checks)
- Provide SSL termination (HTTPS)
- High availability across zones
- Separate public traffic from private instances

---

## 5. Elastic Load Balancing (ELB)

**Definition:** AWS **managed** load balancer service. AWS handles upgrades, maintenance, and high availability.

### Benefits:
- Integrated with AWS services: EC2, ECS, ACM, CloudWatch, Route 53, WAF, etc.
- Cheaper and less effort than setting up your own load balancer
- Automatically scales to handle traffic

---

## 6. Health Checks

**Definition:** Load balancers use health checks to know if instances are **healthy and able to receive traffic**.

- Done on a specific **port + route** (e.g., HTTP on port 4567 at `/health`)
- If response is **not HTTP 200 OK** → instance is marked **unhealthy**
- Unhealthy instances do **not** receive traffic

LB → GET /health → HTTP 200 OK ✅ (Healthy)
LB → GET /health → HTTP 500     ❌ (Unhealthy — removed from rotation)

---

## 7. Types of Load Balancers (4 Types)

### 7.1 Classic Load Balancer (CLB) — **Deprecated (v1)**
- Supports: HTTP, HTTPS, TCP, SSL
- Old generation (2009), AWS recommends moving away from it
- **Not recommended for new applications**

---

### 7.2 Application Load Balancer (ALB) — **Layer 7**
- Supports: **HTTP, HTTPS, WebSocket**
- Routes based on:
  - **URL path** → `/users` vs `/posts`
  - **Hostname** → `one.example.com` vs `other.example.com`
  - **Query strings / Headers** → `?platform=mobile`
- Great for **microservices & container-based apps** (ECS, Docker)
- Has a **port mapping** feature to redirect to dynamic ports in ECS
- Target Groups can be: EC2 instances, ECS tasks, Lambda functions, IP addresses
- Fixed hostname: `xxx.region.elb.amazonaws.com`
- Client IP is in header: `X-Forwarded-For`

/api/users  →  Target Group A (User Service)
/api/posts  →  Target Group B (Post Service)

---

### 7.3 Network Load Balancer (NLB) — **Layer 4**
- Supports: **TCP, TLS, UDP**
- Handles **millions of requests per second** with ultra-low latency (~100ms vs 400ms for ALB)
- Has **one static IP per AZ** (can attach Elastic IP)
- Used for:
  - Extreme performance requirements
  - Gaming, IoT, financial apps
- Target Groups: EC2 instances, IP addresses, **ALB**

---

### 7.4 Gateway Load Balancer (GWLB) — **Layer 3**
- Supports: **IP protocol**
- Used to deploy, scale, and manage **3rd party network virtual appliances**
  - Examples: Firewalls, Intrusion Detection/Prevention Systems (IDS/IPS), Deep Packet Inspection
- Combines: **Transparent Network Gateway + Load Balancer**
- Uses **GENEVE protocol on port 6081**

Users → GWLB → 3rd Party Security Appliances → GWLB → Application


---

## 8. Load Balancer Security Group

**Concept:** Control traffic to/from your load balancer using **Security Groups**.

### Typical Setup:


Internet
↓
[LB Security Group]
Inbound: Allow HTTP (80) / HTTPS (443) from 0.0.0.0/0
↓
[EC2 Security Group]
Inbound: Allow port 80 ONLY from LB Security Group (not internet)

### Why this pattern?
- EC2 instances only accept traffic **from the load balancer**, not directly from internet
- Improved security — instances are protected
- Reference SG by ID in rules (not CIDR)

---

## 9. Load Balancer — Deep Dive

### How It Works Internally:

Client → DNS (ELB hostname) → Load Balancer Nodes (per AZ) → Target Instances

### Key Concepts:

| Concept | Detail |
|---|---|
| **Listener** | Checks for connection requests on a defined port/protocol |
| **Rules** | Determine how to route requests (path, host, headers) |
| **Target Group** | Group of registered targets (EC2, ECS, Lambda, IP) |
| **Health Check** | Periodic check if target is available |
| **Deregistration** | Graceful removal of target from group |

### Request Flow (ALB Example):
1. Client sends request to `app.example.com`
2. DNS resolves to ALB
3. ALB **Listener** receives request on port 443
4. **Rules** evaluated (path `/api/users`)
5. Request forwarded to matching **Target Group**
6. Target Group sends to a **healthy EC2 instance**
7. Response returned through LB to client

### Important Notes:
- LB **does not** forward traffic to unhealthy instances
- Supports connection **keep-alive**
- Supports **slow start mode** (ALB) — gradually increase traffic to new targets
- Operates across **multiple AZs** for HA

---

## 10. Sticky Sessions (Session Affinity)

**Definition:** Ensures the **same client always goes to the same instance** behind the load balancer.

- Achieved using **cookies**
- Available in: **CLB, ALB, NLB**
- Use case: When session data is stored **locally on the instance** (not in a shared DB/cache)
- Downside: Can cause **load imbalance** if many users stick to one instance

### How it works:

Client → LB → Instance A (cookie set)
Client (same cookie) → LB → Instance A (always)

---

## 11. Session Affinity & Cookie Name

### Two Types of Cookies:

#### 11.1 Application-based Cookie
- **Custom Cookie:**
  - Generated by the **target (your app)**
  - Can include any custom attribute
  - Cookie name must be specified per target group
  - ⚠️ Don't use: `AWSALB`, `AWSALBAPP`, `AWSALBTG` (reserved by AWS)
- **Application Cookie:**
  - Generated by the **load balancer**
  - Cookie name: `AWSALBAPP`

#### 11.2 Duration-based Cookie
- Generated by the **load balancer**
- Cookie names:
  - ALB: `AWSALB`
  - CLB: `AWSELB`
- Expiry based on duration you set

---

## 12. Cross-Zone Load Balancing

**Definition:** Distributes traffic **evenly across all registered instances in all AZs**, regardless of which AZ the request arrived at.

### Without Cross-Zone LB:


AZ1 (LB node gets 50%) → 2 instances → each gets 25%
AZ2 (LB node gets 50%) → 8 instances → each gets 6.25%
(Uneven distribution!)


### With Cross-Zone LB:

All 10 instances → each gets 10% (even!)

### Per Load Balancer Type:

| LB Type | Cross-Zone Default | Charges |
|---|---|---|
| ALB | Always ON | No charge for inter-AZ |
| NLB / GWLB | Default OFF | You pay for inter-AZ data |
| CLB | Default OFF | No charge if enabled |

---

## 13. SSL/TLS Basics

### What is SSL/TLS?
- **SSL (Secure Sockets Layer)** / **TLS (Transport Layer Security)**
- Encrypts data **in transit** between client and load balancer
- TLS is the newer, more secure version (SSL is deprecated but term still used)

### How it works with ELB:
- LB uses an **X.509 certificate** (SSL/TLS server cert)
- Managed via **AWS Certificate Manager (ACM)**
- You can also upload your own certificate

### HTTPS Listener:
- Must specify a **default certificate**
- Can add optional list of certs to support **multiple domains**
- Clients use **SNI (Server Name Indication)** to specify which hostname they're connecting to
- Can set a policy for older SSL/TLS versions

### SNI (Server Name Indication):
- Allows **multiple SSL certs on one web server**
- Client specifies hostname in the TLS handshake
- LB finds the right certificate
- Supported by: **ALB, NLB** (not CLB — CLB supports only one cert)

---

## 14. Connection Draining

**Also called:** Deregistration Delay (ALB/NLB)

**Definition:** Time given to instances to **complete in-flight requests** before being deregistered or marked unhealthy.

- During draining: LB **stops sending new requests** to the draining instance
- Existing connections are allowed to complete
- Default: **300 seconds** (range: 1–3600 seconds)
- Can be **disabled** (set to 0) for quick shutdown

### Use Cases:
| App Type | Suggested Delay |
|---|---|
| Short-lived requests | Low value (30–60s) |
| Long-running uploads/operations | High value (300–600s) |


Instance deregistering → Existing connections complete → Instance removed
↑
[Draining Period]


---

## 15. Auto Scaling Group (ASG)

**Definition:** Automatically **adds (scale out) or removes (scale in) EC2 instances** based on demand.

### Goals:
- Scale out → add instances when load increases
- Scale in → remove instances when load decreases
- Ensure min/max number of instances
- Automatically register new instances with Load Balancer
- Re-create instances if one is terminated (self-healing)

### ASG Attributes:
- **Launch Template:** Defines AMI, instance type, key pair, SG, EBS, IAM role, user data, etc.
- **Min Size:** Minimum number of instances always running
- **Max Size:** Maximum number of instances allowed
- **Desired Capacity:** Target number of instances right now

---

## 16. Auto Scaling — CloudWatch Alarms & Scaling

**Definition:** ASG can trigger scaling actions based on **CloudWatch Alarms**.

- Alarms monitor metrics like: **Average CPU, Network In/Out, Request Count**
- Alarm triggers → ASG scales in or out

### Example:

CloudWatch Alarm: CPU > 70% for 5 min
→ ASG: Add 2 EC2 instances (Scale Out)
CloudWatch Alarm: CPU < 30% for 5 min
→ ASG: Remove 1 EC2 instance (Scale In)

### Metrics monitored:
- CPU Utilization
- RequestCountPerTarget
- Average Network I/O
- **Custom metrics** (pushed from app via CloudWatch API)

---

## 17. Auto Scaling — Scaling Policies

### 17.1 Dynamic Scaling

#### Target Tracking Scaling
- **Simplest to set up**
- Example: "Keep average CPU at 40%"
- ASG adjusts automatically to maintain the target

#### Simple / Step Scaling
- CloudWatch alarm triggers a defined action
- Example:
  - CPU > 70% → Add 2 units
  - CPU < 30% → Remove 1 unit
- Step scaling allows different actions at different thresholds

---

### 17.2 Scheduled Scaling
- Anticipate scaling based on **known usage patterns**
- Example: "Scale to 10 instances every Friday at 5 PM"
- Useful for predictable traffic spikes (business hours, events)

---

### 17.3 Predictive Scaling
- Uses **ML to forecast load** based on historical data
- Automatically provisions the right number of instances **in advance**
- Great for recurring patterns (daily/weekly cycles)

---

### Which Policy to Use?

| Policy | Best For |
|---|---|
| Target Tracking | Simple, metric-based (recommended default) |
| Step Scaling | Fine-grained control at thresholds |
| Scheduled | Predictable, time-based events |
| Predictive | Recurring traffic patterns |

---

## 18. Scaling Cooldown

**Definition:** After a scaling activity, ASG enters a **cooldown period** (default: **300 seconds**) during which **no further scaling actions** are taken.

### Why?
- Prevents ASG from launching/terminating instances **before metrics stabilize**
- Avoids over-scaling or rapid oscillation

### Tips:
- Use **ready-to-use AMIs** to reduce instance boot time → allows shorter cooldown
- Default cooldown: **300 seconds**
- Can set cooldown per scaling policy
