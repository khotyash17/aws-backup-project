# 📦 Where Are AWS Backups Stored?
AWS stores backups in a Backup Vault managed by the AWS Backup service.

## 🔐 Backup Vault
 - A logical container where your backup recovery points are securely stored.
 - Located regionally (e.g., Mumbai region).
 - You can use the default vault or create a custom vault (which you likely did: BackupVault-Project).
# 🔄 How to Use or Restore a Backup
## ✅ From AWS Backup Console:
You can restore backups from the console like this:
## 🔁 Restoring EC2 Instance from Backup
1. Go to AWS Backup > Backup vaults.
2. Click your vault (e.g., BackupVault-Project).
3. Click on a Recovery point for your EC2 instance.
4. Choose Restore.
5. Configure:
  - New instance name
  - Subnet, security group, key pair, etc.
6. Click Restore backup.
📌 This will launch a new EC2 instance with the same data as when the backup was taken.
## 🔁 Restoring RDS Database from Backup
1. Go to AWS Backup > Backup vaults.
2. Click the vault and select the RDS Recovery point.
3. Click Restore.
4. Configure:
  - DB instance ID
  - DB instance class
  - VPC/subnet group
5. Click Restore backup.
📌 This creates a new RDS instance from the backup snapshot.
## 🔐 Tip: Permissions
Make sure your IAM role/user has these permissions:
- backup:StartRestoreJob
- ec2:RunInstances
- rds:RestoreDBInstanceFromDBSnapshot

