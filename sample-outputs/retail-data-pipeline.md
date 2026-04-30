## 🏪 Sample Output 3 — Retail Sales Data Pipeline & Executive Dashboard

## Input

| Field | Value |
|---|---|
| **Project Description** | A retail company wants to collect sales data from 200 stores daily, process it overnight, and generate a dashboard report every morning for executives. |
| **Scale** | Medium |
| **Budget** | Medium |
| **Priority** | Reliability |

---

## Architecture Generated

| Layer | Service | Purpose |
|---|---|---|
| Ingestion | S3 + AWS Transfer Family (SFTP) | 200 stores upload daily CSV files |
| Queueing | SQS + Dead Letter Queue | Decouple uploads from processing; capture failures |
| ETL | AWS Glue + Glue Data Quality | Nightly transformation, validation, anomaly flagging |
| Orchestration | AWS Step Functions | Retry logic, state machine, failure recovery |
| Scheduling | Amazon EventBridge | Trigger pipeline nightly at 11 PM |
| Warehouse | Amazon Redshift Serverless | Analytical queries, historical trends |
| Ad-hoc | Amazon Athena | Pay-per-query analysis directly on S3 |
| Dashboard | Amazon QuickSight (SPICE) | Pre-cached executive dashboard |
| Security | KMS + IAM + Secrets Manager | Encryption, least privilege, credential rotation |
| Monitoring | CloudWatch + CloudTrail | Alarms, audit logs, pipeline health |

---

## Estimated Monthly Cost

~$465 – $895/month

| Service | Estimated Monthly Cost |
|---|---|
| AWS Transfer Family (SFTP) | ~$50–80 |
| S3 (Raw + Curated + Logs) | ~$30–60 |
| AWS Glue (ETL Jobs) | ~$80–150 |
| Step Functions | ~$5–15 |
| Amazon Redshift Serverless | ~$150–300 |
| Amazon QuickSight (10 users) | ~$100–200 |
| CloudWatch + CloudTrail | ~$30–50 |
| Misc (KMS, SNS, SQS, Lambda) | ~$20–40 |
| **Total** | **~$465–895/month** |

---

## Key Reliability Features

- **Step Functions auto-retry** with exponential backoff on every pipeline step
- **S3 Cross-Region Replication** for disaster recovery of all raw and curated data
- **Dead Letter Queue (DLQ)** captures any failed SQS messages for investigation
- **CloudWatch alarm** fires if fewer than 190 of 200 stores upload by 10 PM
- **QuickSight SPICE** pre-caches the dataset overnight — dashboard loads in seconds at 6 AM
- **Redshift cross-region snapshots** every 8 hours for warehouse disaster recovery
- **S3 Versioning + MFA Delete** protects against accidental file overwrites

---

## Pipeline Architecture

```
200 Stores
    │
    ▼ (SFTP / Direct Upload)
AWS Transfer Family
    │
    ▼
S3 Raw Bucket ──── S3 Event ──► SQS ──► Dead Letter Queue
    │
    │ (EventBridge Scheduler @ 11 PM)
    ▼
AWS Step Functions (Orchestrator)
    │
    ├── AWS Glue ETL + Data Quality
    │       │
    │       ▼
    │   S3 Curated Bucket (Parquet, Partitioned)
    │       │
    │       ▼
    │   Amazon Redshift Serverless
    │       │
    ├── Lambda (Dashboard Refresh Trigger)
    │       │
    │       ▼
    │   Amazon QuickSight (SPICE Refresh)
    │       │
    └── SNS → "Dashboard Ready" Email to Executives

    Monitoring: CloudWatch Alarms + CloudTrail throughout
    Security:   KMS + IAM + VPC + Secrets Manager
```

---

## Key Tradeoff to Know

> Redshift Serverless is the largest cost driver (~$150–300/month). If budgets tighten, **Amazon Athena + S3** can replace Redshift entirely, offering a
> pay-per-query model at ~$5/TB scanned — with no warehouse infrastructure to manage.
