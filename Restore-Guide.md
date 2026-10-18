# AWS RDS Database Disaster Recovery & Restoration Runbook

## Overview
This runbook outlines the standard operating procedure (SOP) for restoring an Amazon RDS database from automated backups and snapshots in the event of data corruption, accidental deletion, or infrastructure failure.

---

## Prerequisites
* **AWS Console Access:** Appropriate IAM permissions to manage Amazon RDS and AWS Backup.
* **Target VPC & Security Groups:** Pre-configured VPC and security groups allowing inbound traffic on the database port (e.g., Port 3306 for MySQL or Port 5432 for PostgreSQL).
* **Application Configuration:** Knowledge of the application's environment variables to update the new database endpoint once restored.

---

## Step-by-Step Restoration Procedure

### Phase 1: Identify the Target Recovery Point
1. Log in to the **AWS Management Console**.
2. Navigate to the **Amazon RDS Console**.
3. In the left-hand navigation pane, click on **Snapshots** (or go to **Automated backups**).
4. Review the available snapshots and identify the correct point-in-time recovery (PITR) timestamp or manual snapshot created by **AWS Backup**.

### Phase 2: Restore the Database from Snapshot
1. Select the desired recovery snapshot.
2. Click on the **Actions** dropdown menu and select **Restore snapshot**.
3. Configure the restoration settings:
   * **DB instance identifier:** Enter a unique name for the new restored instance (e.g., `prod-db-restored-instance`).
   * **Instance configuration:** Choose the appropriate DB instance class (e.g., `db.t3.micro` for testing/free-tier).
   * **Connectivity:** 
     * Select the target **VPC**.
     * Choose the appropriate **Subnet group** (ensure it deploys into your private subnets).
     * Select the correct **VPC Security Group** to allow internal communication from your application backend EC2 instances.
   * **Database authentication:** Keep the existing master credentials or configure them as required.
4. Click **Restore DB instance**.

### Phase 3: Verify Database Health & Connectivity
1. Wait for the database status in the RDS console to change from `Creating` / `Restoring` to **Available**.
2. Note the new **Endpoint URL** of the restored database.
3. Securely connect to your backend EC2 instance via SSH and test connectivity to the new database endpoint:
   ```bash
   nc -zv <your-new-rds-endpoint> 3306
