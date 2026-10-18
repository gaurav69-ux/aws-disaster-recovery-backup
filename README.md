# AWS Disaster Recovery & Backup Strategy

## Overview
This project demonstrates an enterprise-grade Disaster Recovery (DR) and Backup strategy built on Amazon Web Services (AWS). It establishes automated backup schedules, point-in-time database recovery, continuous monitoring, and instant incident alerting to ensure application availability and data integrity during infrastructure outages.

---

## Technical Architecture

```text
[ PRIMARY REGION / PRODUCTION ENVIRONMENT ]
    │
    ├── EC2 Web / App Tier ────────┐
    └── RDS Database Tier ─────────┼───► AWS Backup Vault (Daily Automated Backups)
                                  │
[ MONITORING & ALERTING ]         │
    ├── Amazon CloudWatch ────────┴───► System & Backup Health Monitoring
    └── Amazon SNS ───────────────────► Real-Time Incident Notifications
                                  │
[ RECOVERY WORKFLOW ]             │
    └── Amazon RDS Snapshot ──────────► Point-in-Time Database Restoration
