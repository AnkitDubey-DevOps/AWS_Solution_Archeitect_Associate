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
