# **Task 1: System Monitoring Setup** 🚀  

This plan ensures **continuous system monitoring**, **log collection**, and **reporting** to meet the task requirements.  

## **🛠️ Step 1: Install Monitoring Tools**  
Install `htop` and `nmon` for real-time monitoring:  
```bash
sudo apt update && sudo apt install -y htop nmon
```
- Run `htop` for interactive CPU & memory usage monitoring.  

---

## **📝 Step 2: Set Up Automated System Logging using cron jobs**  

## Installation Steps

### 1. Create Log Directories

The log files will be stored in their respective directories. Create the directories first:

```bash
sudo mkdir -p /var/log/disk_usage
sudo mkdir -p /var/log/process_usage
sudo mkdir -p /var/log/top_processes
sudo mkdir -p /var/log/memory_usage
```

### 2. Create the Logging Scripts

Next, create the four scripts to log system data.

#### a. **log_disk_usage.sh**

- Purpose: Logs disk usage to `/var/log/disk_usage/` every 5 minutes.
- The log file will be named with a timestamp.

- create file `/usr/local/bin//usr/local/bin/log_disk_usage.sh` with the below script

```bash
#!/bin/bash

# Directory for disk usage logs
LOG_DIR="/var/log/disk_usage"

# Ensure the directory exists
mkdir -p $LOG_DIR

# Store the current time
current_time=$(date "+%Y-%m-%d %H:%M:%S")

# Define log file with timestamp
LOG_FILE="$LOG_DIR/disk_usage_$current_time.log"

# Append disk usage to the log file
echo "🗂️ Disk Usage Report - $current_time" >> $LOG_FILE
df -h >> $LOG_FILE
echo "-----------------------------------" >> $LOG_FILE

# Remove entries older than 7 logs
find $LOG_DIR -type f -name "*.log" | sort | head -n -7 | xargs rm -f
```

#### b. **log_process_usage.sh**

- Purpose: Logs the top 10 processes sorted by memory usage to `/var/log/process_usage/` every 10 minutes.

- create file `/usr/local/bin/log_process_usage.sh` with the below script
```bash
#!/bin/bash

# Directory for process usage logs
LOG_DIR="/var/log/process_usage"

# Ensure the directory exists
mkdir -p $LOG_DIR

# Store the current time
current_time=$(date "+%Y-%m-%d %H:%M:%S")

# Define log file with timestamp
LOG_FILE="$LOG_DIR/process_usage_$current_time.log"

# Append process usage to the log file
echo "⚙️ Process Usage Report - $current_time" >> $LOG_FILE
ps aux --sort=-%mem | head -10 >> $LOG_FILE
echo "-----------------------------------" >> $LOG_FILE

# Remove entries older than 7 logs
find $LOG_DIR -type f -name "*.log" | sort | head -n -7 | xargs rm -f
```

#### c. **log_top_processes.sh**

- Purpose: Logs the top 15 CPU-consuming processes to `/var/log/top_processes/` every 15 minutes.

- create file `/usr/local/bin/log_top_processes.sh` with the below script
```bash
#!/bin/bash

# Directory for top processes logs
LOG_DIR="/var/log/top_processes"

# Ensure the directory exists
mkdir -p $LOG_DIR

# Store the current time
current_time=$(date "+%Y-%m-%d %H:%M:%S")

# Define log file with timestamp
LOG_FILE="$LOG_DIR/top_processes_$current_time.log"

# Append top processes to the log file
echo "🚨 High Resource Usage Report - $current_time" >> $LOG_FILE
top -b -o +%CPU | head -15 >> $LOG_FILE
echo "-----------------------------------" >> $LOG_FILE

# Remove entries older than 7 logs
find $LOG_DIR -type f -name "*.log" | sort | head -n -7 | xargs rm -f
```
#### d. **log_memory_usage.sh**

- purpose: Logs memory usage statistics to /var/log/memory_usage/ every 1 minutes.

- create file `/usr/local/bin/log_memory_usage.sh` with the below script
```bash
#!/bin/bash

# Directory for memory usage logs
LOG_DIR="/var/log/memory_usage"

# Ensure the directory exists
mkdir -p $LOG_DIR

# Store the current time
current_time=$(date "+%Y-%m-%d %H:%M:%S")

# Define log file with timestamp
LOG_FILE="$LOG_DIR/memory_usage_$current_time.log"

# Append memory usage to the log file
echo "🧠 Memory Usage Report - $current_time" >> $LOG_FILE
free -h >> $LOG_FILE
echo "-----------------------------------" >> $LOG_FILE

# Remove entries older than 7 logs
find $LOG_DIR -type f -name "*.log" | sort | head -n -7 | xargs rm -f
```
### 3. Make Scripts Executable

Once the scripts are created, ensure they are executable:

```bash
sudo chmod +x /usr/local/bin/log_disk_usage.sh
sudo chmod +x /usr/local/bin/log_process_usage.sh
sudo chmod +x /usr/local/bin/log_top_processes.sh
sudo chmod +x /usr/local/bin/log_memory_usage.sh
```

### 4. Set Up Cron Jobs

To schedule the scripts to run automatically, we will configure cron jobs.

1. Open the crontab file:

   ```bash
   crontab -e
   ```

2. Add the following lines to the crontab file to run the scripts at the desired intervals:

```bash
# Disk Usage every 5 minutes
* * * * * /usr/local/bin/log_disk_usage.sh

# Process Usage every 10 minutes
* * * * * /usr/local/bin/log_process_usage.sh

# Top Processes every 15 minutes
* * * * * /usr/local/bin/log_top_processes.sh

# Memory Usage every 15 minutes
*/15 * * * * /usr/local/bin/log_memory_usage.sh
```
## crontab jobs Screenshot:
![image](https://github.com/user-attachments/assets/808f69d0-73c0-4fc2-8139-71bc973dfc36)

### 5. Test Cron Jobs

To verify that the cron jobs
## Disk Usage Logs:
![image](https://github.com/user-attachments/assets/12cc1348-c94b-4c6d-b06e-fc56c23b70ea)
## 7 numbers retention:
![image](https://github.com/user-attachments/assets/9769cc2e-a118-4272-af72-848e47859171)
-------------------------------------------------------------------------------------------
## Process usage
![image](https://github.com/user-attachments/assets/7b3eecb8-16f5-400b-bb17-06128df60548)
## 7 numbers retention:
![image](https://github.com/user-attachments/assets/365cb9b4-aa0c-4e45-99da-018c5407c541)
-------------------------------------------------------------------------------------------
## Top Processes Usage
![image](https://github.com/user-attachments/assets/4c499147-42d9-424b-9e8e-5060953d8e1d)
## 7 numbers retention:
![image](https://github.com/user-attachments/assets/8d7d9cb1-48f3-424a-8e8d-02c3a3dcc06a)
-------------------------------------------------------------------------------------------
## Memory Usage log
![image](https://github.com/user-attachments/assets/59bf5772-8378-47e1-9a76-183f6944ead7)
## 7 numbers retention:
![image](https://github.com/user-attachments/assets/0e71d058-9e83-4a3e-a6b1-4f829e12dc3c)
-------------------------------------------------------------------------------------------


---

## ✅ **Task 2: User Management and Access Control**

### 🎯 **Objective**

Create secure user accounts for Sarah and Mike with isolated directories and enforce a strong password policy.

---

### 🔧 **Implementation Steps**

#### ✅ 1. **Create User Accounts with Secure Passwords**

```bash
sudo adduser sarah
sudo passwd sarah  # Use a strong password

sudo adduser mike
sudo passwd mike  # Use a strong password
```

📌 **Tip**: Use a strong password (uppercase, lowercase, number, special character, at least 8 characters).

---

#### ✅ 2. **Create Isolated Workspace Directories**

```bash
# For Sarah
sudo mkdir -p /home/sarah/workspace
sudo chown sarah:sarah /home/sarah/workspace
sudo chmod 700 /home/sarah/workspace

# For Mike
sudo mkdir -p /home/mike/workspace
sudo chown mike:mike /home/mike/workspace
sudo chmod 700 /home/mike/workspace
```
📌 `chmod 700` ensures only the user can access the folder (read/write/execute).

### 📸 **Screenshots**

1. `cat /etc/passwd | grep -E 'sarah|mike'` – Confirm user creation.
   
![image](https://github.com/user-attachments/assets/e20c9031-6cb3-4875-aea0-08458c9400a3)

2. `ls -ld /home/sarah/workspace` and `ls -ld /home/mike/workspace` – Confirm permissions.

![image](https://github.com/user-attachments/assets/8fa4b1f8-b233-4090-b96e-9df5652dabdc)



---

#### ✅ 3. **Implement Password Expiration & Complexity Policy**

##### **Edit the Password Policy File: `/etc/login.defs`**

```bash
sudo nano /etc/login.defs
```

Make sure these lines are set:

```bash
PASS_MAX_DAYS   30
PASS_MIN_DAYS   1
PASS_WARN_AGE   7
```

📌 This means:

* Password expires every **30 days**
* Users can’t change it again within **1 day**
* Warning appears **7 days** before expiry

##### **Enforce Password Complexity (Ubuntu/Debian-based)**

Install PAM password quality module:

```bash
sudo apt install libpam-pwquality
```

Edit PAM configuration:

```bash
sudo nano /etc/pam.d/common-password
```

Ensure the following line (or similar) is present and **uncommented**:

```bash
password requisite pam_pwquality.so retry=3 minlen=8 ucredit=-1 lcredit=-1 dcredit=-1 ocredit=-1
```

📌 **Meaning**:

* Minimum 8 characters
* At least one uppercase (`ucredit=-1`)
* At least one lowercase (`lcredit=-1`)
* At least one digit (`dcredit=-1`)
* At least one special character (`ocredit=-1`)

---

#### ✅ 4. **Force Password Expiry for Initial Login**

```bash
sudo chage -d 0 sarah
sudo chage -d 0 mike
```

📌 This forces them to change their password at first login.

### 📸 **Screenshots**
1. `chage -l sarah` and `chage -l mike` – Confirm password expiry settings.

![image](https://github.com/user-attachments/assets/9a97e712-8bab-4a83-9ae2-665011e6943a)

![image](https://github.com/user-attachments/assets/9d363d0f-465e-4372-97fc-fdc2a01795f3)


2. Snippet of `/etc/login.defs` and `/etc/pam.d/common-password` edits.
<Picture>

![image](https://github.com/user-attachments/assets/1d38df0b-7b36-429d-8ed2-29dcd405eb68)

---


Perfect, Jidendiran! Let's now proceed with **✅ Task 3: Backup Configuration for Web Servers**, which carries **20 marks** — the highest weight in the assignment.

---

## ✅ **Task 3: Backup Configuration for Web Servers**

### 🎯 **Objective**

Automate **weekly backups** (every Tuesday at 12:00 AM) for:

* **Sarah's Apache server**
* **Mike's Nginx server**

Each should:

* Back up config and document root
* Save backups to `/backups/`
* Use filename format: `apache_backup_YYYY-MM-DD.tar.gz` and `nginx_backup_YYYY-MM-DD.tar.gz`
* Verify backup integrity by listing contents
* Log each run

---

### 🔧 **Step-by-Step Implementation**

---

### ✅ 1. **Create Backup Directory**

```bash
sudo mkdir -p /backups
sudo chmod 755 /backups
```

---

### ✅ 2. **Create Backup Scripts**

#### 📁 **Sarah’s Apache Backup Script**

📍 File: `/home/sarah/apache_backup.sh`

```bash
#!/bin/bash

DATE=$(date +%F)
BACKUP_NAME="apache_backup_${DATE}.tar.gz"
BACKUP_PATH="/backups/${BACKUP_NAME}"
LOG_FILE="/backups/apache_backup_${DATE}.log"

# Backup Apache config and HTML root
tar -czvf "$BACKUP_PATH" /etc/httpd/ /var/www/html/ > "$LOG_FILE" 2>&1

# Log verification
echo "Backup file: $BACKUP_PATH" >> "$LOG_FILE"
echo "Verifying backup contents:" >> "$LOG_FILE"
tar -tzf "$BACKUP_PATH" >> "$LOG_FILE"

echo "Backup completed for Apache on $(date)" >> "$LOG_FILE"
```

---

#### 📁 **Mike’s Nginx Backup Script**

📍 File: `/home/mike/nginx_backup.sh`

```bash
#!/bin/bash

DATE=$(date +%F)
BACKUP_NAME="nginx_backup_${DATE}.tar.gz"
BACKUP_PATH="/backups/${BACKUP_NAME}"
LOG_FILE="/backups/nginx_backup_${DATE}.log"

# Backup Nginx config and HTML root
tar -czvf "$BACKUP_PATH" /etc/nginx/ /usr/share/nginx/html/ > "$LOG_FILE" 2>&1

# Log verification
echo "Backup file: $BACKUP_PATH" >> "$LOG_FILE"
echo "Verifying backup contents:" >> "$LOG_FILE"
tar -tzf "$BACKUP_PATH" >> "$LOG_FILE"

echo "Backup completed for Nginx on $(date)" >> "$LOG_FILE"
```

---

### ✅ 3. **Set Permissions and Make Scripts Executable**

```bash
sudo chmod +x /home/sarah/apache_backup.sh
sudo chmod +x /home/mike/nginx_backup.sh
sudo chown sarah:sarah /home/sarah/apache_backup.sh
sudo chown mike:mike /home/mike/nginx_backup.sh
```

---

### ✅ 4. **Create Cron Jobs to Run Every Tuesday at 12:00 AM**

Run:

```bash
sudo crontab -u sarah -e
```

➡️ Add this line:

```
0 0 * * 2 /home/sarah/apache_backup.sh
```

Then run:

```bash
sudo crontab -u mike -e
```

➡️ Add this line:

```
0 0 * * 2 /home/mike/nginx_backup.sh
```

---

### ✅ 5. **Verification After First Run (or Manual Test)**

You can manually run the scripts to test:

```bash
sudo -u sarah bash /home/sarah/apache_backup.sh
sudo -u mike bash /home/mike/nginx_backup.sh
```

Then verify:

```bash
ls -lh /backups/
cat /backups/apache_backup_YYYY-MM-DD.log
cat /backups/nginx_backup_YYYY-MM-DD.log
```

---

### 📸 **Screenshots**

1. Cron job entries (`crontab -l -u sarah`, `crontab -l -u mike`)

![image](https://github.com/user-attachments/assets/64c7ec04-555d-44ac-a7b1-05d78e362fb2)

![image](https://github.com/user-attachments/assets/cca763e4-3dd6-437e-8c9e-02b6dfb402c8)

2. Files in `/backups/` showing naming convention

![image](https://github.com/user-attachments/assets/79bf6495-24df-4438-9e50-fcef4da0db53)

3. Log file contents showing successful backup and verification

![image](https://github.com/user-attachments/assets/d8736653-9bfd-40b5-abf6-c9fb07b50a37)

![image](https://github.com/user-attachments/assets/a82bf77d-d820-4a9d-b1c0-2a98281abb7a)


4. Script files with code

![image](https://github.com/user-attachments/assets/09797393-23b1-41cd-8ed1-dcac52345087)

![image](https://github.com/user-attachments/assets/e59a8447-455c-449d-9ea4-dcdcc717d75a)


5. Manual test outputs (optional but useful)

![image](https://github.com/user-attachments/assets/3a1657ed-d94c-4937-8fc3-4f4e1fe67165)

![image](https://github.com/user-attachments/assets/d80ae836-75a5-4b83-9529-8ff1bdcb8872)

---

