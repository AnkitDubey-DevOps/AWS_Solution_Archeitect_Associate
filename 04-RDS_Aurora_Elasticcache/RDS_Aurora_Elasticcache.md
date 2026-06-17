# AWS RDS, Aurora & ElastiCache — Complete Notes

---

## 1. What is RDS?

**Amazon RDS (Relational Database Service)** is a **managed database service** by AWS for databases that use SQL as a query language.

### Supported Engines:
- PostgreSQL
- MySQL
- MariaDB
- Oracle
- Microsoft SQL Server
- IBM DB2
- **Amazon Aurora** (AWS proprietary)

---

## 2. Advantages of RDS (vs self-managed DB on EC2)

| Feature | RDS (Managed) | Self-managed EC2 |
|---|---|---|
| OS patching | ✅ AWS handles | ❌ You handle |
| Backups | ✅ Automated | ❌ Manual |
| Point-in-time restore | ✅ Built-in | ❌ Complex |
| Monitoring dashboards | ✅ CloudWatch | ❌ DIY |
| Multi-AZ (Disaster Recovery) | ✅ One click | ❌ Complex setup |
| Read Replicas | ✅ Easy setup | ❌ Manual replication |
| Vertical/Horizontal scaling | ✅ Easy | ❌ Manual |
| EBS-backed storage | ✅ Auto-managed | ❌ You manage |

> ⚠️ You **cannot SSH** into RDS instances — it's fully managed by AWS.

---

## 3. RDS Auto Scaling (Storage)

**Problem:** You may run out of DB storage as your data grows.

**Solution:** RDS **Storage Auto Scaling** automatically increases storage when running low.

### How it works:
- You set a **Maximum Storage Threshold**
- RDS automatically scales if:
  - Free storage < 10% of allocated storage
  - Low storage lasts > 5 minutes
  - 6 hours have passed since last modification

### Supports:
All RDS DB engines (MySQL, PostgreSQL, MariaDB, Oracle, SQL Server)

```
Initial: 20 GB allocated
  → Data grows, free < 10%
    → Auto Scales to 25 GB → 30 GB → ... up to Max Threshold
```

---

## 4. RDS Read Replicas — Read Scalability

**Problem:** Your primary DB is overwhelmed with read requests.

**Solution:** Create **Read Replicas** — copies of your primary DB that serve **SELECT** (read) queries.

```
         ┌──────────────┐
         │  Application  │
         └──────┬───────┘
                │
      ┌─────────┼──────────┐
      ▼         ▼          ▼
  [Primary]  [Read     [Read
  (Writes)   Replica]  Replica]
             (Reads)   (Reads)
```

- Up to **15 Read Replicas**
- Can be within AZ, cross-AZ, or **cross-region**
- Replication is **ASYNC** → reads are **eventually consistent**
- Replicas can be **promoted** to their own standalone DB

---

## 5. RDS Read Replicas — Use Cases

### ✅ Reporting / Analytics
```
Normal App → Primary DB (CRUD)
Reporting App → Read Replica (SELECT only — no impact on prod)
```

### ✅ Read-heavy workloads
- News websites, dashboards, leaderboards

### ✅ Disaster Recovery preparation
- Promote a replica to primary if main DB fails

---

## 6. RDS Read Replicas — Network Cost

AWS charges for data transfer **across AZs/Regions**.

| Replication Type | Network Cost |
|---|---|
| Same Region, different AZ | ✅ **FREE** (AWS waives for RDS replication) |
| Cross-Region | 💲 **You pay** for data transfer |

> Use same-region replicas when possible to avoid extra charges.

---

## 7. RDS Multi-AZ — Disaster Recovery

**Purpose:** High Availability and disaster recovery — **NOT for scaling reads**.

### How it works:
```
         Application
              │
              ▼
       ┌─────────────┐
       │  Primary DB  │  ← AZ 1 (Active)
       │   (Read/Write)│
       └──────┬───────┘
         SYNC │ Replication
              ▼
       ┌─────────────┐
       │  Standby DB  │  ← AZ 2 (Passive)
       │   (No reads!) │
       └─────────────┘
```

- Replication is **SYNC** (data written to both at the same time)
- **One DNS name** — automatic failover to standby if primary fails
- Failover triggers on: AZ failure, instance failure, network issue, storage failure
- **No manual intervention** needed
- Standby is **NOT used for reads or writes** — only for failover

---

## 8. RDS — From Single-AZ to Multi-AZ

**Zero downtime operation** — no need to stop the DB.

### Steps (internal):
```
1. Snapshot taken of primary DB
2. Restored in new AZ as standby
3. Sync established between primary and standby
4. Multi-AZ now active ✅
```

Just click **"Modify"** → enable Multi-AZ → done!

---

## 9. RDS Custom

**Available for:** Oracle and Microsoft SQL Server only.

**Purpose:** Access to underlying OS and DB for advanced customization (not possible in standard RDS).

### RDS Custom allows you to:
- Configure settings at OS level
- Install custom patches
- Enable native features
- Access underlying EC2 instance via SSH or SSM

### RDS vs RDS Custom vs EC2 Self-managed:

| Feature | RDS | RDS Custom | EC2 Self-Managed |
|---|---|---|---|
| AWS manages OS | ✅ | Partial | ❌ |
| SSH/SSM access | ❌ | ✅ | ✅ |
| Custom OS config | ❌ | ✅ | ✅ |
| Managed backups | ✅ | ✅ | ❌ |

> ⚠️ Deactivate automation mode before customizing to avoid AWS interference.

---

---

# Amazon Aurora

## 10. What is Amazon Aurora?

**Aurora** is AWS's **proprietary, cloud-optimized** relational database — compatible with MySQL and PostgreSQL.

- **5x performance** over MySQL on RDS
- **3x performance** over PostgreSQL on RDS
- Storage auto-grows from **10 GB to 128 TB** in 10 GB increments
- Costs ~20% more than RDS — but far more efficient

---

## 11. Aurora High Availability & Read Scaling

Aurora stores **6 copies of your data across 3 AZs** automatically.

```
          Writer Endpoint
               │
        ┌──────▼───────┐
        │  Primary      │
        │  Instance     │ ── AZ 1
        └──────┬────────┘
               │  (Auto-replication to 6 copies across 3 AZs)
   ┌───────────┼───────────┐
   ▼           ▼           ▼
[AZ 1]      [AZ 2]      [AZ 3]
 Copy 1,2   Copy 3,4   Copy 5,6

        Read Replicas (up to 15)
        ←──────────────────────→
               Reader Endpoint
               (Load balanced)
```

### Resilience:
- 4 out of 6 copies needed for **writes**
- 3 out of 6 copies needed for **reads**
- Self-healing with peer-to-peer replication
- Storage striped across 100s of volumes

### Failover:
- If primary fails → one replica is promoted in < **30 seconds**
- Cross-region replication supported

---

## 12. Features of Aurora

| Feature | Detail |
|---|---|
| Automatic failover | < 30 seconds |
| Backup & Recovery | Continuous, automatic |
| Isolation & Security | VPC, encryption at rest & transit |
| Industry compliance | HIPAA, SOC, PCI |
| Auto Scaling | Read replicas auto-scale |
| Automated patching | Zero downtime |
| Advanced monitoring | CloudWatch + Performance Insights |
| Routine maintenance | Minimal impact |
| Backtrack | Restore to any point in time without backups |

---

## 13. Aurora Auto Scaling & Custom Endpoints

### Auto Scaling (Read Replicas):
- Define min/max number of replicas
- Aurora adds/removes replicas based on load (e.g., CPU, connections)
- Uses **Reader Endpoint** which auto-includes new replicas

```
Low load: 2 replicas
High load: 5 replicas (auto-added)
Low load again: Back to 2 (auto-removed)
```

### Custom Endpoints:
Define a **subset of Aurora instances** as a custom endpoint for specific workloads.

```
Reader Endpoint (all replicas)
Custom Endpoint A → [db.r5.2xlarge, db.r5.2xlarge] ← Analytics queries
Custom Endpoint B → [db.r5.large, db.r5.large]     ← Regular app queries
```

- After custom endpoints, the default Reader Endpoint is less used
- Lets you run heavy analytics on powerful replicas without affecting regular app

---

## 14. Aurora Serverless

**Purpose:** Automatically starts, shuts down, and scales up/down capacity based on actual usage — **no capacity planning needed**.

### How it works:
```
Client → Proxy Fleet (managed by Aurora)
              │
              ▼
    [Aurora Instance] ← spun up/down automatically
```

- Pay per second — **cost-effective for infrequent workloads**
- Great for:
  - Development/test environments
  - Unpredictable traffic
  - Multi-tenant SaaS apps

---

## 15. Global Aurora

**Purpose:** Low-latency global reads and disaster recovery across regions.

### Architecture:
```
Primary Region (us-east-1)
  └── 1 Primary DB cluster (Read/Write)
        │
        │ Replication lag < 1 second
        ▼
Secondary Region (eu-west-1)
  └── Up to 16 Read Replicas (Read only)

Secondary Region (ap-southeast-1)
  └── Up to 16 Read Replicas (Read only)
```

- Up to **5 secondary regions**
- Each secondary region has up to **16 read replicas**
- Replication lag < **1 second**
- If primary region fails → promote secondary in < **1 minute** (RTO)

---

## 16. Aurora Machine Learning

**Purpose:** Add ML predictions to your SQL queries without moving data.

### Supported services:
- **Amazon SageMaker** — any ML model
- **Amazon Comprehend** — sentiment analysis

### How it works:
```
App → Aurora SQL query
       → Aurora sends data to SageMaker/Comprehend
         → ML result returned as SQL column
```

### Use Cases:
- Fraud detection
- Ads targeting
- Sentiment analysis
- Product recommendations

```sql
-- Example (conceptual)
SELECT user_id, aws_comprehend_detect_sentiment(review_text, 'en') 
FROM reviews;
```

---

## 17. Babelfish for Aurora PostgreSQL

**Purpose:** Allows applications written for **Microsoft SQL Server** to run on Aurora PostgreSQL **with minimal code changes**.

### How it works:
```
SQL Server App (T-SQL)
       │
       ▼
Babelfish Layer on Aurora PostgreSQL
       │
       ▼
Aurora PostgreSQL (stores data)
```

- Understands **T-SQL** (SQL Server dialect)
- Supports **SQL Server wire protocol (TDS)**
- Migrate SQL Server apps → Aurora PostgreSQL without rewriting queries
- Reduces migration cost and complexity

---

---

# RDS & Aurora Backups

## 18. RDS Backup

### Automated Backups:
- Daily full backup (during maintenance window)
- Transaction logs backed up every **5 minutes**
- Point-in-time restore → from oldest backup to **5 minutes ago**
- Retention: **1 to 35 days** (set to 0 to disable)

### Manual DB Snapshots:
- Triggered by user manually
- **Retained as long as you want** (even after DB deletion)
- Good practice: snapshot before deleting an RDS instance

> 💡 Tip: If you stop RDS, you still pay for storage. Snapshot + delete is cheaper for long pauses.

---

## 19. Aurora Backup

### Automated Backups:
- **Cannot be disabled**
- Retention: **1 to 35 days**
- Point-in-time recovery within retention window

### Manual Snapshots:
- Retained as long as you want

---

## 20. RDS & Aurora Restore Options

| Option | Description |
|---|---|
| Restore from snapshot | Creates a **new DB** from snapshot |
| Point-in-time restore | Restore DB to any second in retention window |
| Restore from S3 (MySQL) | Backup on-prem MySQL → upload to S3 → restore to RDS |
| Restore Aurora from S3 | Backup on-prem MySQL → S3 → restore to Aurora MySQL cluster |

### Restoring MySQL RDS from S3:
```
On-prem MySQL DB
  → Backup using Percona XtraBackup
    → Upload to S3
      → Restore to new RDS MySQL instance
```

---

## 21. Aurora Database Cloning

**Purpose:** Create a new Aurora cluster from an existing one — **faster than snapshot + restore**.

### How it works:
- Uses **copy-on-write protocol**
- Initially shares the same data volume as source
- Only writes new data when changes occur → very fast and cost-effective

```
Production Aurora Cluster
        │
        ▼  (clone — instant!)
Staging Aurora Cluster (separate, safe to test on)
```

### Use case:
- Create a staging environment from production data **without impacting prod**
- Useful for testing, analytics, debugging

---

---

# RDS & Aurora Security

## 22. RDS and Aurora Security

### At-rest Encryption:
- Database and snapshots encrypted using **AWS KMS** (AES-256)
- Must be defined at **launch time**
- If primary is not encrypted, read replicas **cannot** be encrypted
- To encrypt an existing unencrypted DB:
  ```
  Snapshot → Copy snapshot with encryption enabled → Restore from encrypted snapshot
  ```

### In-transit Encryption:
- TLS-ready by default
- Use AWS TLS root certificates on client side

### IAM Authentication:
- Connect to DB using **IAM roles** instead of username/password
- Supported by: MySQL and PostgreSQL
- Uses an **auth token** (valid 15 minutes) generated via IAM

### Network Security:
- Deploy in **VPC** (private subnets, not publicly accessible)
- Control access with **Security Groups**
- No SSH access (except RDS Custom)
- Audit logs can be sent to **CloudWatch Logs**

---

## 23. Amazon RDS Proxy

**Purpose:** A fully managed **database proxy** that sits between your app and RDS.

### Why use RDS Proxy?
- **Reduces DB connections** by pooling and sharing them
- Reduces stress on DB (CPU, RAM, open connections)
- **Serverless apps** (Lambda) open/close many DB connections → RDS Proxy prevents connection storms
- Automatic failover is **60% faster** with RDS Proxy
- Enforce **IAM authentication** for DB access
- Never publicly accessible — must be accessed from **within VPC**

### Architecture:
```
Lambda Functions (hundreds of instances)
   │  │  │  │  │
   ▼  ▼  ▼  ▼  ▼
 ┌──────────────────┐
 │   RDS Proxy       │  ← Connection Pooling
 └────────┬─────────┘
          │
    ┌─────▼──────┐
    │  RDS / Aurora │
    └─────────────┘
```

### Supports:
- MySQL, PostgreSQL, MariaDB, SQL Server
- Aurora MySQL, Aurora PostgreSQL

---

---

# Amazon ElastiCache

## 24. What is ElastiCache?

**ElastiCache** is a managed **in-memory cache** service by AWS.

- Supports **Redis** and **Memcached**
- Extremely high performance, **sub-millisecond latency**
- Reduces load on databases for read-heavy workloads
- AWS manages: OS patching, optimization, setup, config, monitoring, backups, failure recovery

> Using ElastiCache requires **application code changes** to query the cache before the DB.

---

## 25. ElastiCache Solution Architecture

### Pattern 1: DB Cache
```
App → ElastiCache? ──hit──→ Return cached result
           │
          miss
           │
           ▼
        RDS DB → Write result to ElastiCache → Return to App
```
- Relieves load on RDS
- Cache must have an **invalidation strategy** to keep data fresh

### Pattern 2: User Session Store
```
User logs in → App writes session to ElastiCache
                │
User hits different App instance
                │
                ▼
App reads session from ElastiCache → User stays logged in ✅
```
- Makes app **stateless** — any instance can serve any user
- Session persists across instances

---

## 26. Redis vs Memcached

| Feature | Redis | Memcached |
|---|---|---|
| Data structures | Strings, Hashes, Lists, Sets, Sorted Sets, Bitmaps, HyperLogLog, Streams | Simple key-value |
| Multi-AZ with failover | ✅ Yes | ❌ No |
| Read replicas | ✅ Yes (scale reads) | ❌ No |
| Data persistence | ✅ AOF / RDB | ❌ No persistence |
| Backup & restore | ✅ Yes | ❌ No |
| Pub/Sub messaging | ✅ Yes | ❌ No |
| Lua scripting | ✅ Yes | ❌ No |
| Geospatial support | ✅ Yes | ❌ No |
| Multi-threaded | ❌ Single-threaded | ✅ Multi-threaded |
| Horizontal sharding | ✅ Cluster mode | ✅ Native sharding |
| Use case | Rich data types, persistence, HA | Simple cache, max throughput |

> 🔑 **Use Redis** when you need durability, replication, advanced data types.
> 🔑 **Use Memcached** when you need a pure, simple, high-performance cache with sharding.

---

## 27. Cache Security in ElastiCache

### Redis:
- Supports **Redis AUTH** (username/password)
- In-transit encryption with **SSL/TLS**
- At-rest encryption with **KMS**
- IAM policies for AWS-level API calls only (not data-level)

### Memcached:
- Supports **SASL-based authentication**
- No built-in encryption support (use VPC for network isolation)

### Network Security (Both):
- Deploy inside a **VPC**
- Use **Security Groups** to restrict access
- ElastiCache is NOT publicly accessible by default

```
App (in VPC) → SG allows port 6379 → ElastiCache Redis
```

---

## 28. Patterns for ElastiCache

### Lazy Loading (Cache-Aside):
```
1. App requests data
2. Check cache → Cache HIT → return data ✅
3. Cache MISS → fetch from DB → store in cache → return data
```
- Pros: Only cache what's requested, resilient to cache failures
- Cons: Cache miss = 3 round trips (slow first request), stale data possible

### Write Through:
```
1. App writes to DB
2. Simultaneously writes to cache
```
- Pros: Cache always up-to-date, no stale data
- Cons: Write penalty (2 writes per operation), cache churn (write data that's never read)

### Session Caching:
- Store user sessions in Redis
- Stateless application tier

### TTL (Time-To-Live):
- Set expiry on all cache keys to prevent stale data buildup
- Apply to Lazy Loading and Write Through

---

## 29. Redis Use Cases

| Use Case | How Redis Helps |
|---|---|
| **Gaming Leaderboards** | Sorted Sets — unique, real-time ranking |
| **Session Store** | Fast key-value with TTL |
| **Real-time analytics** | Counters, HyperLogLog for unique counts |
| **Pub/Sub messaging** | Event broadcasting between services |
| **Rate limiting** | Atomic increment with TTL |
| **Queues / Task Lists** | Redis Lists (LPUSH / BRPOP) |
| **Geospatial queries** | Find nearby locations with GEODIST/GEORADIUS |
| **Caching DB queries** | Sub-millisecond reads, reduce RDS load |

### Gaming Leaderboard (Sorted Sets) Example:
```
ZADD leaderboard 1500 "PlayerA"
ZADD leaderboard 2300 "PlayerB"
ZADD leaderboard 900  "PlayerC"

ZREVRANGE leaderboard 0 -1 WITHSCORES
→ PlayerB: 2300
→ PlayerA: 1500
→ PlayerC: 900
```

Redis Sorted Sets guarantee **uniqueness** and **real-time ordering** with O(log N) operations.

---

---

# Quick Summary Table

| Service/Concept | One-liner |
|---|---|
| RDS | Managed relational DB (MySQL, PostgreSQL, etc.) |
| RDS Auto Scaling | Auto-grows storage when running low |
| Read Replicas | Async copies for read scaling (up to 15) |
| Multi-AZ | Sync standby in another AZ for DR |
| RDS Custom | OS-level access for Oracle/SQL Server |
| Aurora | AWS-native DB, 5x MySQL perf, 6 copies across 3 AZs |
| Aurora HA | 6 copies, auto-failover < 30s, 15 replicas |
| Aurora Serverless | Auto scale capacity, pay per second |
| Global Aurora | Cross-region, < 1s replication lag |
| Aurora ML | SQL → SageMaker/Comprehend predictions |
| Babelfish | Run SQL Server T-SQL on Aurora PostgreSQL |
| RDS Backup | Automated daily + 5-min transaction logs |
| Aurora Backup | Continuous, cannot disable |
| Aurora Cloning | Instant DB clone using copy-on-write |
| RDS Security | KMS, TLS, IAM auth, VPC, SGs |
| RDS Proxy | Connection pooling, faster failover, IAM auth |
| ElastiCache | Managed Redis/Memcached in-memory cache |
| DB Cache Pattern | Cache DB results, reduce RDS load |
| Session Store Pattern | Stateless apps with shared sessions |
| Redis | Rich data types, persistence, HA, pub/sub |
| Memcached | Simple cache, multi-threaded, no persistence |
| Cache Security | Redis AUTH + TLS, VPC isolation |
| Lazy Loading | Cache on miss — can serve stale data |
| Write Through | Write to cache on every DB write |
| Redis Sorted Sets | Real-time leaderboards |
