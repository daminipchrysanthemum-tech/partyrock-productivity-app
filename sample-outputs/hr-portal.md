## 🏢 Sample Output 2 — Internal HR Time-Off Portal

## Input

| Field | Value |
|---|---|
| **Project Description** | A small company needs an internal HR portal where 50 employees can submit time-off requests and managers can approve them. |
| **Scale** | Low |
| **Budget** | Small |
| **Priority** | Cost |

---

## Architecture Generated

| Layer | Service | Key Reason |
|---|---|---|
| Frontend | S3 + CloudFront | Static hosting for pennies; free SSL via ACM |
| Auth | Amazon Cognito | Free tier covers 50,000 MAUs — 50 employees pay nothing |
| API | API Gateway (HTTP API) | 70% cheaper than REST API; free tier covers this workload |
| Compute | AWS Lambda | Free tier: 1M invocations/month — this app stays free |
| Database | RDS MySQL t3.micro | Simple relational model; ~$13/month flat |
| Secrets | AWS Secrets Manager | Stores and rotates DB credentials; ~$0.40/month |
| Email | Amazon SES | Free tier from Lambda: 62,000 emails/month |
| Scheduling | Amazon EventBridge | Nightly reminder emails; effectively $0 at this scale |
| Networking | VPC + Security Groups | RDS in private subnet; no public DB exposure |

---

## Estimated Monthly Cost

~$14 – $15/month total

---

## Cost Breakdown

| Service | Cost |
|---|---|
| S3 (static hosting) | ~$0.10 |
| CloudFront | ~$0.00 (free tier) |
| Cognito | ~$0.00 (free tier) |
| API Gateway (HTTP API) | ~$0.00 (free tier) |
| Lambda | ~$0.00 (free tier) |
| RDS MySQL t3.micro | ~$13.00 |
| Secrets Manager | ~$0.40 |
| SES | ~$0.00 (free tier) |
| EventBridge | ~$0.00 |
| **Total** | **~$14–15/month** |

---

## Key Cost Features

- Almost the entire stack runs within the **AWS free tier** at 50 users
- RDS on a single `t3.micro` in one AZ (no Multi-AZ needed for an internal tool)
- No load balancer (~$16/month saved vs. ALB) — Lambda handles routing
- No ElastiCache, WAF, or EKS — each would cost more than the entire rest of the stack

---

## What Was Deliberately Avoided

| Skipped Service | Reason |
|---|---|
| EC2 / ECS / EKS | Idle compute costs money 24/7; overkill for 50 users |
| ALB | ~$16/month minimum charge; unnecessary without EC2/ECS |
| Aurora | More expensive than RDS MySQL for simple, low-traffic use |
| ElastiCache | No caching needed; DB queries are trivial at this scale |
| WAF | Internal tool; adds cost with minimal benefit |

---

## Lambda API Endpoints

```
POST   /requests          → submitTimeOffRequest()
GET    /requests          → listMyRequests()
GET    /requests/all      → listAllRequests()   [Manager only]
PUT    /requests/{id}     → approveOrDenyRequest()
GET    /employees         → listEmployees()     [Manager only]
```

---

## Key Tradeoff to Know

> This architecture scales gracefully if the company grows:
> - **50 → 500 employees:** Upgrade RDS to `t3.small`, enable Multi-AZ
> - **500 → 5,000 employees:** Migrate to Aurora Serverless v2, add ElastiCache
> - **Public-facing:** Add WAF to API Gateway and CloudFront
