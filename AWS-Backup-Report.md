# 🚀 Project Guide Using Amazon Linux 2
## ✅ 1. Launch EC2 Instance (Amazon Linux 2)
Steps:
1. Go to the AWS Console > EC2 > Launch Instance.
2. Name: BackupProject-EC2
3. AMI: Choose Amazon Linux 2 (64-bit x86)
4. Instance Type: t2.micro (Free tier eligible)
5. Key Pair: Create or use existing (download .pem file).
6. Network Settings:
  - Allow SSH (port 22) and HTTP (port 80).
7. Click Launch Instance and wait for "Running" status.

### 🔧 Connect to EC2 and Install Web Server:

1. Open terminal:
```
ssh -i /path/to/your-key.pem ec2-user@your-ec2-public-ip
```
2. Install and start Apache server:
```
sudo yum update -y
sudo yum install -y httpd
sudo service start httpd
sudo systemctl enable httpd
```
3. Add sample HTML page:
```
echo "<h1>Welcome to the World of Apache</h1>" | sudo tee /var/www/html/index.html
```
4. In your browser, open:
```
http://<your-ec2-public-ip>
```
- You should see your test page.
- Allow http 80 in Security Groups.

## ✅ 2. Launch RDS MySQL Instance
Steps:
1. Go to RDS > Databases > Create database
2. Select:
  - Standard Create
  - Engine: MySQL
  - Version: Default
  - Instance type: db.t3.micro
  - DB identifier: backupdb
  - Username: admin, Password: choose a secure one
  - Public access: Enable (or connect via EC2)
3. Click Create database
### Connect & Insert Sample Data (Optional):
1. From EC2, install MySQL client:
```
sudo yum install -y mysql
```
2. Connect:
```
mysql -h <rds-endpoint> -u admin -p
```
3. In MySQL shell:
```
create database student;
use student;
create table info (id int, name varchar(100));
insert into info values(1, "Ajay");
select * from info;
```
## ✅ 3. Setup AWS Backup
### 🔐 Create a Backup Vault
1. Go to AWS Backup > Backup vaults > Create backup vault
2. Name: BackupVault-Project
3. Use default settings (encryption can stay default)
4. Click Create

### 🧾 Create Backup Plan
1. Go to Backup plans > Create backup plan
2. Select Build a new plan
  - Name: ProjectBackupPlan
  - Add rule:
    - Frequency: Daily
    - Retention: 7 days
    - Start time: Leave default
3. Select the vault: BackupVault-Project
4. Create the plan.

### 🏷️ Assign Resources (Tagging or ID)
Use Tags:
1. Go to EC2 and RDS instances:
    - Add tag: Key=Backup, Value=Yes

2. Go back to Backup Plan > Assign Resources
  - Name: ResourceAssignment-1
  - Assign by tag: Backup=Yes
## ✅ 4. Validate Backup
### Start On-Demand Backup
1. Go to Backup plans > Your Plan > Actions > Start on-demand backup
### ▶️ A. Trigger On-Demand Backup
1. In the AWS Backup console, go to:
  - Backup plans > BackupPlan-Project
  - Click the plan name: BackupPlan-Project
2. Inside the plan, click on "Create on-demand backup" (blue button on the right side).
3. In the Create on-demand backup dialog:
  - Resource type: Select EC2 (first do EC2, then repeat for RDS)
  - Resource ID: Choose your EC2 instance
  - Backup vault: Choose Default or your custom vault (BackupVault-Project)
  - Click "Create on-demand backup"
4. Repeat the same for RDS:
  - Choose RDS as resource type
  - Select your RDS instance
  - Use the same vault
  - Create backup
### ⏳ B. Monitor Backup Job Status
1. Go to AWS Backup > Jobs from the left menu.
2. Wait a few minutes and refresh.
3. Look for "Completed" status for both EC2 and RDS backups.
### 💾 C. View Recovery Points
1. Go to Backup vaults > Select your vault (e.g., BackupVault-Project).
2. Click on the vault name.
3. You should see Recovery points for both EC2 and RDS.

### Verify Backup Completion:
1. Go to Backup jobs → Wait for status: Completed
2. Go to Backup vaults > BackupVault-Project
   - Check Recovery points for both EC2 and RDS
