## ☁️ AWS Cloud Architecture Advisor

> An AI-powered app built on Amazon Bedrock (PartyRock) that generates tailored AWS architecture recommendations from plain-English project descriptions — in seconds.

## 🚀 Try It Live
🔗 [Open the App on PartyRock](https://partyrock.aws/u/daminip/r8ZZPVj1pD/AWS-Cloud-Architecture-Advisor)
> No AWS account or credit card required.

---

## 💡 What It Does

Paste in your project idea, choose your scale, budget, and priority — and the app instantly generates:

- ✅ Recommended AWS services with explanations
- ✅ Estimated monthly cost range
- ✅ Architecture rationale tailored to your constraints
- ✅ Key tradeoffs and warnings to consider

---

## 🧩 How to Use It

1. Describe your project in plain English
2. Select your expected traffic scale (Low / Medium / High)
3. Choose your budget range (Small / Medium / Large)
4. Pick your top priority (Cost / Performance / Reliability)
5. Hit **Generate** and get your architecture plan

---

## 🧪 Sample Test Cases

| # | Project Type | Scale | Budget | Priority |
|---|---|---|---|---|
| 1 | Food delivery app with real-time tracking | High | Medium | Performance |
| 2 | Internal HR portal for 50 employees | Low | Small | Cost |
| 3 | Retail sales data pipeline + executive dashboard | Medium | Medium | Reliability |

---

## 📋 Sample Output — Test 3: Retail Data Pipeline

**Input:**
> "A retail company wants to collect sales data from 200 stores daily,
> process it overnight, and generate a dashboard report every morning
> for executives."
> Scale: Medium | Budget: Medium | Priority: Reliability

**Architecture Generated:**

| Layer | Service | Purpose |
|---|---|---|
| Ingestion | S3 + AWS Transfer Family (SFTP) | 200 stores upload daily CSV files |
| Queueing | SQS + Dead Letter Queue | Decouple uploads from processing |
| ETL | AWS Glue + Glue Data Quality | Nightly transformation & validation |
| Orchestration | AWS Step Functions | Retry logic, state machine pipeline |
| Scheduling | Amazon EventBridge | Trigger pipeline nightly at 11 PM |
| Warehouse | Amazon Redshift Serverless | Analytical queries, historical trends |
| Ad-hoc | Amazon Athena | Pay-per-query analysis on S3 |
| Dashboard | Amazon QuickSight (SPICE) | Pre-cached executive dashboard |
| Security | KMS + IAM + Secrets Manager | Encryption, least privilege, rotation |
| Monitoring | CloudWatch + CloudTrail | Alarms, audit logs, pipeline health |

**Estimated Monthly Cost:** ~$465–$895/month

**Key Reliability Features:**
- Step Functions auto-retry on every pipeline step
- S3 Cross-Region Replication for disaster recovery
- CloudWatch alarm if fewer than 190/200 stores upload
- QuickSight SPICE pre-caches data before executives arrive

---

## 🛠️ Built With

- [Amazon Bedrock](https://aws.amazon.com/bedrock/) — Foundation model inference
- [PartyRock](https://partyrock.aws) — No-code AI app builder
- AWS service knowledge across compute, storage, data, and security domains

---

## 👤 About

Built as a portfolio project to demonstrate AWS solution design thinking
across multiple use cases — from serverless microservices to batch
data pipelines.
