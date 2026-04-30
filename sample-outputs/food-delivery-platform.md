## 🍕 Sample Output 1 — Food Delivery Platform

## Input

| Field | Value |
|---|---|
| **Project Description** | A food delivery startup that needs a website and mobile app backend. Users can browse restaurants, place orders, and track delivery in real time. |
| **Scale** | High |
| **Budget** | Medium |
| **Priority** | Performance |

---

## Architecture Generated

| Layer | Service | Key Reason |
|---|---|---|
| Compute | ECS Fargate | Per-second billing, microservice isolation, no idle EC2 |
| Database | Aurora PostgreSQL Serverless v2 | Relational integrity, auto-scale, 6-way replication |
| Cache | ElastiCache Redis | Sub-ms reads, Geo queries for driver tracking |
| Real-Time | API Gateway WebSocket + AppSync | Persistent connections, GraphQL efficiency |
| Async Events | SQS + SNS + EventBridge | Decoupled order workflow, retry resilience |
| CDN / Storage | CloudFront + S3 | Edge delivery of images, offload origin |
| Auth | Cognito | Multi-user-type auth, edge token validation |
| Location | Amazon Location Service | 50–80% cheaper than Google Maps at scale |
| Observability | CloudWatch + X-Ray | Distributed tracing across microservices |
| Scaling | Application Auto Scaling | Scheduled + reactive scaling for traffic patterns |

---

## Estimated Monthly Cost

~$800 – $2,500 depending on traffic volume

---

## Key Performance Features

- **ECS Fargate** scales only the services under pressure (e.g., order service during lunch rush)
- **ElastiCache Redis** absorbs menu/restaurant read traffic, reducing Aurora queries by 60–80%
- **Aurora Serverless v2** scales from 0.5 to 128 ACUs — no idle capacity during off-peak hours
- **Scheduled Auto Scaling** pre-warms containers before 12 PM and 6 PM rush windows
- **API Gateway WebSocket** maintains persistent connections for real-time driver tracking

---

## Key Tradeoff to Know

> Lambda was considered for compute but rejected — real-time order tracking requires persistent connections and warm containers. Cold-start latency would directly hurt the performance priority. Fargate containers stay warm and handle stateful workflows cleanly.

---

## Microservices Breakdown (ECS Fargate)

```
API Gateway
      │
      ALB
      │
 ┌────┴────────────────────────────────┐
 │                                      │
[User/Auth Service]  [Restaurant/Menu]  [Order/Delivery Service]
       │                    │                      │
       └────────────────────┴──────────────────────┘
                            │
              ┌─────────────┼─────────────┐
              │             │             │
         Aurora PG    ElastiCache   DynamoDB
                            │
                    SQS / SNS / EventBridge
                            │
                     Lambda Functions
                  (Notifications, Payments)
```
