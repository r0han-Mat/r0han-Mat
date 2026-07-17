<div align="center">

```
██████╗  ██████╗ ██╗  ██╗ █████╗ ███╗   ██╗
██╔══██╗██╔═══██╗██║  ██║██╔══██╗████╗  ██║
██████╔╝██║   ██║███████║███████║██╔██╗ ██║
██╔══██╗██║   ██║██╔══██║██╔══██║██║╚██╗██║
██║  ██║╚██████╔╝██║  ██║██║  ██║██║ ╚████║
╚═╝  ╚═╝ ╚═════╝ ╚═╝  ╚═╝╚═╝  ╚═╝╚═╝  ╚═══╝
```

### `> Backend Engineer · IT Undergrad · AWS Enthusiast`

*Building distributed systems that scale under pressure, fail gracefully, and never lose a transaction.*

<br/>

![Java](https://img.shields.io/badge/Java_17-ED8B00?style=for-the-badge&logo=openjdk&logoColor=white)
![Spring Boot](https://img.shields.io/badge/Spring_Boot-6DB33F?style=for-the-badge&logo=spring&logoColor=white)
![AWS](https://img.shields.io/badge/AWS-FF9900?style=for-the-badge&logo=amazonaws&logoColor=white)
![MySQL](https://img.shields.io/badge/MySQL-4479A1?style=for-the-badge&logo=mysql&logoColor=white)
![PostgreSQL](https://img.shields.io/badge/PostgreSQL-316192?style=for-the-badge&logo=postgresql&logoColor=white)
![Docker](https://img.shields.io/badge/Docker-2496ED?style=for-the-badge&logo=docker&logoColor=white)

</div>

---

## `$ whoami`

```yaml
name        : Rohan Mathur
role        : Backend Engineer · IT Undergrad
focus       : Microservices · Distributed Systems · Cloud Infrastructure
stack       : Java · Spring Boot · AWS · Docker · PostgreSQL · MySQL
currently   : Building BankFlow — a production-grade distributed banking backend
open_to     : Backend roles · Cloud internships · Collaborations
```

---

## `$ ls -la ./projects`

---

### ⬡ BankFlow — Distributed Banking Backend

> *3-service distributed backend with thread-safe transfers, fraud detection, and full AWS infrastructure*

```
Account Service :8080  →  Transaction Service :8081  →  SNS Fan-out  →  Worker Service :8082
       │                          │                           │
   REST API                 Saga Pattern              3 SQS Queues
   JPA + @Version           ReentrantLock             alerts · fraud · ledger
   MySQL/RDS                SNMP Polling              DLQ-backed retry
```

**Engineering highlights:**

| What | How |
|------|-----|
| Concurrent transfers | `ReentrantLock` per account ID — granular locking, not global serialization |
| DB-level safety | `@Version` optimistic locking — safe if service scales to multiple ECS tasks |
| Fraud detection | Deque-based sliding window — flags >5 tx/60s, flag-and-proceed (not block) |
| Resilience | Saga pattern — debit → credit → compensating rollback on failure |
| Stress tested | 20 threads · zero balance inconsistencies under concurrent load |

**AWS Infrastructure:**

```
VPC
├── Public Subnet    → ALB
├── Private Subnet   → ECS Fargate (Account + Transaction)
│                    → EC2 ASG (Worker) — scales on SQS queue depth
└── Private Subnet   → RDS MySQL — ECS-only access
                     → S3 receipts — 90-day Glacier lifecycle

SNS → SQS (alerts) + SQS (fraud) + SQS (ledger)   [each DLQ-backed]
Secrets Manager — zero credentials in env vars or task definitions
CloudWatch — DLQ depth · ECS health · RDS connections · ASG alarms
```

> **Stress test result:** 50 concurrent SQS messages → ASG scale-out confirmed in < 3 minutes

![Java](https://img.shields.io/badge/Java-ED8B00?style=flat-square&logo=openjdk&logoColor=white)
![Spring Boot](https://img.shields.io/badge/Spring_Boot-6DB33F?style=flat-square&logo=spring&logoColor=white)
![AWS ECS](https://img.shields.io/badge/ECS_Fargate-FF9900?style=flat-square&logo=amazonaws&logoColor=white)
![SNS](https://img.shields.io/badge/SNS%2FSQS-FF4F8B?style=flat-square&logo=amazonaws&logoColor=white)
![RDS](https://img.shields.io/badge/RDS_MySQL-4479A1?style=flat-square&logo=mysql&logoColor=white)
![Docker](https://img.shields.io/badge/Docker-2496ED?style=flat-square&logo=docker&logoColor=white)

---

### 📡 Network Device Monitoring Platform

> *Real SNMP metrics from a live virtual network — same protocol used by Cisco DNA Center and Prime Infrastructure*

```
Spring Boot App
      │
      ├── SNMP4J (UDP :161) ──► Vagrant-Gateway    192.168.56.10
      │                    ──► Vagrant-WebServer  192.168.56.11
      │                    ──► Vagrant-DBServer   192.168.56.12
      │
      ├── PostgreSQL (Docker) — devices · device_metrics · alerts
      └── Dashboard (localhost:8080) — Chart.js real-time graphs
```

**How it works:**

Every 30 seconds, `@Scheduled` polls all registered devices via SNMP GET requests against real OIDs:

```
MetricScheduler (fixedRate=30000ms)
    └── for each Device:
            CPU    → OID 1.3.6.1.4.1.2021.11.11.0   (idle% → usage = 100 - idle)
            Memory → OID 1.3.6.1.4.1.2021.4.5.0     (total RAM)
                   → OID 1.3.6.1.4.1.2021.4.11.0    (available RAM)
            Uptime → OID 1.3.6.1.2.1.1.3.0           (centiseconds)
            → saves DeviceMetric → AlertService.checkAndGenerateAlerts()
```

**Auto-alert engine:**

| Condition | Alert Type | Severity |
|-----------|------------|----------|
| CPU > 80% | `CPU_HIGH` | 🔴 CRITICAL |
| Memory > 75% | `MEMORY_HIGH` | 🟡 MEDIUM |
| Latency > 150ms | `LATENCY_HIGH` | 🔴 CRITICAL |

Alert lifecycle: `OPEN → ACKNOWLEDGED → RESOLVED`

![Java](https://img.shields.io/badge/Java_21-ED8B00?style=flat-square&logo=openjdk&logoColor=white)
![Spring Boot](https://img.shields.io/badge/Spring_Boot-6DB33F?style=flat-square&logo=spring&logoColor=white)
![PostgreSQL](https://img.shields.io/badge/PostgreSQL-316192?style=flat-square&logo=postgresql&logoColor=white)
![Vagrant](https://img.shields.io/badge/Vagrant-1563FF?style=flat-square&logo=vagrant&logoColor=white)
![SNMP](https://img.shields.io/badge/SNMP4J-FF6B35?style=flat-square&logoColor=white)
![Docker](https://img.shields.io/badge/Docker-2496ED?style=flat-square&logo=docker&logoColor=white)

---

### 🛒 Ecommerce Microservices

> *Three independent Spring Boot services with circuit-breaker protection and live cross-service data validation*

```
User Service ──────┐
                   ▼
Product Service ──► Order/Cart Service  (circuit-breaker wrapped on both calls)
                        │
                   Places order only after:
                   ✓ Fetching real-time price + stock from Product service
                   ✓ Confirming user exists in User service
                   ✗ Fails fast + recovers automatically if either dependency is down
```

Each service owns its own database schema (`userdb` · `product` · `order`) — no direct DB access across service boundaries, same as production.

![Spring Boot](https://img.shields.io/badge/Spring_Boot-6DB33F?style=flat-square&logo=spring&logoColor=white)
![PostgreSQL](https://img.shields.io/badge/PostgreSQL-316192?style=flat-square&logo=postgresql&logoColor=white)
![Docker](https://img.shields.io/badge/Docker-2496ED?style=flat-square&logo=docker&logoColor=white)
![Resilience4j](https://img.shields.io/badge/Resilience4j-Circuit_Breaker-6DB33F?style=flat-square)

---

## `$ cat skills.json`

```json
{
  "languages"  : ["Java 17/21", "SQL", "Bash"],
  "frameworks" : ["Spring Boot", "Spring Data JPA", "Hibernate", "Resilience4j"],
  "aws"        : ["ECS Fargate", "EC2 ASG", "RDS", "S3", "SNS", "SQS",
                  "ALB", "VPC", "Secrets Manager", "CloudWatch"],
  "databases"  : ["MySQL", "PostgreSQL", "RDS"],
  "infra"      : ["Docker", "Docker Compose", "Vagrant", "VirtualBox"],
  "patterns"   : ["Microservices", "Saga Pattern", "Event-Driven Architecture",
                  "DLQ Retry", "Circuit Breaker", "Sliding Window Detection"],
  "tools"      : ["Git", "Swagger/OpenAPI", "Chart.js", "SNMP4J"]
}
```

---

## `$ ping rohan`

<div align="center">

[![LinkedIn](https://img.shields.io/badge/LinkedIn-0077B5?style=for-the-badge&logo=linkedin&logoColor=white)](https://linkedin.com/in/YOUR_USERNAME)
[![GitHub](https://img.shields.io/badge/GitHub-100000?style=for-the-badge&logo=github&logoColor=white)](https://github.com/YOUR_USERNAME)
[![Email](https://img.shields.io/badge/Email-D14836?style=for-the-badge&logo=gmail&logoColor=white)](mailto:YOUR_EMAIL)

*Open to backend / cloud roles · internships · collaborations*

</div>

---

<div align="center">
<sub>Built with Java, Spring Boot, and too much CloudWatch. · Last deployed: 2026</sub>
</div>
